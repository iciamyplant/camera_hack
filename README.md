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
|apt-get | programme de gestion des paquets|

- **apt-get update** (optionnel) : pour mettre notre cache à jour si ce n'est pas déjà fait ;
- apt-cache search monpaquet (optionnel) : pour rechercher le paquet que nous voulons télécharger si nous ne connaissons pas son nom exact ;
- **apt-get install** monpaquet : pour télécharger et installer notre paquet.

### Surveiller l'activité du système
### Exécuter des programmes en arrière-plan
### La connexion sécurisée à distance avec SSH
### Transférer des fichiers
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
