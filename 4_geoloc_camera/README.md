# Géolocalisation et Appareil photos

Dans de nombreuse applications, connaître la position de l'utilisateur est déterminant.
Cela est directement géré par l'API Android et il est donc très simple d'accéder à ces données via une application.

Il peut être intéressant de coupler cette fonction à la prise de photo (afin de créer des photos géo-référencées).


## Géolocalisation

Nous allons créer une application simple affichant les coordonnées de l'utilisateur :

![Interface de localisation]( "Interface de localisation")

### Autorisation

Pour cela, il faut dans un premier temps que l'application ait le droit d'**utiliser la position de l'utilisateur**. Cela se passe dans le fichier `AndroidManifest.xml`.

Il y a deux types de **permission** de localisation existant :
* `ACCESS_COARSE_LOCATION` : pour une localisation approximative (via le WiFi et les antennes téléphoniques) ;
* `ACCESS_FINE_LOCATION` : pour une localisation précise (via GNSS) ;

La première permission est automatiquement requise avec la seconde.

Ajouter donc les lignes suivante dans la balise `manifest` du fichier XML :

```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <!-- Needed only if your app targets Android 5.0 (API level 21) or higher. -->
    <uses-feature android:name="android.hardware.location.gps" android:required="true" />
```

On ne demande que la plus précise. Il y a deux lignes car l'a syntaxe de demande de permission change à partir de la version 21 de l'API d'Android.

### Création du gestionaire de position

Dans un deuxième temps, il faut instancier un objet qui va gérer le suivi de position : le `LocationManager`.

Dans la fonction `OnCreate` de l'activité, ajoutez la ligne :

```java
		LocationManager locationManager = (LocationManager)getSystemService(Context.LOCATION_SERVICE);
```

### Fournisseur de position

Comme l'a suggéré le paragraphe sur les permissions, la localisation du téléphone peut venir de différentes sources. Et elles n'ont pas forcément la même qualité !

L'API d'Android permet de sélectionner automatiquement les bonnes sources en utilisant des critères.

Pour aller plus vite, dans cet exercice, nous utiliserons uniquement une source, fixée en dur dans le code : `LocationManager.NETWORK_PROVIDER` (le réseau WiFi ou téléphone, donc).

### Écouteur de position

Lorsque la position du téléphone change, l'API d'Android va créer un événement. On va donc devoir créer un *écouteur de position* pour suivre la position de l’utilisateur. Encore une fois, on fait de la programmation événementielle.

L'écouteur peut-être anonyme ou un attribut de la classe d'activité. Pour cet exemple, nous allons créer un attribut :

```java
public class GeoActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_geo);

        // Init the location manager
        LocationManager locationManager = (LocationManager)getSystemService(Context.LOCATION_SERVICE);

        // Init the position eventListener
        try {
            locationManager.requestLocationUpdates(LocationManager.NETWORK_PROVIDER, 42000, 42, locationListener);
            // App is running
            Toast.makeText(GeoActivity.this, "Application lancée...", Toast.LENGTH_SHORT).show();
        }
        catch (SecurityException e) {
            // App does not has required permissions
            Toast.makeText(GeoActivity.this, "L'application n'a pas les autorisations requises...", Toast.LENGTH_LONG).show();
        }
    }

    // My private attribut which is the location listener
    private LocationListener locationListener = new LocationListener() {
        @Override
        public void onStatusChanged(String provider, int status, Bundle extras) {
        }

        @Override
        public void onProviderEnabled(String provider) {
        }

        @Override
        public void onProviderDisabled(String provider) {
        }

        @Override
        public void onLocationChanged(Location location) {
            // Here add the code to manage the location
        }

    };
}
```

Comme vous pouvez le voir, l'objet permet :
* de suivre le statut (la qualité) de la position (méthode `onStatusChanged`) ;
* de prévenir l'utilisateur lorsque le suivi de position est perdu/retrouvé (méthodes `onProviderEnabled` et `onProviderDisabled`) ;
* et surtout, de récupérer les coordonnées à chaque nouvelle position (méthode `onLocationChanged`).

Vous devez impérativement *implémenter* ces méthodes, par contre, elles peuvent rester vides : dans cet exercice on n'utilisera que la méthode `onLocationChanged`.

