# Cours

## Présentation d'Android

[Voir la présentation en PDF](Les_bases_théoriques_d_Android.pdf)

## Organisation du code

### Fichier AndroidManifest

Le fichier contient les méta-données de l'application (nom, icône, etc) ainsi que la liste des activités de l'application et les permissions de l'application (connaître la position de l'utilisateur, accéder aux documents, prendre des photos, etc).

Localisation : `app/manifests/AndroidManifest.xml`

Exemple de code :

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="fr.ign.vsasyan.jourdelasemaine">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="Jour de la Semaine"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <!-- Aucune permission ici -->

        <activity android:name=".JourActivity">
            <!-- Une activité "Jour" -->
            <intent-filter>
                <!-- Qui est l'activité principale (lancé à démarrage de l’application) -->
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```

### Les activités

L'interface graphique est composée d'**activités** (qui sont les équivalents des pages d'un site internet) on parle également de **vue**.

A chaque activité est associée :
* un fichier XML décrivant les composants graphiques utilisés ;
* un fichier Java permettant de programmer des choses (actions, lancement de traitements, etc).

#### Exemple de partie XML pour l'activité « `Jour` »

Localisation : `app/res/layout/activity_jour.xml`

Code :

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Le fichier contient UNE première balise qui est un Layout (ici un RelativeLayout) -->
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_dice"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="fr.ign.vsasyan.jourdelasemaine.JourActivity" >

    <!-- Ensuite il y a un bouton -->
    <Button
        android:text="Afficher le Jour"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:id="@+id/b_afficher_jour"
        android:layout_alignParentRight="true"
        android:layout_alignParentEnd="true" />

    <!-- Puis une zone de texte -->
    <TextView
        android:id="@+id/tv_jour"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/b_afficher_jour">
    </ScrollView>

</RelativeLayout>
```

L'équivalent en HTML pourrait être :

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Jour de la Semaine</title>
    </head>

    <body>
        <button id="b_afficher_jour">Afficher le Jour</button>
        <textarea id="tv_jour"></textarea>
    </body>
</html>
```

#### Exemple de partie Java pour l'activité « `Coin` »

Localisation : `app/java/fr.ign.vsasyan.jourdelasemaine/JourActivity.java`

Code :

```java
package fr.ign.vsasyan.jourdelasemaine;

import ...;

public class JourActivity extends AppCompatActivity {

    // Éventuels attributs
    String jourDeLaSemaine;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_jour);

        // Autres instructions à exécuter à la création
    }

    // Éventuelles méthodes
    private void setJourDeLaSemaine() {
        this.jourDeLaSemaine = 'Lundi';
    }
}

```

C'est une classe comme une autre en Java, vous pouvez donc lui ajouter des attributs et des méthodes si besoin.

La méthode `onCreate` est la plus important, elle est exécutée au chargement de l'activité.

Elle contient deux lignes qui permettent de gérer l'historique d'affichage et l'affichage de la vue actuelle. **Votre code doit figurer après.**

#### Manipuler la vue

Tout comme le JavaScript vous permet de modifier une page HTML et d'effectuer des actions au clic de l'utilisateur, vous aller pouvoir modifier la vue et réagir aux actions de l'utilisateur depuis le code Java.

Dans un premier temps, il faudra instancier un objet Java pour chaque élément de votre vue que vous voulez utiliser. Il faut pour cela utiliser la méthode `findViewById` qui prend en paramètre un identifiant d'élément et retourne une vues (`View`).

Par exemple ici, il faudra instancier le bouton et le champs de texte :

```java
// Déclaration des variables Java
Button b_afficher_jour;
TextView tv_jour;

// Instanciation des élément en variable Java
b_afficher_jour = (Button)findViewById(R.id.b_afficher_jour);
tv_jour = (TextView)findViewById(R.id.tv_jour);
```

L'équivalent JavaScript serait :

```javascript
// Déclaration des variables
var b_afficher_jour;
var tv_jour;

