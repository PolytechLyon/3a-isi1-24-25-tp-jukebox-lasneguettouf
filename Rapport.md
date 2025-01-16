# Compte Rendu TP Jukebox

## Binôme
- Lasnes Tanguy
- Guettouf Yanis

## Choix de conception et de réalisation
Choix techniques : 
Vue.js : Framework choisi pour sa simplicité.
CSS  : Utilisé dans Playlist.vue en personnalisant les boutons Play et Delete.
Reactivité : Gérée via ref() et watch() pour suivre les changements dans la piste en cours et les fichiers audio.
Structure :

Nous avons choisi une architecture modulaire en utilisant Vue.js. Chaque composant est responsable d'une fonctionnalité spécifique :

    AddTrack.vue : Permet à l'utilisateur d'ajouter un fichier audio à la playlist. Ce composant intègre une gestion de fichiers locale.
    Playlist.vue : Affiche la liste des pistes ajoutées, avec deux fonctionnalités principales : lecture et suppression. Les boutons Play et Delete ont été stylisés grâce au css directement intégré au fichier.
    Player.vue : Gère la lecture des pistes, avec un suivi de progression et des modes de lecture :
        Repeat Track : Répète la piste en cours.
        Repeat List : Passe automatiquement à la piste suivante ou revient au début de la playlist.
        Don’t Repeat : Arrête la lecture à la fin de la piste.
    Jukebox.vue : Le composant parent, qui gère les interactions entre AddTrack.vue, Player.vue, et Playlist.vue. Il gère également les données partagées, comme la liste des pistes et la piste actuellement en lecture.
## Difficultés rencontrées (optionnel)
Lors de la mise en place des modes de lecture, nous avons rencontré des difficultés à gérer les transitions entre pistes dans Repeat List et Repeat Track. La solution a consisté à :

    Utiliser emit() dans Player.vue pour notifier le composant parent (Jukebox.vue) des changements de piste.
    Introduire un watch() pour détecter les modifications de la piste en cours et recharger l’audio.
## Extensions réalisées (optionnel)
Nous avons réalisé l'extension qui permet aux pistes passées par URL dans la playlist de rester en mémoire même lorsque la page est rafraîchie.
