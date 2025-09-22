# Interface virtuelle (menu et pause)

## Menus

Souvant, les jeux vidéo fixent des informations importantes à l'écran, comme le comptage de points ou les points de vie du personnage. Pour créer un menu qui est affiché à tout temps, il faut qu'on configure nos élements d'interface pour être rendus indépendentment de la position de la caméra.

Dans Godot, on peut créer un noeud **CanvasLayer** pour créer une couche des noeuds qui vont être rendus séparement du reste de la scène. Les objets de l'interface (herités de **Control**) sont placés comme fils de ce CanvasLayer pour définir le layout et organisation des diférents élements d'interface.

Exemple d'**arborescence** :

```
- InterfaceNiveau (CanvasLayer)
    - HUD (Control)
        - BarreVie (ProgressBar)
        - LabelPointage (Label)
        - ControlePause (Button)
        - PanneauPause (Panel)
            - ButtonInstructions (Button)
```

![Exemple de arborescence de scène d'une interface utilisateur.](image-1.png)

Les noeuds d'interface ont des options de placement adaptatif et auto-gérés et d'utilisation de thèmes visuels. C'est un système fléxible et élaboré; pour plus d'information, regarder la [documentation officielle](https://docs.godotengine.org/fr/4.x/tutorials/ui/index.html).

![Exemple de bouton ancré au coin en haut-droit.](image.png)

De façon résumé:

1. Les noeuds hérités de **Control** nous permettent de placer les élements de façon rélative à leus parents avec différents comportements d'adaptation. Pour ça, ils utilisent des **[ancres](https://docs.godotengine.org/fr/4.x/tutorials/ui/size_and_anchors.html)**. Les ancres controllent comment un noeuds-fils s'étire (ou non) pour suivre les dimensions et placement de se noeud parent.
2. Les options de **thème** nous permettent de définit une [habillage] (https://docs.godotengine.org/fr/4.x/tutorials/ui/gui_skinning.html) centrale pour nos élements d'interface et aussi de personaliser les options de thème pour chaque élement. On a déjà utilisé ça un peu pour customiser la taille des polices de caractères d'un **Label**, par exemple.
3. Les [noeuds conteneurs](https://docs.godotengine.org/fr/4.x/tutorials/ui/gui_containers.html)  (**containers**) controlent automatiquement le placement et taille de ses noeuds-enfants (ex. ScrollContainer, TabContainer, GridContainer). Ils sont particulièrement pratiques pour les interfaces plus complèxes et dynamiques.

## Pause

Les systèmes de pause dans les jeux vidéo généralement fonctionnent en arrêtant l'exécution des fonctions et des simulations qui sont calculées à chaque frame. Généralement ça veut dire que la configuration de *process mode* ou **modes de traitement** de chaque objet change temporairement.

Pendant la pause, les fonctions `_process()`, `_physics_process()`, `_input()`, et `_input_event()` ne sont pas exécutées, mais les noeuds vont continuer à recevoir des signaux. Les animations et audio sont mis en pause, et la simulation phyisique aussi.

![Jeu en pause.](pause.jpg)

### Mise en place

Dans Godot, on a plusieurs options de mode de traitement pour les noeuds dans une scène: 

> **Inherit** : L'exécution dépend de l'état du parent, du grand-parent, etc. Le premier parent qui a un état n'étant pas Inherit.
> **Pausable**: Traité seulement quand quand le jeu n'est pas pausé.
> **WhenPaused**: Traité seulement quand quand le jeu est pas pausé.
> **Always**: Traité le noeud à tout temps.
> **Disabled**: Le noeud n'est jamais traite (`process()` n'est jamais exécuté).

Pour plus d'info, regarder la [documentation officielle](https://docs.godotengine.org/fr/4.x/tutorials/scripting/pausing_games.html).

Donc, il faut configurer les noeuds qui on veut pauser avec le process mode `Pausable` (ex. les personnages, animations, HUD du jeu). Les obejts qui on veut que sont animés ou qui on veut que répond à des entrées pendant la pause doivent avoir un *process mode* `Always` ou `WhenPaused` (ex. interfaces virtuelles, boutons de menu, sources de musique de menu qui vont continuer à jouer).

### Activation et désactivation

Pour activer la pause, on utilise le command:

```gdscript
get_tree().paused = true # ou false, pour désactiver la pause
```
