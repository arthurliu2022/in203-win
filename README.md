# Projet IN203 sur Windows

Le tutoriel suivant vise à pouvoir travailler sur le projet d'IN203 sur Windows, sans utiliser WSL.

## Explication

En suivant ce tutoriel, vous allez installer les librairies graphiques SDL2 et SDL_Image, utiliser un Makefile modifié, et vous pourrez ainsi compiler le projet d'IN203 sur Windows.\
En option vous pourrez créer un alias pour rendre la compilation plus rapide (par exemple taper `make` au lieu de `mingw32-make`.

## Prérequis

- Windows (Windows 10 64 bits de préférence)
- MinGW correctement installé devant normalement inclure :
	- g++ (pour vérifier taper `g++ --version` dans l'invite de commandes cmd.exe)
	- mingw32-make (`mingw32-make --version` pour vérifier)
- Avoir récupéré le dossier du projet sur https://github.com/JuvignyEnsta/Promotion_2022

## Préparation de l'installation
- Vérifier si vous avez installé la version 64 bits ou 32 bits de MinGW. La ligne `Target :`de la commande `g++ -v` vous indiquera votre version : `mingw32` pour 32 bits et `x86_64-w64-mingw32` ou quelque chose de semblable pour 64 bits.

- Trouver le dossier d'installation de MinGW, normalement `C:\MinGW` ou bien `C:\Program Files\mingw-w64`. Le dossier dans lequel nous allons travailler est le sous-dossier contenant les dossiers `bin`, `include`, `lib`. Pour moi c'est  `C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64`.
Si vous avez la version 32 bits, c'est probablement juste `C:\MinGW`. Dans tous les cas, conservez précieusement le chemin vers ce dossier. Dans la suite du tutoriel, il sera indiqué par **dossier mingw préparé précédemment**.
- Ouvrez le dossier dans lequel vous avez déposé le projet. Cela doit être le dossier dans lequel se trouve le `Makefile`. Il sera indiqué dans la suite du tutoriel par **dossier du projet**.
- Télécharger ou cloner ce git

## Installation de SDL et SDL_Image

Ouvrir le dossier d'installation de MinGW, et aller jusqu'au **dossier mingw préparé précédemment** (il doit contenir les sous-dossiers `bin`, `include`, `lib`).

- Si vous avez la version 64 bits de MinGW : 
	- Ouvrir l'archive `SDL 64 bits.zip`
	- Copier le contenu de `/mingw` dans le **dossier mingw préparé précédemment**.
	- Copier le contenu de  `/projet` dans le **dossier du projet**.

- Si vous avez la version 32 bits de MinGW : 
	- Ouvrir l'archive `SDL 32 bits.zip`
	- Copier le contenu de `/mingw` dans le **dossier mingw préparé précédemment**.
	- Copier le contenu de  `/projet` dans le **dossier du projet**.


Tout cela permet de :
1. Installer les librairies SDL2 et SDL_Image
2. Ajouter les fichiers .dll qui permettront d'exécuter colonisation.exe
3. Ajouter le Makefile modifié pour Windows et mingw32-make

## Compilation avec mingw32-make

Se rendre dans le **dossier du projet**.

Il faut maintenant modifier le Makefile pour qu'il puisse avoir accès aux librairies qui viennent d'être installées.

Ouvrir le Makefile et trouver ces lignes.
```Makefile
INCLUDE_PATHS = -I"C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\include"

LIBRARY_PATHS = -L"C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\lib"
```

Remplacer le chemin sur la première ligne par le chemin vers votre propre `\include` dans votre **dossier mingw préparé précédemment**. (`C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\include` ou bien `C:\MinGW\include` par exemple).

Puis remplacer le chemin sur la deuxième ligne de la même façon vers votre propre `\lib`. (`C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\lib` ou bien `C:\MinGW\lib` par exemple).

Attention à bien garder le chemin entre guillemets `""` dans tous les cas.

Une fois que cela est fait, la compilation devrait pouvoir se faire correctement. Ouvrez un invite de commandes cmd.exe et allez dans le **dossier du projet**. Puis tapez la commande `mingw32-make all` (il s'agit de la même commande même si MinGW est en 64 bits).

Si tout fonctionne correctement, la compilation se fait sans erreur et vous pouvez lancer le programme `colonisation.exe` à partir de l'invite de commandes cmd.exe ou en double-cliquant dessus. L'animation doit s'afficher normalement, et des informations doivent apparaître dans le terminal.

Lors de la compilation, si il y a une erreur concernant memcpy, ajouter `#include <cstring>` au début de parametres.cpp.


## (Optionnel) Création d'un alias pour mingw32-make

Il peut être long d'écrire `mingw32-make all` à chaque fois que l'on veut compiler. Il est possible de créer un alias pour ne devoir taper que `make` au lieu de `mingw32-make` par exemple.

Créer un dossier Alias où vous voulez. Par exemple `C:\Alias`. Conserver le chemin vers le dossier créé.

Dedans, créer un ficher `[alias].bat` avec `[alias]` la commande que vous souhaitez réaliser.
Modifier le .bat (clic droit, puis Modifier) et écrire à l'intérieur :
```batch
@echo off
echo.
mingw32-make %*
```
Sauvegarder et ouvrir l'**éditeur de variables d'environnement Windows.**
Pour cela ouvrir le menu Windows (touche Windows) et taper `variables d'environnement`, puis sélectionner le premier résultat.
Si cela ne marche pas, taper Windows+R et écrire `control sysdm.cpl,,3`, appuyer sur OK, une fenêtre s'affiche, sélectionner Variables d'environnement en bas.

Dans la fenêtre Variables d'environnement, sélectionner Path dans la partie du haut, puis cliquer sur le bouton modifier.
Une nouvelle fenêtre s'affiche, cliquer sur Nouveau et entrer le chemin vers le dossier créé précédemment (par exemple `C:\Alias`).\
Cliquer sur OK pour valider toutes les fenêtres.

Pour tester si cela a fonctionné, tapez `[alias] --version` dans l'invite de commandes.
