# Les bases d'Android

## L'environnement

### Plateforme et Architecture

* Smartphone utilisation particulère
* Java, language multiplateforme
* A chaque version d'Android est associée une API
 * gestion des vues
 * partage de données (Contacts, ...)
 * notifications

### Environnement de développement

* Android Studio
 * Android SDK Manager
 * Android AVD : Android Virtual Device
 * Android ADB : Android Debug Bridge
 * Logcat

## Organisation

### Anatomie des projets

![]( Image présentant l'anatomie)

### Détails des fichiers

#### Manifest

* Manifest : informations de base concernant l'application
 * Nom et icône
 * Permissions
 * Compatibilité (API)
 * Liste les activités

#### Ressources

* Ressources : l'ensemble des données
 * drawable : images
 * layout : les vues en XML
 * menu : les menus en XML
 * values : toutes les constantes (strings, couleurs, ...)

#### java

* Les classes java
 * Activités : controlleurs associés aux vues
 * Services : classes spécialisées permettant de gérer la données
 * ...
 
### Structure d'un projet

