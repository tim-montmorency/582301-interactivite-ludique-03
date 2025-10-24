# Interface HUD

HUD est l’abréviation d’interface originaire des avions (*heads-up display*). L’idée de base c’est que ces éléments d’interface sont visibles à la majorité du temps du gameplay, parce qu’ils communiquent des informations relevantes pour son déroulement. Les éléments de HUD sont souvent placés aux coins de l’écran et utilisent différentes conventions et techniques d’interface pour présenter des informations.

Quelques exemples d’information affichée sur des HUD. 

- Points ou score
- Niveau actuel
- Argent
- Barres de vie
- Clés et items importants
- Munitions
- Comptage du temps ou chronomètres

Les éléments que sont montrés changent de jeu à jeu selon leur genre ou direction esthétique.

## Assemblage avec Godot

Pour afficher une interface relative à l’écran (indépendamment de la position de la **Camera2D**), on utilise un nœud **CanvasLayer** avec des nœuds d’interface (hérités de **Control**) dedans. On peut utiliser des conteneurs, comme les **HBoxContainer**, pour les organiser horizontalement.


La connexion entre les nœuds d’interface et les variables du jeu (niveau courant, nombre de monnaies, nombre de vies, etc.) peut être réalisée avec des **signaux**. Voici un exemple pour le nombre de monnaies.

```gdscript
# Dans main.gd, le script central du jeu

@export var monnaies = 0
signal monnaie_collectee

func augmenter_monnaies():
    monnaies += 1
    monnaie_collectee.emit(monnaies)
```

![Arborescene de la scène Monnaie](image.png)

```gdscript
# monnaie.gd
extends Area2D

func _ready() -> void:
    body_entered.connect(monnaie_touchee)

# Fonction quand un corps touche la monnaie
func monnaie_touchee(body):
    if body is Joueur:
        Main.augmenter_monnaies()
        call_deferred("queue_free")
```

![Arborescence de la scène HUD](image.png)

```gdscript
# Dans hud.gd
extends Control

# Connecte le signal pour activer la rétroaction et
# actualiser le HUD
func _ready() -> void:
    %MonnaiesLabel.text = str(Main.monnaies)
    Main.monnaie_collectee.connect(retroaction_monnaie)	

func retroaction_monnaie(valeur_monnaies):
    %MonnaiesLabel.text = str(valeur_monnaies)
    %MonnaiesSFX.play()
```