// Récupération des éléments HTML
b_afficher_jour = document.getElementById('b_afficher_jour');
tv_jour = document.getElementById('tv_jour');
```

Une fois cela fait, on va vouloir effectuer une action quand l'utilisateur clique sur le bouton.

Comme en JavaScript, on va ajouter un écouteur d'événement au bouton :

```java
    // Déclaration de la variable d'écouteur d’événement
    View.OnClickListener eventListener;

    // Instanciation de l'écouteur d'événement : une classe avec une seule méthode "onClick" qui sera exécutée au clic (d'où le nom...)
    eventListener = new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            // La fonction à exécuter au clic

            Calendar calendrier = new GregorianCalendar();
            String jour = calendrier.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.LONG, Locale.getDefault());
            tv_jour.setText(jour);
        }
    };

    // Définition de l’écouteur d'événement précédemment crée comme étant l'écouteur du bouton
    b_button.setOnClickListener(eventListener);
```

L'équivalent en JS serait :

```javascript
    // Définition de la fonction à exécuter au clic
    function afficherJour() {
        var jour = new Date().toLocaleString('fr-fr', {  weekday: 'long' });
        tv_jour.innerHTML(jour);
    }
    
    // Ajout d'un écouteur au bouton associé à l'événement "click"
    b_afficher_jour.addEventListener("click", afficherJour);
```

Voici le code Java au complet :

```java
package fr.ign.vsasyan.jourdelasemaine;

import ...;

public class JourActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_jour);

        // Autres instructions à exécuter à la création : c'est donc évidement ici qu'on met notre code

        // Déclaration des variables Java
        Button b_afficher_jour;
        TextView tv_jour;

        // Instanciation des élément en variable Java
        b_afficher_jour = (Button)findViewById(R.id.b_afficher_jour);
        tv_jour = (TextView)findViewById(R.id.tv_jour);
    
        // Déclaration de la variable d'écouteur d’événement
        View.OnClickListener eventListener;

        // Instanciation de l'écouteur d'événement : une classe avec une seule méthode "onClick" qui sera exécutée au clic (d'où le nom...)
        eventListener = new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // La fonction à exécuter au clic

                Calendar calendrier = new GregorianCalendar();
                String jour = calendrier.getDisplayName(Calendar.DAY_OF_WEEK, Calendar.LONG, Locale.getDefault());
                tv_jour.setText(jour);
            }
        };

        // Définition de l’écouteur d'événement précédemment crée comme étant l'écouteur du bouton
        b_button.setOnClickListener(eventListener);
    }
}

```

Et l'équivalent en JS au complet :

```javascript
// Déclaration des variables
var b_afficher_jour;
var tv_jour;

// Récupération des éléments HTML
b_afficher_jour = document.getElementById('b_afficher_jour');
tv_jour = document.getElementById('tv_jour');

// Définition de la fonction à exécuter au clic
function afficherJour() {
    var jour = new Date().toLocaleString('fr-fr', {  weekday: 'long' });
    tv_jour.innerHTML(jour);
}

// Ajout d'un écouteur au bouton associé à l'événement "click"
b_afficher_jour.addEventListener("click", afficherJour);
```

### Les ressources

Ce sont des fichiers (en général XML mais cela peut également être des images, etc.) qui stockent tout ce qui n'est pas du code : les textes, les illustrations, mais également les vues...

Il y a une gestion automatique des langues, des différentes résolution de téléphones ou encore du mode portait ou paysage.

Exemple pour le fichier contenant les chaînes de caractères :

Localisation :
* Anglais : `app/res/values/strings.xml` (par défaut)
* Français : `app/res/values-fr/strings.xml`

Cela permet de traduire facilement l’application, en particulier quand c'est couplé au formatage des string en Java.

```xml
<!-- values/string.xml -->
<resources>
    <string name="app_name" translatable="false">MyApp</string>
    <string name="local">en</string>
    <string name="result">Your result is: %f</string>
</resources>
```

```xml
<!-- values-fr/string.xml -->
<resources>
    <string name="app_name" translatable="false">MyApp</string>
    <string name="local">fr</string>
    <string name="result">Votre score est : %f</string>
