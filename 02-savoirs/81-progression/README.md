# Progression aux jeux vidéo

## Analyse et discussion

Jouez les 2 niveaux de l’exemple multi-niveaux ([lien jouable](https://egl-edu.github.io/exemple--multi-niveaux/), [repo](https://github.com/egl-edu/exemple--multi-niveaux)). 

Voici les cartes de ces deux niveaux. 

- Quelles sont les principales différences entre les niveaux ? 
- Quelles sont ses caracteristiques liées à la progression ?

![Niveau 1](image-1.png)

![Niveau 2](image.png)

<!--

- Développement thématique
  - Mécaniques
    - Combinaison des mécaniques
    - Systèmes que se rendent plus complexes
  - Narratif
- Entre dificulté et rhythme
  - Apprentissage
  - Répétition et performance

-->

## Notes d'implementation et conception

### Intégration avec la machine d'état du joueur

Le `Joueur` de l'exemple possède un `etat_courant` (REPOS, PROMENER, SAUT...). On peut utiliser ces états pour contrôler :

- animations (changer animation selon l'état)
- capacités temporaires (verrouiller le saut lors d'une cutscene)
- interactions (ne pas autoriser certains triggers si le joueur est en état particulier)

Astuce : dans le trigger, vous pouvez interroger `body.etat_courant` pour décider si le passage est autorisé (ex. ne laisser passer que si le joueur est au sol).

```gdscript
if body is Joueur and body.etat_courant != body.Etat.SAUT:
	Main.changer_scene(nouvelle_scene)
```

### Structure minimale pour la progression apr transition de scène (ex. porte /level gate)

Étapes :

1. Placez une `Area2D` (porte/trigger) avec un `CollisionShape2D` à l'endroit où on doit changer de niveau.
2. Exposez la `PackedScene` cible (`nouvelle_scene`) en export sur le script du trigger.
3. Lors du signal `body_entered`, vérifiez que l'objet est bien le joueur (type, groupe ou propriété).
4. Appeler un gestionnaire central (autoload) pour effectuer le changement de scène de façon sûre (déféré si nécessaire).

Pourquoi utiliser un autoload (Main) ?
- Centralise la logique de changement de scène.
- Permet de faire des préparations (sauvegarde d'état, fade out, statistiques) avant le changement.
- `get_tree().change_scene_to_packed()` doit idéalement être appelé de façon différée depuis un signal pour éviter les conflits de pile d'exécution — d'où `call_deferred` dans `Main.changer_scene`.

Code minimal du trigger (bonnes pratiques) :

```gdscript
func _on_body_entered(body: Node) -> void:
	# Vérifier soit par type, soit par groupe
	if body is Joueur or body.is_in_group("player"):
		# Option : désactiver le trigger pour éviter retriggers
		set_monitoring(false)
		# Option : jouer animation/son puis changer
		Main.changer_scene(nouvelle_scene)
```

### Variantes utiles pour la progression

- **Gating (verrouillage par condition)** : ne changer de niveau que si le joueur possède un item ou a rempli une condition. C'est la base pour les [systèmes de clé et porte](/02-savoirs/82-cle-et-porte/).

    ```gdscript
    if body.has_method("has_key") and body.has_key("gold_key"):
        Main.changer_scene(nouvelle_scene)
    ```

- **Téléportation avec transition** : animer la disparition du joueur, sauvegarder l'état, puis changer la scène.

- **Checkpoints et retour** : stocker la position/restart point dans un autoload ou un fichier pour reprendre en cas de mort.

- **Branches de niveaux** : `nouvelle_scene` peut être choisi dynamiquement selon l'état du jeu (choix du joueur, score, etc.).


### Scénarios avancés (design de niveaux)

- Niveaux modulaires : charger des scènes petites (modules) et composer la carte à la volée.
- Transition douce : cross-fade visuel + audio pour améliorer l'expérience.
- Niveaux dépendants d'objets : débloquer portes si un ensemble d'objectifs est accompli.
- Multiplay / persistance réseau : synchroniser l'état de progression côté serveur.