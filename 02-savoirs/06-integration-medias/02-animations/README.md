# Techniques d'animation d’éléments visuels

## Animation via AnimatedSprite2D

Déjà mentionné sur les [notes d'intégration d'images](/02-savoirs/06-integration-medias/01-images). Plus d'info sur les docs : 
https://docs.godotengine.org/fr/4.x/tutorials/2d/2d_sprite_animation.html.

- Usage : animations image‑par‑image (marche, saut, idle).
- Géré via un Resource SpriteFrames édité dans l’éditeur.
- Avantage : simple et léger pour SFX visuels et pixel art.
- Docs : https://docs.godotengine.org/fr/4.x/classes/class_animatedsprite2d.html

Exemple
```gdscript
# attaché à un AnimatedSprite2D
func _ready():
    $AnimatedSprite2D.play("run")
```

## AnimationPlayer (keyframes)

- Usage : animer propriétés (position, scale, modulate, paramètres audio), appels de fonctions, tracks multiples.
- Idéal pour synchroniser son+visuel et créer séquences complexes.
- Sauvegarde les animations comme ressources réutilisables.
- Docs : https://docs.godotengine.org/fr/4.x/classes/class_animationplayer.html
- Tutoriel : https://docs.godotengine.org/fr/4.x/tutorials/animation/animation_player.html

Exemple
```gdscript
# jouer une animation nommée "open"
$AnimationPlayer.play("open")
```

## Tweening / SceneTreeTween (interpolations procédurales)

- Usage : interpolations temporisées (position, alpha, volume) sans créer d’animation dans l’éditeur.
- API moderne via create_tween() (chaînage, easing, loops).
- Docs : https://docs.godotengine.org/fr/4.x/classes/class_scenetreetween.html

Exemple
```gdscript
# déplacer un Node2D et changer la couleur avec un tween
func _ready():
    var t = create_tween()
    t.tween_property($Sprite2D, "position", Vector2(400, 200), 0.6).set_trans(Tween.TRANS_QUAD).set_ease(Tween.EASE_OUT)
    t.tween_property($Sprite2D, "modulate", Color(1,0.5,0.5), 0.6)
```

## Autres outils utiles

- AnimationTree / StateMachine : pour blend et systèmes d’animation avancés (surtout 3D, mais utile en 2D avancé). https://docs.godotengine.org/fr/4.x/classes/class_animationtree.html
- Skeleton2D / Bone2D : rigs pour animation d’objets 2D découpés. https://docs.godotengine.org/fr/4.x/classes/class_skeleton2d.html
- Particles (GPUParticles2D) : effets visuels complémentaires. https://docs.godotengine.org/fr/4.x/classes/class_gpuparticles2d.html

## Bonnes pratiques

- Séparer animations visuelles (AnimatedSprite2D) et logiques (AnimationPlayer/Tween).
- Nommer clairement les animations et documenter déclenchements (par input, état, collisions).
- Réutiliser AnimationPlayer pour synchroniser son + visuel.
- Optimiser : limiter tailles d’images, regrouper spritesheets, tester en export HTML5/WASM.
- Pour pixel art, désactiver le filtrage (filter off) et utiliser frames alignées.

## Ressources
- AnimationPlayer : https://docs.godotengine.org/fr/4.x/classes/class_animationplayer.html  
- AnimatedSprite2D : https://docs.godotengine.org/fr/4.x/classes/class_animatedsprite2d.html  
- SceneTreeTween / create_tween() : https://docs.godotengine.org/fr/4.x/classes/class_scenetreetween.html