</resources>
```

```java
    Locale locale = new Locale(getResources().getString(R.string.local));
    String stringToFormat = getResources().getString(R.string.result));
    String formatedString = String.format(locale, stringToFormat, 42.42);
```

Qui va afficher :
* en anglais : `Your result is: 42.42`
* en français : `Votre score est : 42,42`

## Rappel 1 : le Java et les Objets Java

Java est un langage à **typage statique** (contrairement au JavaScript ou au PHP par exemple). Il faut donc déclarer les variables (donner le **type**) avant de les utiliser :

```java
    // Déclaration
    int unEntier;
    float unFlottant;
    String uneChaineDeCaracteres;

    // Utilisation
    unEntier = 40;
    unFlottant = 2.42;
    uneChaineDeCaracteres = "Je suis une chaîne de caractères...";

    // On peut également déclarer et utiliser en même temps :
    int unAutreEntier = 8;
```

Lorsque l'on utilise des Classes, il faut *toujours* **déclarer** et **instancier** la variable, avant de l'utiliser :

```java
    // Fichier : Chien.java

    // Définition de la classe Chien
    public class Chien {

        // Elle a un attribut nom qui est un String
        String nom;

        // Constructeur
        public Chien(String nom) {
            this.nom = nom;
        }

        // Méthode
        public aboyer() {
            System.out.println("{0} : « Wouaf, wouaf ! »".format(this.nom));
        }
    }
```

```java
    // Fichier : appli.java

    // Importation de la classe
    import Chien;

    // 1) je déclare la variable :
    Chien chien1;

    // 2) j'instancie ma variable :
    chien1 = new Chien("Milou");

    // 3) j'utilise ma variable :
    milou.aboyer();


    // Erreur type 1 : pas de déclaration
    chien2 = new Chien("Rantanplan");
    // Dans ce cas l'éditeur Android Studio devrait souligner la variable chien2 avant même la compilation en vous disant qu'il ne comprend pas d'où ça sort...

    // Erreur type 2 : pas d'instanciation
    Chien chien3;
    chien3.aboyer();
    // Dans ce cas vous aurez une erreur à l'exécution du code de type : "null does not have method aboyer"
```


## Rappel 2 : l'écoute événementielle

L'interface graphique nécessite un moyen d'effectuer une certaine tâche lorsque l’utilisateur fait une certaine action (appuyer sur un bouton, glisser l'écran vers la gauche, etc).

Cela est géré grâce à l'**écoute événementielle** (exactement comme en JavaScript).

Vous allez créer un lien entre un objet Java (bouton par exemple) pour que quand un certain événement se produit (par exemple un clique de l'utilisateur) une certaine fonction soit exécutée.

Exemple en JavaScript :

```javascript
    // Définition de la fonction à exécuter au clic
    function deconnecterUtilisateur() {
        utilisateur.logout();
    }

    // Récupération du bouton (instanciation d'un objet JavaScript à partir de la page HTML)
    var bouton = document.getElementById("bouton");
    
    // Ajout d'un écouteur au bouton associé à l'événement "click"
    el.addEventListener("click", deconnecterUtilisateur);
```

Le principe en Java est exactement le même (sauf qu'en Java tout est objet, on ne donne donc pas une *fonction* à exécuter mais un *objet* dont il faudra exécuter l'une des méthodes).

```java
    // Je déclare mes variables (le bouton et l'écouteur d’événement)
    View.OnClickListener eventListener;
    Button b_button;

    // J'instancie l'écouteur d'événement : une classe avec une seule méthode "onClick" qui sera exécutée au clic (d'où le nom...)
    eventListener = new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            // La fonction à exécuter au clic
        }
    };

    // J'instancie le bouton : je récupère l’objet de l'interface graphique et l'instancie en un objet Java qui le représente
    b_button = (Button)findViewById(R.id.b_button);

    // Je définie l’écouteur d'événement précédemment crée comme étant l'écouteur de mon bouton
    b_button.setOnClickListener(eventListener);

```

