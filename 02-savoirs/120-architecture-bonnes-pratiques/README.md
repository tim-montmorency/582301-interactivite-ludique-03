# Architecture et bonnes pratiques pour un projet de jeu vidéo


Une architecture solide est cruciale dans le développement d’un jeu vidéo. Voici quelques piliers fondamentaux à respecter.


## Une vision claire de l'arborescence


Il est crucial de toujours garder une compréhension générale des **nœuds présents dans l'arborescence principale du jeu**. Savoir quel objet est parent de quel autre, et où se situent les éléments critiques (comme le joueur, l'interface utilisateur ou les systèmes globaux) à tout moment permet de naviguer et de déboguer le projet avec efficacité.


## Le principe d'encapsulation


L'encapsulation consiste à créer des composants autonomes et réutilisables, parce qu'ils exposent seulement des éléments minimaux pour modification externe. Dans le contexte d'un moteur de jeu, cela passe par plusieurs pratiques :
*   **Diviser une arborescence complexe en sous-scènes logiques** en utilisant la fonction "Sauvegarder une branche comme scène". Cela permet de modulariser votre code et vos assets.
*   **Maintenir la logique centrale dans les nœuds racines d'une scène**. Ces nœuds orchestrent le comportement de leurs enfants.
*   **Exposer uniquement les paramètres nécessaires** via l'annotation `@export` dans l'Inspecteur. Cette technique crée une interface propre et contrôlée pour configurer votre objet, tout en protégeant ses données et nœuds internes d'un accès accidentel depuis l'extérieur, réduisant ainsi les erreurs.


## La communication entre nœuds : "Call Down, Signal Up"


Cette technique est un principe directeur pour la communication entre les différents éléments de votre jeu. Elle est définie par la **direction de la communication** dans l'arborescence. L'objectif de cette technique est de créer de scènes que sont modulaires et autonomes une des autres le plus possible.
*   **Appeler vers le bas (Call Down)** : Un script parent doit utiliser des références directes pour appeler des méthodes ou modifier les propriétés de ses nœuds enfants. Il doit également se connecter et traiter les signaux émis par ses enfants.
*   **Signaler vers le haut (Signal Up)** : Un script enfant ne doit jamais avoir des références directes vers son parent ou un nœud de niveau supérieur. Au lieu de cela, il **émet un signal** (par exemple, `ennemi_detruit`, `objet_ramasse`) pour notifier qu'un événement s'est produit. C'est à un nœud plus haut dans la hiérarchie (souvent le nœud  racine de la scène) de se connecter à ce signal et d'orchestrer la réponse.
*   **L'exception des systèmes globaux** : Les systèmes accessibles de partout (comme les *Autoloads* pour la gestion du son, UI ou des sauvegardes) peuvent parfois être appelés directement. Toutefois, l'utilisation de signaux reste souvent préférable pour maintenir un couplage faible.


## Des conventions de nomenclature cohérentes

L'uniformité dans les noms est une forme de documentation et un gain de productivité énorme.
*   Toujours utiliser des **noms descriptifs** qui aident à éviter des ambiguïtés.
*   **Dans les scripts** : Adoptez et suivez rigoureusement des conventions pour nommer les variables, les fonctions, les classes et les signaux (par exemple, `snake_case` pour les variables et fonctions, `PascalCase` pour les classes ou nœuds, préfixer les réponses à des signaux par `on_` comme `on_ennemi_detruit`). [La convention détaillée suggérée par Godot est disponible ici](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html). L'ordre des éléments dans un script est aussi importante pour sa clarté et lisibilité. Voici une liste de base (ignorez les éléments selon le besoin de chaque script):

```text
01. @tool, @icon, @static_unload
02. class_name
03. extends
04. ## doc comment

05. signals
06. enums
07. constants
08. static variables
09. @export variables
10. remaining regular variables
11. @onready variables

12. _static_init()
13. remaining static methods
14. overridden built-in virtual methods:
	1. _init()
	2. _enter_tree()
	3. _ready()
	4. _process()
	5. _physics_process()
	6. remaining virtual methods
15. overridden custom methods
16. remaining methods
17. subclasses
```

*   **Pour les fichiers et ressources** : Deux grandes approches d'organisation sont possibles. L'**organisation par type** groupe tous les fichiers similaires (tous les sprites, toutes les scènes, tous les sons). L'**organisation par fonctionnalité** regroupe dans un même dossier tous les éléments liés à un concept (textures, scènes, scripts, sons du "personnage_joueur"). Le choix dépend de la taille et de la nature de votre projet, mais la cohérence est primordiale. [Godot a aussi quelques conventions de base](https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html), comme l'utilisation de `snake_case` pour les dossiers et noms de fichiers.
