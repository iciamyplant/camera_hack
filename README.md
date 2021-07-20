## plan
### I - Linux Fundamentals
- Les utilisateurs et les droits
- Installer des programmes avec apt-get
- Surveiller l'activité du système
- Exécuter des programmes en arrière-plan
- La connexion sécurisée à distance avec SSH
- Analyser le réseau et filtrer le trafic avec un pare-feu
### II - Network Fundamentals
#### OSI Model
- .1. Couche 1 et 2 : communiquer dans un réseau local (LAN)
- .2. Couche 3 : communiquer entre réseaux
- .3. Couche 4 : communiquer entre applications
- .4. Couche 7 : Applications 
- .5. protocoles DHCP, DNS, et HTTP
### III - Offensive Pentesting
#### Attaques
#### Etapes
#### .1. Footprinting and Reconnaissance
#### .2. Scanning Networks
- Ma machine
- Scanning
- Enumeration
- Vulnerability Analysis
#### .3. System Hacking
- Sniffing
- Hacking Wireless Networks
- Malware Threats, Denial-of-Service, Session Hijacking, SQL injection ..
### IV - Hack the Box
- Installation
- Methodo



# I - Linux Fundamentals
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

Sur mac :
```
#ap1: Access Point. This is used if you are using your MacBook as a wireless host where you are sharing its connection

#en0 at one point "ethernet", now is WiFi = wireless connection
#en1 is an ordinary ethernet connection
#en2 
#en3 is a connection using a Thunderbolt-to-ethernet adapter (adaptateur sur le mac)

#p2p0 : interface pour AirDrop (le p2p apple)
#awdl0: Apple Wireless Direct Link. WIFI p2p connection for things like AirDrop, Airplay, etc. Also used for Bluetooth

#bridged0 : un pont quelconque entre deux interfaces, possiblement du vmware
#fw0 is the FireWire network interface
#utun0: Tunneling interface. Used for VPN connections to tunnel traffic or for software like Back To My Mack.
```

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

