# Les bases (pratiques) d'Android

Pour mettre en pratique l'utilisation d'Android Studio et prendre
en main un projet Android, nous allons créer une application simulant
un lancé de pièce (pile ou face).

## Le principe de l'application

L'application sera composée d'une vue permettant d'afficher un text (TextView).
Il y aura, stocké dans les ressources, deux strings correspondant aux résultats
en français et anglais ("Pile" ou "Face" et "Tails" ou "Heads").

Au démarrage de l'application, le programme génère un nombre égale à 0 ou 1, et
affiche le label correspondant. L'utilisateur peut effectuer un nouveau lancé en cliquant sur un bouton.

Grâce à la bonne utilisation des ficheirs de ressources, l'application est
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
