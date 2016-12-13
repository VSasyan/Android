# Les bases (pratiques) d'Android

Pour mettre en pratique l'utilisation d'Android Studio et prendre
en main un projet Android, nous allons créer une application simulant
un lancé de pièce (pile ou face).

## Le principe de l'application

L'application sera composée d'une vue permettant d'afficher un text (TextView).
Il y aura, stockés dans les ressources, deux labels correspondant aux résultats
en français et anglais ("Pile" ou "Face" et "Tail" ou "Head").

Au démarrage de l'application, le programme génère un nombre égale à 0 ou 1, et
affiche le label correspondant. L'utilisateur peut effectuer un nouveau lancé en cliquant sur un bouton.

Grâce à la bonne utilisation des fichiers de ressources, l'application est
automatiquement traduite (vous pouvez changer la langue de votre téléphone pour voir).

## Mise en place

Créez un nouveau projet appelé "Dice" :

![Écran de création 1/3](screens/1_creation_1.png)

![Écran de création 2/3](screens/1_creation_2.png)

![Écran de création 3/3](screens/1_creation_3.png)

Regardez la structure du projet créé par Android Studio :

![Structure du projet](screens/2_environnement_1_structure.png)

Ouvrez les fichiers `manifest`, `DiceActivity`, et `activity_dice` en mode Deisgn et Text :

![Structure du projet](screens/2_environnement_1_manifest.png)

![Structure du projet](screens/2_environnement_2_DiceActivity.png)

![Structure du projet](screens/2_environnement_4_activity_dice_design.png)

![Structure du projet](screens/2_environnement_2_manifest.png)

## Pas à pas des modifications

### 1) Modification de la vue

Dans un premier temps, il faut modifier la vue pour ajouter une zone de text.
Modifier le TextView automatiquement ajouté par Android Studio : rennommez le en `tv_dice_results` et agrandissezz-le pour qu'il prenne tous l'espace.

Vous devez obtenir le code suivant :

```xml
    <TextView
        android:text="TextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/tv_dice_results"
        android:layout_alignParentTop="true"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentEnd="true" />
```

### 2) Ajout des labels dans les fichiers strings

Ouvrez le fichier `res/values/string.xml`. Android Studio doit automatiquement afficher un mesage vous proposant de traduire, cliquez sur « Open editor » (sinon faites un clique-droit sur le fichier `string.xml` et choisissez « Open Translations Editor »).

Cliquez sur la petite croix verte :

![Croix verte](screens/3_res_1_ajouter_string.png)

Remplissez la fenêtre (faites de même avec « head ») :

![Fenêtre pour « tail »](screens/3_res_2_tail.png)

Cliquez sur la petite Terre bleue et selectionnez « French ».

![Terre bleue](screens/3_res_1_ajouter_string.png)

Le programme doit ajouter une nouvelle colonne qu'il suffit de compléter, voici le résultat à obtenir :

![Traductions complétées](screens/3_res_3_termine.png)

Remarquez que le programme a en fait créé un deuxième fichier XML :
* `res/values/strings.xml` (dossier par défaut)
* `res/values-fr/strings.xml` (dossier en *fr*ançais)

![Fichiers](screens/3_res_4_fichiers.png)

### 3) Modification de l'activité

Nous allons maintenant modifier l'activité. La méthode `onCreate` est exécutée à la création de l'activité. Comme cette activité est l'activité de démarrage de l'application, cette fonction sera exécutée sans intervention de l'utilisateur.

