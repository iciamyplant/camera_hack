# Linux Fundamentals
- [cours openclassroom](https://openclassrooms.com/fr/courses/43538-reprenez-le-controle-a-laide-de-linux/38351-la-structure-des-dossiers-et-fichiers)
### Les utilisateurs et les droits
Linux est un système multi-utilisateurs = plusieurs personnes peuvent travailler simultanément sur le même OS, en s'y connectant à distance notamment

- **Root** : Il y a un utilisateur « spécial », root, aussi appelé superutilisateur. Celui-ci a tous les droits sur la machine, et que lui a acces a certaines commandes.
- **Les autres** : On peut créer autant d'utilisateurs que l'on veut, eux-mêmes répartis dans des groupes. Par defaut utilisateur non root car un virus ne peut rien faire de plus que vous quand vous êtes connectés avec des droits limités. Mais si vous êtes en root il pourra tout faire. Sous Windows, vous êtes toujours connectés en administrateur par défaut (équivalent de root), ce qui explique pourquoi les virus y sont si dangereux.

| commande | fonctionnement |
|----------|-------|
| sudo *commande*  | devenir root temporairement | 
| sudo su | devenir root et le rester jusqu'a tapper exit|
| adduser *nom utilisateur*| ajouter un utilisateur (seul root peut le faire) repertoire dans /home/nomutilisateur|
| passwd *nom utilisateur* | changer le mot de passe (idem en root)|

- Chaque fichier et chaque dossier possède une liste de droits (ls -l). C'est une liste qui indique qui a le droit de voir le fichier, de le modifier et de l'exécuter. Root a tous les droits.

| commande | fonctionnement |
|----------|-------|
| chown *nom du nouveau proprio* *nom du fichier* | changer le propriétaire d'un fichier (idem en root)|
| chmod *chiffre* *fichier* | Modifier les droits d'accès (voir readme minishell) |

### Installer des programmes avec apt-get
Sous Windows, les programmes sont éparpillés aux quatre coins du Net.
Sous Linux, on a choisi de placer tous les programmes (paquets) au même endroit.L'installation de programmes fonctionne différemment d'une distribution à une autre.


| concept | principe |
|----------|-------|
| paquet | Sous Ubuntu, pas de programmes d'installation mais des paquets. Sorte de dossier zippé qui contient tous les fichiers du programme (fichier.deb). Contient toutes les instructions nécessaires pour installer le programme.|
| depot/repository | Tous les.deb sont rassemblés au même endroit sur un même serveur appelé dépôt. C'est le serveur sur lequel on va télécharger nos paquets. (y en a plein avec les memes packets dessus et on peut changer le serveur sur lequel on recupere nos packets par defaut) |
|dependance |un paquet peut avoir besoin de plusieurs autres paquets pour fonctionner, on dit qu'il a des dépendances|
|apt-get | programme de gestion des paquets. Si paquets pas dispo avec apt-get il faudra compiler un programme depuis les sources (avec tar zxvf)|

- **apt-get update** (optionnel) : pour mettre notre cache à jour si ce n'est pas déjà fait ;
- apt-cache search monpaquet (optionnel) : pour rechercher le paquet que nous voulons télécharger si nous ne connaissons pas son nom exact ;
- **apt-get install** monpaquet : pour télécharger et installer notre paquet.

### Surveiller l'activité du système
Linux est un système multi-tâches et multi-utilisateurs : il est capable de gérer plusieurs programmes tournant en même temps + plusieurs personnes peuvent utiliser la même machine en même temps (en s'y connectant via Internet).

Série de commandes qui nous permettront de savoir ce qui se passe actuellement dans notre ordinateur :

| commande | fonctionnement |
|----------|-------|
| w | plusieurs infos : La charge avec "load average ..." donne le nombre moyen de processus en train de tourner et qui réclament l'utilisation du processeur. En dessous c'est une ligne par user connecte. FROM (ou DE) indique l'IP (si connecte en local sera :0).  WHAT (ou quoi) la commande qu'il est en train d'exécuter en ce moment. En général, si vous voyez bash, cela signifie que l'invite de commandes est ouverte et qu'aucune commande particulière n'est exécutée.|
| who | Lui aussi donne la liste des connectes |
| ps | obtenir la liste des processus qui tournent au moment où vous lancez la commande et sur l'utilisateur sur lequel on est. Cette liste n'est pas actualisée en temps réel |
|ps -ef|  la liste de tous les processus lancés par tous les utilisateurs sur toutes les consoles |
|ps -ejH | afficher les processus en arbre, pour savoir quel processus enfant appartient a quel parent |
|top | processus en temps reel. En haut y a l'uptime et la charge, mais aussi la quantité de processeur et de mémoire utilisée (voir man plein de speficites)|
|sudo halt| arreter l'ordinateur|
|sudo reboot| redemarrer l'ordinateur|


### Exécuter des programmes en arrière-plan
Plusieurs programmes peuvent tourner en même temps au sein d'une même console. 

| commande | fonctionnement |
|----------|-------|
| & | rajouter le petit symbole & à la fin de la commande que vous voulez envoyer en arrière-plan. Exemple : cp video.avi copie_video.avi & |
|nohup |détacher le processus de la console, meme si on quitte la console ca kill pas le processus. Exemple : nohup cp video.avi copie_video.avi. La sortie de la commande est par défaut redirigée vers un fichiernohup.out|
|Ctrl + Z puis bg| mettre en pause l'exécution du programme qui est en premier plan puis le passer en arriere plan avec bg|
| jobs | connaître les processus qui tournent en arrière-plan|
|fg | reprendre un processus au premier plan|

### La connexion sécurisée à distance avec SSH

Toutes les machines sous Linux peuvent être configurées pour que l'on s'y connecte à distance, pour peu qu'elles restent allumées.

- Pourquoi faut-il sécuriser les échanges ?
Comme on ne peut pas complètement empêcher quelqu'un d'intercepter les données qui transitent sur l'internet (par exemple avec wireshark), il faut trouver un moyen pour que le client et le serveur communiquent de manière sécurisée. Le chiffrement sert précisément à ça : si le pirate récupère le mot de passe chiffré, il ne peut rien en faire.

- Comment fait SSH pour sécuriser les échanges ?
Il existe des tonnes d'algorithmes de chiffrement classables en deux categories : les chiffrements symétriques (une seule cle) et les chiffrements asymétriques (cle privee + cle publique). SSH = utilise d'abord le chiffrement asymétrique pour s'échanger discrètement une clé secrète de chiffrement symétrique. Ensuite, on utilise tout le temps la clé de chiffrement symétrique pour chiffrer les échanges.

- Comment utiliser SSH concrètement ?
Si vous voulez accéder à votre PC depuis un autre lieu (et donc suivre le reste de ce chapitre), vous devez le transformer en serveur au préalable.
```
##creer le serveur :
sudo apt-get install openssh-server
sudo /etc/init.d/ssh start #normalement pas besoin serveur ssh lance au demarrage
```
Puis pour s'y connecter, l'authentification par mot de passe ou l'authentification par clés publique et privée du client (ssh-keygen).
```
###s'y connecter a partir d'une machine Linux :
ssh *login*@*adresseipdemonordi* #verifier que le port 22 n'est pas bloque par un parefeu
# on va nous demander notre mot de passe
```

### Analyser le réseau et filtrer le trafic avec un pare-feu


- [Tryhackme](https://tryhackme.com/room/linuxfundamentalspart1)

# Network Fundamentals
- [Tryhackme](https://tryhackme.com/paths)
### What is Networking ?
### Intro to LAN
### OSI Model
### Packets & Frames
### Extending Your Network

# Offensive Pentesting
- [Certification CEH v11](https://bookshelf.vitalsource.com/#/books/9781635675337/cfi/112!/4/4@0.00:36.5)
- [Tryhackme](https://tryhackme.com/paths)

### Scanning Networks
### Enumeration
### Vulnerability Analysis
### System Hacking
### Malware Threats
### Sniffing
### Hacking Wireless Networks

# Cyber Defense
- [Tryhackme](https://tryhackme.com/paths)
### Threat and Vulnerability Management
### Secure Operations & Monitoring
### Malware Analysis