Je vous laisse consulter ici la documentation pour connaître les paramètres de la méthode [requestLocationUpdates](<https://developer.android.com/reference/android/location/LocationManager.html#requestLocationUpdates(java.lang.String,%20long,%20float,%20android.location.LocationListener)>). Utilisez des valeurs adaptées à l’exercice...

Intéressons nous à l'objet `Location` passé en paramètre de la méthode de récupération de la position. La [documentation](https://developer.android.com/reference/android/location/Location.html) nous donne deux les deux fonctions à utiliser :
* `double getLatitude()` ;
* `double getLongitude()`.

Insérez le code pour 
* afficher dans un TextView nommé "tv_last_position" la position au format : « Lat : 45.361797 ; Lon : 5.597899 : à : 10:42:38 » ;
* prévenir l'utilisateur avec un `Toast` que la position est actualisée.

Le Toast est un objet Android permettant d'afficher un court message à l'utilisateur :

```java
		Toast.makeText(GeoActivity.this, "Position mise à jour...", Toast.LENGTH_LONG).show();
```

Arguments :
* le contexte où afficher le toast (celui de l'activité) ;
* le message à afficher ;
* le temps d'affichage.

### La première version est terminée

Lancez l'application. Vérifiez que vous obtenez bien la position, qu'elle est actualisée de temps en temps et qu'un Toast s'affiche à ce moment.

## Affichage de la position sur une carte

On va modifier l'activité pour qu'elle puisse afficher une carte. Ainsi on pourra pointer la position de l'utilisateur sur une carte !

### Modification de la vue

Pour afficher la carte, il faut ajouter une balise `fragment` dans le fichier XML. Voici le code :

```xml
        <fragment xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:map="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:id="@+id/map"
            android:name="com.google.android.gms.maps.SupportMapFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            tools:context="fr.ign.vsasyan.geopicture.MapsActivity" />
```

Comme cet objet a l'attribut `android:layout_height="match_parent"` il va prendre tout l'espace. Pour structurer votre vue, vous devez ajouter une balise `LinearLayout` avec au moins un attribut `android:orientation="vertical"`. Allez regarder la documentation pour plus de détails... Vous devez ensuite déplacer les balises `TextView` et `fragment` dans cette balise.

Voilà le résultat attendu :

![Interface attendu après l'ajout de la carte](screens/gui_ajout_carte.png "Interface attendu après l'ajout de la carte")

### Modification structurelle de l'activité

Pour cela, il faut modifier notre activité pour qu'elle hérite de l'objet `FragmentActivity`, elle doit aussi implémenter l'interface `OnMapReadyCallback` :

```java
public class GeoActivity extends FragmentActivity implements OnMapReadyCallback {

    GoogleMap mMap;
    Marker mMarker;
    boolean isMapReady = false;

    // ...
}
```

On en profite pour ajouter trois attributs qui vont nous permettre de :
* stocker l'objet qui représente notre carte ;
* stocker l'objet qui représente le marqueur donnant la position de l'utilisateur ;
* dire si la carte est prête ou non.

Cela va nous obliger à implémenter la méthode `OnMapReadyCallback`. Cette méthode est un "callback". Elle va en fait être automatiquement exécutée lorsque que la carte sera chargée.

On va ajouter dans la méthode `onCreate` de l’activité les lignes suivantes :

```java
        // Obtain the SupportMapFragment and get notified when the map is ready to be used.
        SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map);
        mapFragment.getMapAsync(this);
```

On récupère le fragment de carte que l'on a ajouté dans la vue et on l'instancie en objet Java. C'est ce composant qui va afficher la carte.
La deuxième ligne permet d'initier l'affichage en précisant qu'elle méthode de callback il faut exécuter la carte est rattachée (`this`).


Il faut ensuite implémenter la fonction `OnMapReadyCallback` :

```java
    @Override
    public void onMapReady(GoogleMap googleMap) {
        mMap = googleMap;
        isMapReady = true;
    }
```

On stocke l'objet qui représente la carte et on passe le booléen `isMapReady` à `true`.

### Modification fonctionnelle de l'activité

Dans la fonction `` qui met à jour la position de l'utilisateur, nous allons ajouter un code qui actualise la position du marqueur :

```java
            if (isMapReady) {
                // Add a marker and move the camera
                LatLng position = new LatLng(lastLocation.getLatitude(), lastLocation.getLongitude());
                if (mMarker == null) {
                    mMarker = mMap.addMarker(new MarkerOptions().position(new LatLng(0,0)));
                }
                mMarker.setPosition(position);
                mMarker.setTitle(sdf.format(lastTime));
                mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(position, 14));
            }
```

// TODO

## Prise d'une photo

On souhaite que l'utilisateur puisse prendre une photo, et que l'on associe la dernière position connue à l'image avant de la sauvegarder sur l'appareil. On ouvrira une nouvelle activité, elle permettra à l'utilisateur de prendre une photo et de la sauvegarder.

### Utiliser l'appareil photos

Il faut demander les permissions permettant de prendre d'accéder à l'appareil photo et d'écrire sur l'appareil :

```xml
    <uses-permission android:name="android.hardware.camera" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <!-- Needed only if your app targets Android 5.0 (API level 21) or higher. -->
    <uses-feature android:name="android.hardware.camera" android:required="true" />
    <uses-feature android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:required="true" />
```

On doit ajouter à la vue une surface sur laquelle on peut afficher un aperçu.
