
# Intégration d'images et des animations 2D

## Ajout d'images

Pour ajouter une image à un projet, on peut le glisser sur la fênetre **FileSystem**. On peut aussi changer les options d`importation pour ce fichier dans la fenêtre **Import**.

## Nœuds images

Voic quelques aspects important pour le choix de type de nœud à utiliser pour les images:

- Choisir le bon nœud selon l’usage : rendu 2D, interface, tuiles, textures 3D, ou rendu dynamique.
- Optimiser pour la cible (WebGL/WASM → poids réduit, atlas, formats compressés quand possible).
- Configurer l’import (compression, filtre, repeat, mipmaps) dans l’onglet Import pour chaque image.

Les types de nœuds qui peuvent traiter les images 2D en Godot sont :

- Sprite2D — afficher une texture 2D simple (images isolées)  
  https://docs.godotengine.org/en/stable/classes/class_sprite2d.html
- AnimatedSprite2D / SpriteFrames — animations image-par-image (spritesheets ou frames)  
  https://docs.godotengine.org/en/stable/classes/class_animatedsprite2d.html
- TextureRect — intégrer une image dans l’interface (Control) avec options d’échelle et de stretch  
  https://docs.godotengine.org/en/stable/classes/class_texturerect.html
- TextureButton — bouton graphique basé sur textures (UI interactive)  
  https://docs.godotengine.org/en/stable/classes/class_texturebutton.html
- NinePatchRect — images UI redimensionnables (bordures conservées)  
  https://docs.godotengine.org/en/stable/classes/class_ninepatchrect.html
- TileMap / TileSet — création de niveaux 2D à partir de tiles / atlas  
  https://docs.godotengine.org/en/stable/classes/class_tilemap.html | https://docs.godotengine.org/en/stable/classes/class_tileset.html
- ViewportTexture / Viewport — rendre une scène dans une texture (render-to-texture, mini-caméras, effets)  
  https://docs.godotengine.org/en/stable/classes/class_viewport.html | https://docs.godotengine.org/en/stable/classes/class_viewporttexture.html
- MeshInstance3D + StandardMaterial3D — appliquer des textures en 3D (albedo, normal, roughness...)  
  https://docs.godotengine.org/en/stable/classes/class_meshinstance3d.html | https://docs.godotengine.org/en/stable/classes/class_standardmaterial3d.html
- Importer d’images & workflow d’import — réglages disponibles lors de l’import (format, compression, mipmaps)  
  https://docs.godotengine.org/en/stable/tutorials/assets/importing_assets.html

## Définition d'animations frame par frame

Pour des [images individuelles avec frames d'animation](https://docs.godotengine.org/fr/4.x/tutorials/2d/2d_sprite_animation.html#individual-images-with-animatedsprite2d) : 

1. On peut utiliser un noeud **AnimatedSprite2D**. 
2. Avec ce node, dans l'**Inspecteur**, on ajoute un nouveau ressource du type **SpriteFrames**.
3. L'**éditeur de SpriteFrames** en bas de l' écran nous permet de glisser les frames, les ordonner et définir plusieurs animations.
4. On peut changer quelle animation joue en chageant la propriété `animation` du noeud **AnimatedSprite3D**.

Pour créer les animations a partir d'un fichier *spritesheet* (i.e. avec plusieurs frames dans un même fichier d'image), le processus est le même, mais on peu utilise le bouton de grille sur l'éditeur de **SpriteFrames**.

![Bouton de grille sur l'**éditeur de SpriteFrames**](image.png)

Après, on peut choisir des frames specifiques pour l'animation:

![Selectionner les frames sur la feuille de sprites](image-1.png)

## Contrôle d' animations

> Une fois l'animation terminée, vous pouvez contrôler l'animation via le code en utilisant les méthodes `play()` et `stop()`. 

Voici un bref exemple pour lire l'animation tant que le bouton de la souris est maintenue enfoncée et l'arrêter lorsque vous le relâchez.

```gdscript
extends Node2D

@onready var animations = $AnimatedSprite2D

func _process(delta: float) -> void:
	if Input.is_mouse_button_pressed(MOUSE_BUTTON_LEFT):
		animations.play("default")
	else:
		animations.stop()
```

## Bonnes pratiques et optimisation

- Atlas / spritesheets : regrouper petites images dans un atlas réduit overhead (surtout Web).
- Formats : PNG pour transparence, JPEG pour photos (sans alpha). Pour le web, réduire la résolution et compresser avant import.
- Taille : privilégier puissances de deux si utilisé avec certaines compressions/mipmaps pour 3D.
- Mipmaps : utiles en 3D et pour textures réduites; à désactiver pour pixel art 2D.
- Pixel art : désactiver filtrage (filter off) et activer pixel snap si nécessaire.
- Assets lourds : compresser, ou stocker hors dépôt (Git LFS pour projets avancés).
- Tester sur la cible (navigateur WebGL/WASM) : rendu et performances diffèrent du desktop.
