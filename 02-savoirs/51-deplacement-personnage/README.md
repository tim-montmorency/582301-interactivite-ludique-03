# Déplacement dans un environnement virtuel

Pour définir des objets qui se déplacent et peuvent naviguer l'espace du jeu, on utilise le système de physique en 2D. On a plusieurs noeuds d'intêret à notre disposition.

> Les corps `CharacterBody2D` détectent les collisions avec d'autres corps, mais ne sont pas affectés par des propriétés physiques comme la gravité ou la friction. Ils doivent plutôt être contrôlés par l'utilisateur à l'aide de code. Le moteur physique ne déplacera pas un corps de personnage. 
> [Source](https://docs.godotengine.org/fr/4.x/tutorials/physics/physics_introduction.html)

Cette option nous permet beaucoup de contrôle sur le mouvement precis du personnage.

Le nœud `RigidBody2D` implémente la physique 2D simulée. On ne contrôle pas un RigidBody2D directement. Au lieu de cela, on lui applique des forces et le moteur physique calcule le mouvement résultant, y compris les collisions avec d'autres corps, et les réactions de collision, telles que le rebondissement, la rotation, etc (adapté des [docs](https://docs.godotengine.org/fr/4.x/tutorials/physics/physics_introduction.html)). Cette option crée des interactions physiques complexes, mais on perd un peu de contrôle.
