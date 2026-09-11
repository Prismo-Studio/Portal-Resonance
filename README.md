# Portal: Resonance

Mod Portal 2 (Hammer + assets P2) dont l'histoire se déroule à Black Mesa.

Un technicien de Black Mesa teste un prototype de portal gun volé à Aperture. La Cascade de résonance frappe en plein essai. Il faut fuir à travers le complexe, puis Xen. Le titre joue sur la Cascade de résonance et la pierre de lune / cristal de Xen qui « résonnent » avec le dispositif.

## Prérequis
- **Portal 2** + **« Portal 2 Authoring Tools »** (Steam > Bibliothèque > filtre Outils). Lancer Portal 2 une fois.
- **Hammer++ build Portal 2** : https://ficool2.github.io/HammerPlusPlus-Website/ → extraire le contenu du zip (les 4 éléments : dossier `hammerplusplus`, `hammerplusplus.exe`, `hlmvplusplus.dll`, `hlmvplusplus.exe`) directement dans `Steam\steamapps\common\Portal 2\bin\`, à côté de `hammer.exe`. Pas de sous-dossier.

## Installation du mod
1. Créer `Steam\steamapps\sourcemods\` s'il n'existe pas (même bibliothèque Steam que Portal 2).
2. `git clone <url> resonance` dans ce dossier.
3. Redémarrer Steam complètement : « Portal: Resonance » apparaît dans la bibliothèque.
4. Jonction obligatoire, sinon Hammer++ ne trouve pas les shaders et affiche des vues noires (PowerShell admin, adapter la lettre de disque) :

   ```
   cmd /c mklink /J "D:\SteamLibrary\steamapps\sourcemods\platform" "D:\SteamLibrary\steamapps\common\Portal 2\platform"
   ```

## Config Hammer++ (Tools > Options)
Onglet **Game Configurations > Edit > Add** « Resonance » :
- Game Data files : `...\Portal 2\bin\portal2.fgd`
- Default PointEntity : `info_player_start` ; Default SolidEntity : `func_detail`
- Game Executable Directory : `...\Portal 2`
- Game Directory : `...\sourcemods\resonance`
- Hammer VMF Directory : `...\sourcemods\resonance\mapsrc`

Onglet **Build Programs** (config Resonance) :
- Game executable : `...\Portal 2\portal2.exe`
- BSP / VIS / RAD : `...\Portal 2\bin\vbsp.exe`, `vvis.exe`, `vrad.exe`
- Place compiled maps in : `...\sourcemods\resonance\maps`

Redémarrer Hammer++, choisir **Resonance**. La fenêtre Messages doit afficher `Search Path (GAME): ...sourcemods\resonance\` et aucune erreur « Couldn't load vertex shader ».

## Compiler et tester
- F9, BSP/VIS/RAD en **Normal**. Paramètres de lancement conseillés : `-windowed -w 1920 -h 1080 -novid`.
- Map de test : `mapsrc/test.vmf` (boîte creuse 512x512x256, `info_player_start`, `light`). Console en jeu : `map test`.

## Pièges rencontrés
- **Grille** : icônes 2e/3e de la barre du haut (les crochets ne marchent pas en azerty).
- **Outil Entity** : un clic pose un aperçu, il faut **Entrée** pour créer l'entité. Changer d'entité avant Entrée annule la première.
- Un bloc transformé en `func_detail` (bouton toEntity) disparaît au Make Hollow et ne ferme pas la map : vérifier `solid with 6 faces` dans la barre d'état, sinon `toWorld`.
- **Make Hollow** (Tools, Ctrl+H) : épaisseur 16, bloc d'au moins 256 de haut.
- **Browse** choisit la texture courante, **Shift+T** l'applique à la sélection. Replace ne sert pas à ça.
- Dans les propriétés d'une `light`, Brightness s'affiche comme un rectangle blanc : ce n'est pas vide.
- Écran noir en jeu avec le réticule = spawn dans un mur ou pas d'`info_player_start`.

## Git
- On versionne les `.vmf`, jamais les `.bsp` (déjà dans `.gitignore`).
- Une map par personne, pas deux personnes sur le même `.vmf` en même temps.
