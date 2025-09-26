# Création d'objet interactif

Les jeux vidéo utilisent une grande variété d' objets interactifs. Même les personnages jouables sont, d'une ceratine façon, des objest interactifs. D'autres exemples sont les power-ups, les items collectables d'inventaire, les ennemis, les zones de détection, des objets de scénario interactifs, les NPCs (personnages non-jouables).

Souvant, un objet interactif doit impacter ou au moins influencier un autre objet. Par exemple, un power-up de saut double va impacter le comportement d'un personnage. Un objet interactif peut aussi changer des informations : une monnaie peut changer la valeur du score ou de l'inventaire. Pour le TP2, on veut explorer les objets nteractifs

## Conditions et activation

- Quelles sont les conditions de déclénchement de l'objet interactif ? Est-ce qu'il y a un comportement de base actif à atout temps ?
- Est-il activé par proximité ? Ou par collision ? Ou par temps ? Quels noeuds on peut utiliser pour faciliter cette détection ?
- Combien de fois peut cet objet déclencher sont comportement ?
- Comment le jeu communique que cet objet est interactif (affordance) ?

## Effets

- Quels sont les autres objets qui sont mpactés quand l'objet interactif est activé ?
- Quels sont les effets que vont être appliqués ?
- Après l'application des effets, quel est l'état de l'objet interactif ? Est-ce que il retourne à sont état original ou est-il détruit après l'utilisation ?

### Noeuds outiles

- Area2D pour la détection de proximité ou changement de paramètres par volume.
- VisibilityNotifier2D pour savoir si un objet est visible a l'écran.
- CharacterBody2D si l'objet a du mouvement comme un personnage.
- RigidBody2D si l'objet a des mouvmeents contrôlés par la simulation physique.

