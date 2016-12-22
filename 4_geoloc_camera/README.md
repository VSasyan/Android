# Géolocalisation et Appareil photos

Dans de nombreuse applications, connaître la position de l'utilisateur est déterminant.
Cela est directement géré par l'API Android, et il est donc très simple d'accéder à ces données via une application.

Il peut être intéressant de coupler cette fonction à la prise de photo (afin de profiter de photos géo-référencées).

## Géolocalisation

Nous allons créer une application simple affichant les coordonnées de l'utilisateur :

![Interface de localisation]( "Interface de localisation")

### Autorisation

Pour cela, il faut dans un premier temps que l'application est le droit d'utiliser la position de l'utilisateur. Cela se passe dans le fichier `AndroidManifest.xml`.

Il y a deux type de permission de localisation à demander :
* `ACCESS_COARSE_LOCATION` : pour une localisation approximative (via le WiFi et les antennes téléphoniques) ;
* `ACCESS_FINE_LOCATION` : pour une localisation précise (via GNSS) ;

La première permission est automatiquement requise avec la seconde.

Ajouter donc les lignes suivante dans la balise `manifest` du fichier XML :

```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <!-- Needed only if your app targets Android 5.0 (API level 21) or higher. -->
    <uses-feature android:name="android.hardware.location.gps" android:required="true" />
```

### Création du gestionaire de position

Dans un deuxième temps, il faut instancier un objet qui va gérer le suivi de position : le `LocationManager`.

Dans la fonction `OnCreate` de l'acivité, ajoutez la ligne :

```java
		LocationManager locationManager = (LocationManager)getSystemService(Context.LOCATION_SERVICE);
```

### Fournisseur de position

Comme l'a suggéré le paragraphe sur les autorisation, la localisation du téléphone peut venir de différentes sources. Et elles n'ont pas forcément la même qualité.

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

### Afficher la position sur une carte

On va modifier l'activité pour qu'elle puisse afficher une carte. Ainsi on pourra pointer la position de l'utilisateur sur une carte, c'est plus visuel !

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