# II - Network Fundamentals
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
| identifiant | Chaque machine a une adresse MAC. L'adresse MAC est l'adresse d'une carte réseau (carte reseau contenue dans la machine). |
| protocole | Étant donné que nous discutons entre des machines très différentes, qui ont des OS différents, nous devons créer un langage de communication commun pour se comprendre (= protocole). Le protocole le + utilise sur la couche 2 est le protocole Ethernet. |
| fonctionnement protocole | Nous avons vu que des 0 et des 1 allaient circuler sur nos câbles. Nous allons donc recevoir des choses du genre : 001101011110001100100011111000010111000110001... Ce qui ne veut pas dire grand-chose tant que nous ne nous entendons pas sur leur signification. Le protocole va définir quelles informations vont être envoyées, et dans quel ordre. Par exemple on peut dire que les 48 premiers caractères que nous allons recevoir représentent l'adresse MAC de l'émetteur (puisque l'adresse MAC fait 48 bits) les 48 suivants l'adresse du récepteur, puis enfin le message. Le protocole va définir le format des messages envoyés sur le réseau. |
| nom des messages | Ce message est appelle une trame. La trame est le message envoyé sur le réseau, en couche 2. |
| materiel| Le commutateur (= switch) est un matériel qui va pouvoir nous permettre de relier plusieurs machines entre elles. Nous allons donc brancher nos machines au switch, voire d'autres switchs à notre switch. Pour envoyer la trame vers la bonne machine, le switch se sert de l'adresse MAC destination contenue dans l'en-tête de la trame.|

--> Pour l'IP privee : au sein d'un même réseau, chaque machine possède une et une seule adresse IP

--> Pour l'IP publique : ordinateurs d'un meme reseau prive partagent une adresse publique (si on fait un test via un site What Is My IP? le site interrogera votre routeur ou votre Box et pas vos machines donc renverra l'IP publique).

--> Les adresses IP privees sont differentes entre une VM et mon pc

#### 2. Couche 3 : communiquer entre réseaux

Comment envoyer un message à un réseau auquel on est pas directement reliés ? Les réseaux sont tous reliés entre eux, comme une chaîne. Internet est un ensemble de réseaux collés les uns aux autres. Pour voir par quelles machines on passe pour aller jusqu'a une machine :
```
sudo apt install traceroute
traceroute www.siteduzero.com #chacune des lignes correspond à une machine que nous avons rencontrée
```

|couche| 3 |
|------|----|
| role | Communiquer entre réseaux|
| identifiant | Adresse IP : l'adresse IP est en fait l'adresse du réseau ET de la machine. Une partie de l'adresse IP représentera l'adresse du réseau, et l'autre partie l'adresse de la machine. C'est le masque qui va indiquer quelle est la partie réseau de l'adresse, et quelle est la partie machine. |
| protocole | le protocole IP, ou Internet Protocol. (mais y a aussi d'autres protocoles : ICMP, ARP...)|
| nom des messages | Pour le protocole IP, le message s'appelle un datagramme ou un paquet. (qui contiendra l'IP source et l'IP de la machine de destination)|
| materiel| routeur : va nous permettre d'envoyer un message en dehors de notre réseau. C'est une machine qui a plusieurs interfaces (plusieurs cartes réseau), chacune reliée à un réseau. Son rôle va être d'aiguiller les paquets reçus entre les différents réseaux, se différencie d'une simple machine, car il accepte de relayer des paquets qui ne lui sont pas destinés. Aiguille les paquets grâce à sa table de routage qui indique quelle passerelle utiliser pour joindre un réseau.|


```
# Exemple : on associe l'adresse IP 192.168.0.1 au masque 255.255.0.0.
255.255.0.0 -> 11111111.11111111.00000000.00000000  #tous les bits a 1 = la partie reseau
192.168.0.1 -> 11000000.10101000.00000000.00000001  #tous les bits à 0 = la partie machine de l'adresse
# Donc la partie réseau de l'adresse est 192.168, et la partie machine est 0.1.
```

```
route -n # afficher la table de routage
## La première ligne est pour notre réseau, et on voit une particularité de Linux qui n'indique pas notre adresse, mais 0.0.0.0. C'est comme ça.
La seconde est la route par défaut qui est ici 88.191.45.1.
route del default # enlever la route par defaut
route add default gw 10.0.0.254 # changer la route par defaut pour 10.0.0.254
route add -net 192.168.0.0 netmask 255.255.255.0 gw 10.0.0.253 #ajouter une route spécifique si nous le souhaitons pour aller vers le réseau 192.168.0.0/24 en passant par la passerelle 10.0.0.253
```

--> Une box Internet est constitué d'un modem ADSL et d'un routeur 

#### 3. Couche 4 : communiquer entre applications

Un serveur = offrir un service. Un serveur web va mettre à disposition des internautes des pages web. Un serveur de messagerie mettra à disposition des adresses mail ainsi qu'un service d'envoi et de réception de mails.

Étant donné que le serveur est censé fournir un service accessible tout le temps, on dit qu'il est en écoute. En fait, le serveur va écouter sur le réseau et être prêt à répondre aux requêtes qui lui sont adressées.

````
netstat -antp #tous les listen en train d'ecouter
#La colonne "Adresse locale" nous donne l'adresse IP en écoute, ainsi qu'un numéro que nous ne connaissons pas encore.
#La colonne "Etat" nous indique l'état du service
#La colonne "PID/program name" nous indique le numéro du processus en écoute ainsi que son nom.
#Si le service a une addr locale 127.0.0.1 : service non joignable depuis le reseau, joignable que depuis la machine elle-même, reserve a une utilisation locale.
````

Client = Oui, en ce moment même vous utilisez un client web qui est votre navigateur et qui se connecte au serveur du Site du Zéro.
 = Connexion client/serveur entre votre navigateur et le serveur web du Site du Zéro. Client de messagerie comme Outlook.
 
|couche| 4 |
|------|----|
| role |  objectif de faire dialoguer ensemble des applications, cad gérer les connexions applicatives|
| identifiant | le port : Le port c'est l'adresse d'une application sur une machine. On a 65535 ports à notre disposition, et pleins de ports reserves (web=80, mail=25, ssh=22, imap=143, proxy=8080, https=443...)|
| protocoles | 2 protocoles : TCP (applications qui nécessitent un transport fiable des données, mais qui n'ont pas de besoin particulier en ce qui concerne la vitesse de transmission par exemple les mails ou le web) et UDP (applications qui nécessitent un transport immédiat des informations, mais qui peuvent se permettre de perdre quelques informations. Par exemple le streaming ou la radio) |
| nom des messages | datagramme UDP (qui contient: Port Source=addrapplicationqui envoielinfo - Port Destination=addrapplicationquirecoitinfoparexmonnavigateur - Longueur totale - Checksum - Données à envoyer) et le segment TCP (Port Source - Port Destination - Flags - Checksum - Données à envoyer)|

#### 4. Couche 7 : Applications 
Elle est là pour représenter les applications pour lesquelles nous allons mettre en œuvre des communications. Les couches 1 à 4 sont appelées les couches "réseau". Ce sont elles qui ont la responsabilité d'acheminer les informations d'une machine à une autre, pour les applications qui le demandent.

#### 5. protocoles DHCP, DNS, et HTTP
- Protocole DHCP = Un protocole pour distribuer des adresses IP. La première fonction d'un serveur DHCP est de fournir des adresses IP aux machines en faisant la demande.
- Protocole DNS = Systeme de nomage pour eviter d'avoir a retenir toutes les adresses ip, se charge de convertir (on parle de résolution) le nom du site web demandé en adresse IP.
- Protocole HTTP = Le principe du protocole HTTP est de transporter ces pages HTML, et potentiellement quelques informations supplémentaires. Le serveur web met donc à disposition les pages web qu'il héberge, et le protocole HTTP les transporte sur le réseau pour les amener au client.



# III - Offensive Pentesting
- [Certification CEH v11](https://bookshelf.vitalsource.com/#/books/9781635675337/cfi/112!/4/4@0.00:36.5)
- [Tryhackme](https://tryhackme.com/paths)

### Attaques

| Attack| Description | Example|
|------|----|-------|
| Passive Attacks | interception et surveillance du trafic réseau et des flux de données sur le réseau cible sans modifier les données. Capturer les données ou les fichiers transmis sur le réseau sans le consentement de l'utilisateur (données non chiffrées en transit, informations d'identification en texte clair...) | **sniffing** (wireshark) and eavesdropping, footprinting|
| Active Attacks | altèrent les données en transit ou perturbent la communication ou les services entre les systèmes pour contourner ou pénétrer dans les systèmes sécurisés. Les attaquants lancent des attaques sur le système ou le réseau cible en envoyant activement du trafic pouvant être détecté. | **Ddos** (prendre pour cible un système informatique en l’inondant de messages entrants ou de requêtes de connexion afin de provoquer un déni de service), **Man-in-the-Middle**, **session hijacking** (technique consistant à intercepter une session TCP initiée entre deux machine afin de la détourner), **SQL injection** (injecter dans la requête SQL en cours un morceau de requête non prévu par le système et pouvant en compromettre la sécurité.), **malware attacks** (virus, ransomware..), **spoofing** (usurpation d'identite, se faire passer pour le routeur par ex), **password-based attacks** (cracker mdp), **Cryptography attacks**|
| Close-in Attacks | Lorsque l'attaquant se trouve à proximité physique du système ou du réseau cible. Objectif de collecter ou de modifier des informations ou d'en perturber l'accès. | Eavesdropping, **shoulder surfing** (regarder par-dessus l'épaule), **dumpster diving** (chercher dans la poubelle de quelqu'un des informations)|
| Insider Attacks | effectuées par des personnes de confiance qui ont un accès physique aux actifs critiques de la cible. Utiliser un accès privilégié pour enfreindre les règles ou provoquer intentionnellement une menace pour les informations ou les systèmes d'information de l'organisation.| **theft of physical devices** (vol d'appareils physiques) and **planting keyloggers** (enregistreurs de claviers), backdoors, malware, social engineering |
| Distribution Attacks | Lorsque les attaquants altèrent le matériel ou le logiciel avant l'installation. Par exemple les portes dérobées créées par les fournisseurs de logiciels ou de matériel au moment de la fabrication | modification of software or hardware during production |

### Etapes

| Etape |description|
|------|----|
|**1** | **Reconnaissance** : recolter le plus d'infos genre organization’s clients, employees, operations, network, and systems|
|**2** | **Scanning** : utiliser les détails recueillis lors de la reconnaissance pour scanner le réseau à la recherche d'informations spécifiques genre mapping of systems, routers, and firewalls by using simple tools such as the standard Windows utility Traceroute. L'analyse peut inclure the use of dialers, port scanners, network mappers, ping tools, vulnerability scanners, or other tools. Les attaquants extraient des informations telles que les machines actives, le port, l'état du port, les détails du système d'exploitation, le type de périphérique et la disponibilité du système pour lancer une attaque. scanners de vulnérabilités, qui peuvent rechercher des milliers de vulnérabilités connues sur un réseau cible. Si vulnerabilite direct ca donne un avantage à l'attaquant car il n'a qu'à trouver un seul moyen d'entrée|
|**3** | **Gaining Access** : Les attaquants utilisent les vulnérabilités identifiées lors des phases de reconnaissance et d'analyse pour accéder au système et au réseau cibles. The attacker can gain access to the OS, application, or network level (gain access to the target system locally (offline), over a LAN, or the Internet) avec differents types d'attaques. Une fois qu'un attaquant a accès au système cible, il essaie alors d'élever les privilèges afin de prendre le contrôle total. Ce faisant, ils compromettent également les systèmes intermédiaires qui y sont connectés.|
|**4**| **Maintaining Access** : Une fois qu'un attaquant a accès au système cible avec des privilèges d'administrateur il peut utiliser à la fois le système et ses ressources à volonté. Les attaquants qui choisissent de ne pas être détectés suppriment les preuves de leur entrée et installent une porte dérobée ou un cheval de Troie pour obtenir un accès répété. Ils peuvent également installer des rootkits au niveau du noyau pour obtenir un accès administratif complet à l'ordinateur cible. Les rootkits accèdent au niveau du système d'exploitation, tandis que les chevaux de Troie accèdent au niveau de l'application. Les rootkits et les chevaux de Troie nécessitent que les utilisateurs les installent localement.|
|**5**| **Clearing Tracks** : utilisent des utilitaires tels que PsTools, Netcat ou des chevaux de Troie pour effacer leurs empreintes des fichiers journaux du système. D'autres techniques incluent la stéganographie et la tunnellisation. Les administrateurs système peuvent déployer des IDS (systèmes de détection d'intrusion) et un logiciel antivirus basés sur l'hôte afin de détecter les chevaux de Troie et autres fichiers et répertoires apparemment compromis |

### 1. Footprinting and Reconnaissance
### 2. Scanning Networks

####  Ma machine

| ifconfig | Ping | Route |
|----------|-------|--------|
| - La première est l'interface eth0 (eth pour Ethernet !) qui est ma carte réseau. - Enfin nous avons l'interface lo (pour local, ou loopback) qui est une interface réseau virtuelle qui n'est accessible que sur la machine elle-même. Son adresse est toujours 127.0.0.1, sur toutes les machines. C'est une convention. - Si ifconfig indique inet adr:88.191.45.68  Bcast:88.191.45.255  Masque:255.255.255.0 : adresse IP 88.191.45.68, masque 255.255.255.0 et l'adresse de broadcast 88.191.45.255.  | Machine emetrice du ping fait un ICMP echo request, machine receptrice fait un ICMP echo reply (ping utilise le protocole ICMP). Permet de savoir si la machine pingée est correctement joignable sur le reseau. + mesure le temps necessaire au paquet pour faire l'aller retour entre la machine emetrice et la machine receptrice. | pour avoir mon gateway (= routeur)|


````
ifconfig
ifconfig eth0 10.0.0.1 netmask 255.255.255.0 #modifier son adresse et la remplacer par 10.0.0.1/24 
````
````
ping 8.8.8.8 #je ping le serveur DNS de google
````
````
route
````

si je suis sur VM attention reglages reseau [explication](https://chrtophe.developpez.com/tutoriels/gestion-reseau-machine-virtuelle/)
- soit jutilise le LAN (et j'ai addresse IP de type 10.0.2.2)
- soit j'utilise un pont avec ma machine hote. Dans ce mode, la carte réseau virtuelle est « pontée » à une carte réseau physique de l’hôte. Cette carte réseau aura donc 2 adresses IP, une dédiée à l'hôte et l'autre dédiée à la machine virtuelle. 


[calculateur ip et infos](http://www.hobbesworld.com/reseaux/calcip.php#gene) 

[CIDR masque de sous-reseau](https://fr.wikipedia.org/wiki/Sous-r%C3%A9seau)


####  Scanning

| Port Scanning | Network Scanning | Vulnerability Scanning |
|----------|-------|--------|
| Répertorie les ports et services ouverts. L'analyse des ports consiste à se connecter ou à sonder les ports TCP et UDP du système cible pour déterminer si les services sont en cours d'exécution ou sont en état d'écoute. L'état d'écoute fournit des informations sur l'OS (=fingerprinting) et l'application en cours d'utilisation. Parfois, les services actifs à l'écoute peuvent permettre à des utilisateurs non autorisés de mal configurer les systèmes ou d'exécuter des logiciels présentant des vulnérabilités. | Répertorie les hôtes actifs et les adresses IP.| Affiche la présence de faiblesses connues. Un scanner de vulnérabilité se compose d'un moteur d'analyse et d'un catalogue. Le catalogue comprend une liste de fichiers communs avec des vulnérabilités connues et des exploits communs pour une gamme de serveurs. les ports sont les portes et les fenêtres d'un système qu'un intrus utilise pour y accéder.|

[doc de reference nmap](https://nmap.org/man/fr/index.html)
`````
###sur l'outil nmap : 
outil open source d'exploration réseau, a été conçu pour rapidement scanner de grands réseaux, fonctionne aussi très bien sur une cible unique. 
Détermine quels sont les hôtes actifs sur le réseau, quels services (y compris le nom de l'application et la version) ces hôtes offrent, 
quels systèmes d'exploitation (et leurs versions) ils utilisent, quels types de dispositifs de filtrage/pare-feux sont utilisés, 
ainsi que des douzaines d'autres caractéristiques.
###
nmap [ <Types de scans> ...] [ <Options> ] { <spécifications des cibles> }
-A: Active la détection du système d'exploitation et des versions
sP: Ping Scan - Ne fait que déterminer si les hôtes sont en ligne
--traceroute: Détermine une route vers chaque hôte
nmap -sP *addrreseau**masquesousreseauCIDR*
`````

```
hping3 <options> <target IP address> #can be used for network security auditing, firewall testing, manual path MTU discovery, advanced traceroute, remote OS fingerprinting, remote uptime guessing, TCP/IP stacks auditing, etc.
#default is TCP
hping3 -1 #ICMP
hping3 -A #ACK
hping -2 #UDP 
```

####  Enumeration
Processus d'extraction des noms d'utilisateur, des noms de machine, des ressources réseau, des partages et des services d'un système ou d'un réseau. Selon les ports ouverts, on va enumerer :

Techniques to extract informations :
- **Extract usernames using email IDs**
- **Extract information using default passwords** : nombreuses ressources en ligne fournissent une liste de mots de passe par défaut attribués par les fabricants à leurs produits
- NetBIOS Enumeration (port 139 + sur windows)
- **SNMP Enumeration (UDP 161)**
- **NTP and NFS Enumeration**
- **SMTP and DNS Enumeration** : Un administrateur réseau peut utiliser le transfert de zone DNS, avec commandes nslookup et dig, pour répliquer les données DNS sur plusieurs serveurs DNS ou sauvegarder des fichiers DNS. Dans cette operation il convertira tous les noms DNS et adresses IP hébergés par ce serveur en texte ASCII. Si le serveur DNS est mal config, le transfert de zone DNS peut être une méthode efficace pour obtenir des informations sur le réseau de l'organisation :listes de tous les hôtes nommés, sous-zones et adresses IP associées.
- **LDAP Enumeration** : Microsoft Active Directory aurait une erreur de conception. toutes les tentatives d'authentification du service entraînent des messages d'erreur différents. Les attaquants en profitent pour énumérer des noms d'utilisateur valides, et s'ils trouvnet un nom d'utilisateur valide ils brute fore le mdp.

```
############Exemple d'enumuration############
#####Si le systeme cible est sur Windows#####
###et qu'il a le port 139 ouvert = Netbios###

nbtstat [-a RemoteName] [-A IP Address] [-c] [-n] [-r] [-R] [-RR] [-s] [-S] [Interval]
nbtstat -a <ip addres of the remote machine>
nmap -sV -v --script nbstat.nse <target IP address>
##Enumeration of user accounts with Pstools##
PsExec - executes processes remotely 
PsFile - shows files opened remotely 
PsGetSid-displays the SID of a computer or user 
PsKill - kills processes by name or process ID 
PsInfo - lists information about a system 
PsList - lists detailed information about processes
PsLoggedOn - shows who is logged on locally and via resource sharing
PsLogList - dumps event log records 
PsPasswd - changes account passwords
PsShutdown - shuts down and optionally reboots a computer
```

```
############Exemple d'enumuration############
#######Si le systeme cible utilise SNMP######
###############port 161 ouvert###############

Snmpcheck
SoftPerfect Network Scanner
```

####  Vulnerability Analysis

acronyme CVE = désigne une liste publique de failles de sécurité informatique. Lorsque l'on parle d'une CVE, on fait généralement référence à l'identifiant d'une faille de sécurité répertoriée dans cette liste. Les identifiants CVE sont attribués par des autorités déléguées, les CNA (CVE Numbering Authority). Les CNA disposent de blocs d'identifiants CVE alloués par le MITRE, qui sont réservés et attribués aux failles au moment de leur découverte. 

```
OpenVAS #scanner de vulnerabilites
Nessus #logiciel scanner de vulnerabilites
Nikto #scanner vulnerabilites d'un serveur web
metasploit
```

### 3. System Hacking
#### Sniffing

Wireshark est un sniffer. Un sniffer est un programme qui écoute sur le réseau, intercepte toutes les trames reçues par votre carte réseau, et les affiche à l'écran.
- on peut voir la liste des trames reçues lors d'une requête (couche 1 "Frame 187", la couche 2 "Ethernet", la couche 3 "IP" etc.),
- en cliquant sur l'une des trames on voit que Wireshark sépare les éléments de chacune des couches du modèle OSI
- on peut voir le contenu de chacune des couches en cliquant sur le triangle en face d'une couche

[tuto wireshark](https://openclassrooms.com/fr/courses/857447-apprenez-le-fonctionnement-des-reseaux-tcp-ip/855562-rendre-mes-applications-joignables-sur-le-reseau)

Tcpdump est un sniffer, comme wireshark, mais en ligne de commande sous linux.
 est un sniffer. C'est un programme qui est capable d'écouter toutes les trames qui arrivent sur notre carte réseau et de nous les afficher à l'écran (comme wireshark, mais en ligne de commande sous linux).
````
tcpdump -i eth0 icmp
````

#### Hacking Wireless Networks
- ARP poisoning using Ettercap, MiTM attack
ARP poisoning is an attack that is accomplished using the technique of ARP spoofing

#### Malware Threats, Denial-of-Service, Session Hijacking, SQL injection ..

# IV - Hack the Box
### Installation
Se connecter au VPN [tuto](https://www.youtube.com/watch?v=msCWpKegNlc)

```
sudo apt-get install network-manager-openvpn
sudo openvpn --config /path/to/file.ovpn
# initialization sequence completed = OK
```

### Methodo

```
######sur mon reseau######
ifconfig
curl ifconfig.me #addresse IP publique sous mac
ipconfig getifaddr en0 #adress IP privee sur mon mac
hostname -I #sous linux pour laddr IP privee
nmap -sP *ip reseau*/*masque sous reseau* #connaitre le nombre de terminaux connectés aux wifi et leurs adresses IP ("... hosts up")
```
```
######en savoir plus sur la cible######
ping *addr ip cible* #Permet de savoir si la machine pingée est correctement joignable sur le reseau
nmap -A *addr ip cible* #Active la détection du système d'exploitation et des versions (nous donnes les ports ouverts)
```
```
######vulnerabilites connues######
nmap --script=vuln *addr ip cible* #vulnerabilite connue, --script=<lua scripts>: ici <lua scripts> est une liste de répertoires ou de scripts séparés par des virgules
searchsploit *nom du service qui tourne*
msfconsole search *nom du service qui tourne*
```
```
######exploit######
msfconsole search cve:2003-0352 #chercher un exploit qui correspond a une faille par son CVE
show exploits #afficher tous les exploits disponibles sur Metasploit
search nom_exploit #pour chercher un exploit
use nom_exploit #par exemple : use exploit/windows/dcerpc/ms03_026_dcom (path sous name quand on a search avec le cve)
show targets
set target *chiffre target* #Configurer la target
info nom_exploit #Avoir des informations sur un exploit
show options #Voir les options d’un exploit
```
