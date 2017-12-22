# Rappels et aides supplémentaires

## Les bases (théoriques d'Android)

[Voir la présentation en PDF](Les_bases_théoriques_d_Android.pdf)

## Les objets Java

Java est un langage à **typage statique** (contrairement au JavaScript ou au PHP par exemple). Cela implique la déclaration des variables :

```java
    int unEntier;
    float unFlottant;
    String uneChaineDeCaracteres;

    // Ensuite je peux les utiliser :
    unEntier = 40;
    unFlottant = 2.42;
    uneChaineDeCaracteres = "Je suis une chaîne de caractères...";

    // On peut également déclarer et utiliser en même temps :
    int unAutreEntier = 8;
```

Lorsque l'on utilise des Classes, il faut *toujours* **déclarer** et **instancier** la variable, avant de l'utiliser :

```java

    // Déclaration de ma classe
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
    // Dans ce cas vous aurez une erreur à l'exécution du code de type : "null does not have aboyer method"
```


## L'écoute événementielle

L'interface graphique nécessite un moyen d'effectuer une certaine tâche lorsque l’utilisateur fait une certaine action (appuyer sur un bouton, glisser l'écran vers la gauche, etc).

Cela est géré grâce à l'**écoute événementielle** (exactement comme en JavaScript).

Vous allez créer un lien entre un objet Java (bouton par exemple) pour que quand un certain événement se produit (par exemple un clique de l'utilisateur) une certaine fonction soit exécutée.

Exemple en JavaScript :

```javascript
    // Définition de la fonction à exécuter au clique
    function deconnecterUtilisateur() {
        utilisateur.logout();
    }

    // Récupération du bouton (instanciation d'un objet JavaScript à partir de la page HTML)
    var bouton = document.getElementById("bouton");
    
    // Ajout d'un écouteur au bouton associé à l'événement "click"
    el.addEventListener("click", deconnecterUtilisateur);
```

Le principe en Java est exactement le même, sauf qu'en java tout est objet, donc on ne donne pas une fonction à exécuter, mais un objet (dont il faudra exécuter l'une des méthode).

```java

    // Je déclare mes variables (le bouton et l'écouteur d’événement)
    View.OnClickListener eventListener;
    Button b_button;

    // J'instancie l'écouteur d'événement : une classe avec un seul méthode qui sera exécuté au clique (d'où le nom...)
    eventListener = new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            // La fonction à exécuter au clique
        }
    }

    // J'instancie le bouton : je récupéré l’objet de l'interface graphique, et le lie à un objet Java qui le représente
    b_button = (Button)findViewById(R.id.b_button);

    // J'ajoute l’écouteur d'événement précédemment crée comme étant l'écouteur de mon bouton
    b_button.setOnClickListener(eventListener);

```

