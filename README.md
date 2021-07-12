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

#### 1. Lorsque vous êtes connectés à l'internet, vous avez régulièrement des applications qui vont se connecter puis télécharger et envoyer des informations. Comment surveiller ce qui se passe ? Quelle application est en train de communiquer et sur quel port ?


Chaque IP est associee a un hostname (= nom en toutes lettres plus facile à mémoriser et qui revient exactement au même que d'écrire l'adresse IP). L'association des 2 est faite par les serveurs DNS. Si je tappe siteduzero.com dans mon navigateur je tombe sur openclassroom, plus facile que de retenir l'adresse ip.


| commande | fonctionnement |
|----------|-------|
| host *google.fr*  | nous indique ladresse IP du hostname google.fr | 
| whois *siteduzero.com* | permet d'obtenir toutes les infos sur un nom de domaine (prenom, adresse ...) |
| ifconfig | permet de gérer les connexions réseau de votre machine|
| netstat | permet d'analyser ces connexions, de connaître des statistiques, etc |


**ifconfig** :
- liste les interfaces reseau (eth0, lo ...) et permet de faire des reglages reseau (par exemple desactivier l'interface eth0 : ifconfig eth0 down)
- lo : c'est la boucle locale. Tout le monde doit l'avoir c'est connexion a nous meme
- packets *un chiffre pas egal a 0* indique l'interface active la plus utilisee 


**netstat** :
- netstat permet d'analyser ces connexions, de connaître des statistiques, etc.
- netstat -i : tableau présentant, pour chaque interface réseau que vous avez, une série de statistiques d'utilisation.
- netstat -uta : lister toutes les connexions ouvertes. Ce tableau vous indique qui, depuis l'adresse locale, est connecté à qui (à une adresse distante).
- netstat -ta : idem que -uta mais on enleve les connexions UDP.
- l'etat LISTEN : à l'écoute des connexions entrantes.
- l'etat ESTABLISHED : la connexion a été établie avec l'ordinateur distant
- regarder les ports sur lesquels ces connexions ecoutent (après le symbole « : » dans colonne "adresse locale") : on peut se connecter à chaque ordinateur via différentes « portes » appelées ports. Chaque service utilise un port différent (page web 80, fichier transfert 21, email 110). Ouvrir une page web et y aura plein de connexions established


#### 2. Savoir paramétrer un pare-feu est essentiel : Configurer le parefeu iptables

- Il permet d'établir un certain nombre de règles pour dire par quels ports on peut se connecter à votre ordinateur, mais aussi à quels ports vous avez le droit de vous connecter. Notre objectif est de bloquer par défaut toutes ces portes et d'autoriser seulement celles dont vous avez besoin, que vous considérez comme « sûres » et que vous utilisez. Par exemple, le port 80 utilisé pour le web est un port sûr que vous pouvez activer.
- Avoir un pare-feu ne vous prémunit pas contre les virus. En revanche, cela rend la tâche particulièrement difficile aux pirates qui voudraient accéder à votre machine.
- voir toutes les regles de config sur openclassroom

- [Tryhackme](https://tryhackme.com/room/linuxfundamentalspart1)

# Network Fundamentals
- [Tryhackme](https://tryhackme.com/paths)
- [openclassroom](https://openclassrooms.com/fr/courses/857447-apprenez-le-fonctionnement-des-reseaux-tcp-ip/853038-le-materiel-de-couche-2-le-commutateur)

### OSI Model
Le modèle OSI est une norme qui préconise comment les ordinateurs devraient communiquer entre eux. Modèle a 7 couches. Pour communiquer par courrier il faut un émetteur, récepteur, support de transmission (lettre) etc. Les chercheurs ont imaginé 7 éléments a mettre en place pour communiquer sur internet.

- La couche 1 ou **couche physique** :
Nom : physique.
Rôle : offrir un support de transmission pour la communication.
Rôle secondaire : RAS.
Matériel associé : le hub, ou concentrateur en français.
- La couche 2 ou **couche liaison** :
Nom : liaison de données.
Rôle : connecter les machines entre elles sur un réseau local.
Rôle secondaire : détecter les erreurs de transmission.
Matériel associé : le switch, ou commutateur.
- La couche 3 ou **couche réseau** :
Nom : réseau.
Rôle : interconnecter les réseaux entre eux.
Rôle secondaire : fragmenter les paquets.
Matériel associé : le routeur.
- La couche 4 ou **couche transport** :
Nom : transport.
Rôle : gérer les connexions applicatives.
Rôle secondaire : garantir la connexion.
Matériel associé : RAS.
- couche 5 et 6 : Modèle OSI = modèle théorique. Le modèle sur lequel s'appuie Internet aujourd'hui est le modèle TCP/IP. Or, ce modèle n'utilise pas les couches 5 et 6, donc... on s'en fiche !
- La couche 7 ou **couche application** :
Nom : application.
Rôle : RAS.
Rôle secondaire : RAS.
Matériel associé : le proxy.

Le modèle OSI ajoute deux règles :

--> chaque couche est indépendante : exemple, l'adresse IP qui est une adresse de couche 3 ne pourra pas être utilisée par une autre couche.

--> chaque couche ne peut communiquer qu'avec une couche adjacente : Si on entre l'adresse d'un site dans la barre de recherche. L'application (le navigateur) de couche 7, s'est adressé a la couche du dessous, on a parcouru les couches du modèle OSI de haut en bas.

L'encapsulation : un message est envoyé depuis la couche 7 du modèle OSI, et il traverse toutes les couches jusqu'à arriver à la couche 1 pour être envoyé sur le réseau. En fait, un en-tête va être ajouté à chaque passage par une couche. Au passage par la couche 4, on ajoutera l'en-tête de couche 4, puis celui de couche 3 en passant par la couche 3, et ainsi de suite. On encapsule un message dans un autre.  ce qui va circuler sur le réseau est une trame de couche 2, qui contient le datagramme de couche 3 (qui lui-même contiendra l'élément de couche 4).

#### 1. Couche 1 et 2 : communiquer dans un réseau local (LAN)

|couche| 1 |
|------|----|
| role | brancher les machines = acheminer des signaux électriques, des 0 et des 1 en gros. |
|fonctionnement | Les 0 et les 1 vont circuler grâce aux différents supports de transmission. Les câbles coaxiaux, la fibre optique. |
| topologie | Y a differents moyens de brancher les machines entre elles (la topologie en bus, la topologie en anneau, la topologie en étoile). |

|couche| 2 |
|------|----|
| role | faire communiquer les machines qui sont branchées sur un réseau local.|
| adresse | Chaque machine a une adresse MAC. L'adresse MAC est l'adresse d'une carte réseau (carte reseau contenue dans la machine). |
| protocole | Étant donné que nous discutons entre des machines très différentes, qui ont des OS différents, nous devons créer un langage de communication commun pour se comprendre (= protocole). Le protocole le + utilise sur la couche 2 est le protocole Ethernet. |
| fonctionnement protocole | Nous avons vu que des 0 et des 1 allaient circuler sur nos câbles. Nous allons donc recevoir des choses du genre : 001101011110001100100011111000010111000110001... Ce qui ne veut pas dire grand-chose tant que nous ne nous entendons pas sur leur signification. Le protocole va définir quelles informations vont être envoyées, et dans quel ordre. Par exemple on peut dire que les 48 premiers caractères que nous allons recevoir représentent l'adresse MAC de l'émetteur (puisque l'adresse MAC fait 48 bits) les 48 suivants l'adresse du récepteur, puis enfin le message. Le protocole va définir le format des messages envoyés sur le réseau. |
| nom des messages | Ce message est appelle une trame. La trame est le message envoyé sur le réseau, en couche 2. |
| materiel| Le commutateur (= switch) est un matériel qui va pouvoir nous permettre de relier plusieurs machines entre elles. Nous allons donc brancher nos machines au switch, voire d'autres switchs à notre switch. Pour envoyer la trame vers la bonne machine, le switch se sert de l'adresse MAC destination contenue dans l'en-tête de la trame.|


#### 2. Couche 3 : communiquer entre réseaux

Comment envoyer un message à un réseau auquel on est pas directement reliés ? Les réseaux sont tous reliés entre eux, comme une chaîne. Internet est un ensemble de réseaux collés les uns aux autres. Pour voir par quelles machines on passe pour aller jusqu'a une machine :
```
sudo apt install traceroute
traceroute www.siteduzero.com #chacune des lignes correspond à une machine que nous avons rencontrée
```

|couche| 3 |
|------|----|
| role | Communiquer entre réseaux|
| adresse | Adresse IP : l'adresse IP est en fait l'adresse du réseau ET de la machine. Une partie de l'adresse IP représentera l'adresse du réseau, et l'autre partie l'adresse de la machine. C'est le masque qui va indiquer quelle est la partie réseau de l'adresse, et quelle est la partie machine. |
| protocole | le protocole IP, ou Internet Protocol.|
| nom des messages | Pour le protocole IP, le message s'appelle un datagramme ou un paquet. (qui contiendra l'IP source et l'IP de la machine de destination)|
| materiel| routeur : va nous permettre d'envoyer un message en dehors de notre réseau. C'est une machine qui a plusieurs interfaces (plusieurs cartes réseau), chacune reliée à un réseau. Son rôle va être d'aiguiller les paquets reçus entre les différents réseaux.|

```
# Exemple : on associe l'adresse IP 192.168.0.1 au masque 255.255.0.0.
255.255.0.0 -> 11111111.11111111.00000000.00000000  #tous les bits a 1 = la partie reseau
192.168.0.1 -> 11000000.10101000.00000000.00000001  #tous les bits à 0 = la partie machine de l'adresse
```
Donc la partie réseau de l'adresse est 192.168, et la partie machine est 0.1.



### Wireshark
Wireshark est un sniffer. Un sniffer est un programme qui écoute sur le réseau, intercepte toutes les trames reçues par votre carte réseau, et les affiche à l'écran.
- on peut voir la liste des trames reçues lors d'une requête (couche 1 "Frame 187", la couche 2 "Ethernet", la couche 3 "IP" etc.),
- en cliquant sur l'une des trames on voit que Wireshark sépare les éléments de chacune des couches du modèle OSI
- on peut voir le contenu de chacune des couches en cliquant sur le triangle en face d'une couche

 
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
