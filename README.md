
# Firewall

## Mots clés :

* DMZ : Zone Démilitarisée (Définit #10)
* Pare-feu : Logiciel / matériel permettant de faire respecter la politique de sécurité du réseau. (mûr de brique)
* NGFW : Pare-Feu nouvelle génération 
* SI : Système d'information, c'est l'ensemble des ressources qui permettent de collecter, stocker, traiter l'information. Une partie est le personnel (liées au SI) et l'autre est la technologie (hardware et software).
* DSI : Directeur des système d'information, c'est le responsable de l'ensemble des composants matériels (postes, serverus, équipements réseaux, stockage, sauvegarde et logiciels du SI)
* Carte heuristique : ou carte mentale, est un schéma reflétant le fonctionnement de la pensé. C'est un diagramme qui représente l'organisation des liens sémantiques entre idées et concepts.
* Firewalling : Faire du firewall
* Access list :  Liste d'accès -> Ici ACL : Liste @ & ports autorisés / interdits par un pare-feu.
* Système de gestion unifiée : UTM (Unified thread management) décrit les pare-feu réseaux qui possèdent des fonctionnalités supplémentaires qui ne sont pas disponible dans les pare-feu traditionnel.
* Sécurisation périmétrique : cf 9
* Sécurité en profondeur : cf 9
* ASA : Arbre Syntaxique Absrait, arbre dont les noeuds internes sont marqués par des opérateurs donc les noeuds externes (feuilles) représentent les opérandes.
* UTM : Unified Threat Management
* Fortinet : Multinationale américaine dont le siège social est en Californie. Commercialise principale de la cyber-sécurité (pare-feu / antivirus) -> Quatrième au rang mondial en chiffre d'affaire.
* Net filter : Frameworl implémentant un pare-feu au sein du noyau linux. 
* Appliance : Application ?
* IP table : Logiciel libre de de Linux, grâce auquel un admin peut configurer les chaînes et règles dans pare-feu. Situé dans /usr/sbin/iptables.
* Kerio : Entreprise spécialisé dans les logiciels, pour les petites et moyennes organisations. HQ en Californie.
* check point : Fournisseur mondial de solutions de sécurités informatiques, pionnier des pare-feu avec FireWall 1 et sa technologie "stateful inspection"
* Pfsense : Routeur / Pare feu Open Source de FreeBSD. [...]
* IPCop : Distribution Linux qui fournit un pare-feu  simple à gérer sur un PC. (Famille, petites ou moyennes entreprises), offrant DMZ ainsi que VPN
* IPFire : Distribution Linux, "Linux From Scratch" faisant office de pare-feu.

## Contexte :

**Quoi ?**

* Mettre en place un firewall
* Protéger le serveur de la DMZ
* Modifier l'architecture
* Gérer les accès

**Pourquoi ?**

* Sécuriser l'accès au SI
* Empêcher les attaques sur son site

**Comment ?**

* En mettant en place un système de gestion unifiée des menaces
* Aligner les politiques de sécurité avec la gouvernance de SI

## Contraintes :

* aligner les politiques de sécurité avec la gouvernance de SI
* coût faible
* Le serveur est dans une DMZ

## Problématique :

**Comment mettre en place une politique de sécurité à l'aide d'un pare-feu ?**


## Généralisation :

Sécurité

## Hypothèses :

* IPFire est basé sur Linux
* DMZ = demilitarized zone
* DMZ sécurise tout en les isolant
* On peut avoir un pare-feu applicatif t un sur le routeur
* Un pare-feu peut être logique ou physique
* L'IP table sert à créé des pare-feux et des ACLs
* Les ACLs permettent de filtrer le trafic
* Un pare-feu sert à filtre le trafic
* Le pare-feu est basé sur les ACLs
* On peut utiliser un prxy pour faire office de pare-feu
* Il existe des distributions linux permettant de monter des pare-feux gratuitement
* Une bonne pratique est d'interdire tout par défaut et autoriser ce qu'on veut laisser passer

## Plan d’action :

### Etudes

 ACLs

NGFW

Firewalls (fonctionnement)

DMZ

Différents périmètres de défense

### Réalisations

Définir les critères du firewall

Comparer les pare-feux

Configurer le pare-feu (packet-tracer)


## 1  - Généralités sur les firewall

**Filtrage simple** -  Stateless

Au niveau de la couche réseau et transport, c'est accepter ou refuser le passage d'un paquet en se basant sur :
- @IP S/D
- N° Port S/D
- Protocole n3+

Pour se faire, on utilise des ACL (Access Control List)

MAIS

L'admin est contraint d'autoriser un trop grand nombre d'accès pour que le Firewall offre une réelle protection en utilisant un ACL, ce qui peut laisser passer un certain nombre d'attaques pirates, comme IP Spoofing, ou IP Flooding, la mutilation de paquet ou encore DoS.

**Filtrage de paquet avec état** -  Stateful

Amélioration du filtrage simple, on conserve la trace des sessions et des connexions dans des tables d'états internes. Le firewall prend alors ses décisions en fonction des états de connexion et peut réagir danss le cas de situations anormales. Permet de se protéger contre les attaques de DoS (Trop de fois la même co -> block)

**POUR** le protocole UDP et ICMP : pas de mode "connecté", on fait donc :
- Autoriser pendant un certain délai les réponses légitimes aux paquets envoyés
- Paquets ICMP sont bloqués par le Firewall
Pas besoin de bloquer les paquets de tyes 
 (Destination unreachable) et (ralentissement de la source), non utilisable pou un attaquant 
 http://www.frameip.com/wp-content/uploads/firewall-filtrage-paquet-etat-1.jpg

**POUR** le protocole TCP : Il faut gérer deux connexions
- Laisser passer le flux de données établi par le serveur
- Doit connaître le protocole FTP et tous les protocoles sur le même principe
- Appelée 'Filtrage dynamique (Stateful Inspection)" 

MAIS

\\§/ Constructeurs 
- Aucun filtrage sur les requêtes et réponses 

**filtrage applicatif** (proxy ou proxying applicatif)

Réalisé au niveau de la couche application, les requêtes sont traitées par des processus dédiés. Par exemple, une requête de type HTTP sera filtrée par un processus proxy Http (rejetant toute les autres). Le proxy doit connaître toute les règles protocolaire qu'il doit filtrer.

MAIS :

Extrêmement difficile de pouvoir réaliser un filtrage qui ne laisse rien passer (trop de protocoles).
Bien qu'il apporte plus de sécurité que le filtrage de paquet avec état, mais cela se paie en performance.

On ne peut donc pas proposer une technologie 100% proxy pour les réseaux à gros trafic au jour d'aujourd'hui.


**QUE FAIRE ALORS ? **

http://www.frameip.com/wp-content/uploads/firewall-filtrage-applicatif-choisir.jpg

Il faut protéger son proxy par un stateful inspection, et éviter d'installer les deux types de filtrage sur un même firewall. Permet de se protéger contre l'ARP spoofing.

Choisir sa politique :

- Tout autoriser sauf ce qui est dangereux (beaucoup trop laxiste, laisse passer l'imagination, à éviter ABSOLUMENT)
- Tout interdire sauf ce dont on a besoin : Beaucoup plus sécuritaire. Le firewall est pour se protéger des zones extérieures. Cependant, plus on autorise, plus on est vulnérable.

## 2  - Les différents types de firewall

**Firewall Bridge**

Agissent  comme de vrais câbles réseaux (filtrages).
Ce sont des interface sans @IP qui transfèrent d'une interface à une autre en leur appliquant des règles prédéfinies.

- Impossible de l'éviter (avantage)
- Peu couteux (avantage)
Mais
- Possiblité de le contourner 
- Configuration restraignante
- Très basique (filtrage sur une @IP, port)

**Firewall matériel**

Routeurs achetés dans le commerce, font office de boîte noire. L'avantage, est que leur interaction avec les autres fonctionnalités du routeur est simplifiée de part leur présence. Peu flexible, car leur code est inaccessible, ils sont vulnérable aux attaques. 
Ce système n'est implémenté que dans les Firewall haut de gamme, car cela évite de remplacer le logiciel par un autre produit du fabricant. Dépendant du constructeur pour les MAJ.

- Intégré au matériel réseau
- Administration simple
- Bon niveau de sécurité
MAIS
- Dépendant du constructeur
- Souvent peu flexibles

**Firewell logiciel**

- Personnel : Commerciaux, sécurise un ordi en particulier et non un groupe d'ordi. Peuvent-être contraignants et parfois très peu sécurisés.
	- Sécurité en boût de chaine (poste client)
	- Personnalisable assez facilement
MAIS
	- Facilement contournable
	- Difficile à départager de part leur nombre
- Firewall "sérieux"
Tourne sous linux, car cet OS offre une sécurité réseau plus élevée et un contrôle plus adéquat, ont pour but d'avoir le même comportement que les firewalls matériel des routeurs, mais configurable à la main. Ils utilisent le noyau linux.
	-	Personalisable
	-	Niveau de sécu très bon
MAIS 
	- Nécessite une administration système supplémentaire

GLOBALEMENT :
Firewall logiciel = grande faille => N'utilisent pas la couche réseau, il suffit donc de passer outre le noyaux en ce qui concerne la récupération de ces paquets (avec une lib spéciale) pour récupérer les paquets qui auraient été normalement "perdu" par le firewall si on accède au PC.


## 3  - Attaque, outils, défenses - Permettre une backdoor


backdoor = porte cachée sur système qui permet d'en prendre le contrôle à distance

- 1 : Pas de protection 
![](http://www.frameip.com/wp-content/uploads/firewall-attaque-outils-defenses-protection.jpg)
Pas de protection

- 2 : Filtrer les flux entrants
Bien qu'on est mis un firewall, le pirate a pris le soin de s'arranger pour que sa backdoor initie elle même les sessions, le firewall laisse passer les requêtes de l'attaquant qui sont considérées comme des réponses par celui-ci.

![](http://www.frameip.com/wp-content/uploads/firewall-attaque-outils-defenses-filtrer-flux.jpg)

3- Bloquer les flux entrants et sortants :

Le problème est dû aux flux sortant, qui permet au "cheval de troie" d'initier des sessions avec la machine de l'attaquant, la seul défense insère donc un proxy afin de contrôler qui sort du réseau.
Malheuresement, le 'trojan" (cheval de troie)  peut encore sortir car il devra se renseigner sur les flux autorisés à sortir par le proxy, et ainsi passer le proxy.
![](http://www.frameip.com/wp-content/uploads/firewall-attaque-outils-defenses-bloquer-flux-entrant.jpg)

4 - Protection locale via un firewall personnel 

En survelllant le trafic entrant et sortant d'une mahcine infectée, on peut :
-  Passer au dessus du Firewall via une application autorisée (utilisant des thread) permettant d'injecter du code sous Windows
- Passer en dessous du Firewall via une bibliothèque adaptée (Winpcap sous Windows ou pcap sous Linux)

5 - Piratage de VPN

![](http://www.frameip.com/wp-content/uploads/firewall-attaque-outils-defenses-piratage-vpn.jpg)
On peut accéder au réseau de l'entreprise via le VP.

## 4  - Après la backdoor - Rootkit

APRES AVOIR UTILISE UNE BACKDOOR, celui ci va installer un rootkit.
Il existe deux types de rootkit : Simple et noyaux.

Les rootkit simple se contentent de remplacer une collection de programmes systèmes (ps, netstat, ifconfog) lui permettant d'effacer les traces de son passage et masquer la backdoor.

Des outils comme **Tripwire** permet de vérifier l'intégrité des commandes.

Les rootkit noyaux sont beaucoup plus difficiles à détecter, ils modifient le noyaux, il peut rendre invisible un processus à chaque appel, modifie les routines de journalisation du système, masque les connexions réseau...

L'idéal est de patcher son noyau pour empêcher l'installation d'un rootkit et de désactiver les LKM (Loadable Kernel Modules) qui permettent au root d'introduire un nouveau code dans le système d'exploitation pendant que ce dernier est en cours d'exécution). Il existe également un outil Linux pour faire de nombreuses vérifications "modules" de backdoor, nomée 'rkscan', et qui vérifie les versions de rootkits les plus populaires.

## 5 - Quelques attaques à connaître :

- IP Spoofing : Modifier les paquets IP pour passer le firewall. => Contrer avec IPSec qui va chiffrer les entêtes des paquets
- DOS et DDOS : Envoyer le plus de paquets possibles vers un serveur générant beaucoup de trafic inutile, bloquant l'accès aux utilisateurs normaux, provient la majorité d'un virus, très difficilement évitable. Certains firewall peuvent s'en protéger.
- Port Scanning : Consiste à déterminer quels ports sont ouverts afin de déterminer les vulnérabilité du système. Le Firewall les bloques en les annonçants comme "fermé. Il suffit de bloquer l'adresse qui fait le port scanning.
- Exploit : Exploiter les vulnéaribilités des logiciels installés, comme un serveru HTTP, FTP... La seule solution est les MAJ logiciel au fur et à mesure des découvertes.


## 6 - Connaître les pares-feu :


**Versions .Libre :**
- iptables / Linux Netfilter :
	- Liste le contenu des règles
	- Rapide (ne regarde que les header)
	- Modifier facilement
- Linux IpChains (Pare-feu ancien noyau)
- Packet Filter (PF) de OpenBSD
- IPFilter (IPF) de BSD et Solaris
- Ipfirewall ou IPFW de FreeBSD
- iSafer pour Windows

- Smoothwall Express => Linux
	- Admin. web
	- Etat machine sur pare feu (Mémoire + Dd)
	- Supervision trafic
	- Proxy 
	- Dhcp / Dns / Ntp (Temps)
	- SSH
	- Détection intrusion
	- VPN IPSec
	- Filtrage (Par état)
	- NAT
	- Priorité trafic & QoS (Qualité of Service)
	- Logs
	- MAJ
- IPCop => Linux
	-  Pas de proxy
	- Supporte le VLAN trunking
	- Supporte telnet
	- Pas de supervision de trafic en temps réeel
	- Ajout d'add-on possible
	- A peu près similaire à Smoothwall 
-  Vyatta Community Edition
	- Basé sur le noyau Debian
	- Services de haute disponiblitié
	- Beaucoup de services de routage
	- VPN / SSL ...
	- IPS (Système de prévention d'intrusion) à la place d'un IDS (Système de détection d'intrusions)

- M0n0wall
	- OS pare-feu sur le noyau FreeBSD et non Linux
	- Démarre à partir d'une séquence de boot basée sur des fichiers d'extension .php et non shell classique
	- Stock toute sa config dans un .xml
	- Limitté dans les fonctionalités (Pas IDS/  IPS / FTP / Proxy / Serv. temps...)
	- Présence SNMP (?)
	- Filtrage inspiré du pare-feu logiciel FreeBSD, utilisant "IPFW" (plus complet que le reste des FreeBSD).
- PFSense => Linux
	- Descendant de m0n0wall : OS basé sur FreeBSD aussi, et comporte le OPFW...
	- CARP (Common Address Redundency Protocol) 
	- PFsyn (Syncrhonisation entre machines PFSense)
	- Possibilité d'Alias étendue
	- XML syncrhonisé entre maître et hôte de backup (point d'admin pour un cluster)
	- Load Balancing entre trafic entrant et sortant
	- Graphes de files d'attentes
	- SSH
	- Interfaces WAN
	- Serveur PPPoE
- NuFW  (Pare-feu identifiant) => Windows
	- Pare-feu logiciel
	- Extension iptables Netfilter (pare feu linux)
	- Authentification filtrage classiques
	- Nécessite des clients pour l'authentification auprès du pare-feu.
- Shorewall => Linux
	- Utilise la connexion Netfilter pour traquer facilement le filtrage
	- Supporte une grande variété de routeur / Firewall...
	- Admin centralisé
	- Interface GUI web
	- Support FAI
	- Supporte le masquage & le port forwarding
	- Supporte le VPN 
- UFW (Uncomplicated Firewall)
	- Pare feu par défaut pour les serveurs Ubuntu
	- Supporte l'IPV6
	- Fonctionnalités de login étuendues
	- Monitoring
	- Peut-être couplé à des applis
	- Ajouter / Retirer des règles selon les nécessités
- Vuurmuur :=> Linux
	- IPV6
	- Règlement sur le trafic
	- Monitoring
	- Règlage de la bande passante
	- NAT facilement
	- Anti-Spoofing 
- Endian :
	- Firewall 'Bidirectionnel'
	- Détecte les intrusions
	- Peut sécuriser les serv web avec des Proxy HTTP / FTP / Antivirus / URL blakcklist
	- Peut sécuriser les serv mail avec SMPT, POP3 proxies...
	- VPN avec IPSec
	- Logs sur le trafic
- ConfigServer Security Firewall 
	- daemon process LFD (Login Failure Daemon) qui regarde les échecs de logs dans les services "fragiles"
	- Configure des email alert si quelque chose est détecté, ou si il y a une instrusion
	- Peut-être facilement intégré avec des panneaux de contrôles comme "cPanel, Direct Admin ou Webmin".
	- Notifie par mail l'utsage de ressources excessives"
	- système avancé pour détecter l'intrusion
	- Facile à utiliser (démarage/ Stop...)

**Récapitulatif** :

https://saboursecurity.wordpress.com/2010/12/30/quelques-solutions-pare-feu-open-source/


## 7 - Firewall de new generation :

Un peu de lexique :
- UTM = Technologies clé en main (outils prêts à l'emploi), protection des email, sécu des points d'accès wifi, VPN
- NGFW (Next Generation Firewall) : Firewall (filtrage DPI) accompagnés de détection d'intrusion (IPS) & contrôle app web (filtrage URL), techniques manuelles

![](https://www.watchguard.com/sites/default/files/ngfw-utm-scale_fr.png)

\\!/ La plupart des UTM surpassent des solutions NGFW.


De nos jours (La cinquième génération),  le pare-feu doit être proche de l'utilisateur et de son poste de travail, afin de pouvoir faire des **analyses comportementales** de l'usage de ses outils et de ses applis.  Pour y contribuer, on conçoit des pare-feu bien spécifique, appellé "Nouvelles génération" (NFGW)

Un pare-feu de nouvelle génération doit comporter :
- Base :
	- "FW, SPI, VPN (chiffrement et accélération) IPSEC / SSL / L2TP / PPPoE / PPPoA / GRE..., Xauth, PKI (...), IDS/IPS, routage, VRRP, Multicast, Filtrage Web, Mail Security, Antivirus, NAT, VoIP "Hybrid networking", Proxy, load sharing, cache, teleprovisionning, streaming, Client logiciel basique, connecteurs Tacacs, LDAP, Radius..."
- Avancé :
	- "Reconnaissance applicative, gestion de l'identité, Data Loss Prevention (DLP), routage avancé et QoS, Corrélation d'information dynamique et consolidation de rapports automatiques et personnalisables pour plusieurs milliers de pare-feu simultanément... (couplé avec des fonctions de monitoring, supervision et alerting évolué), Chiffrement intégrale des postes de travail, client logiciel évolué de plus en plus multifonctions, virtualisation..."

Conclusion 
- Plus un pare feu, mais un ensemble d'élements de sécurité permettant de garantir un niveau de sécurité admissible en fonction **du profil, des infrastructures, des terminaux...**

## 8 - Les ACL :

- Liste de règles permettant de filtrer / autoriser trafic
- Autoriser ou bloquer
- Une ACL par interface et par sens
- Analisé par L'IOS de manière séquencielle
- ACL bloque par défaut tout le trafic


On retrouve
- ACL Standard : En fonction de l'@ IP source
- ACL Etendues :  IP Sources / Dest / Protocoles / Port


Rappel  des commandes : https://www.ciscomadesimple.be/wp-content/uploads/2011/06/CMSBE_F04_ACL.pdf

## 9 - Les types de défenses (zone)

- Défense périmétrique :
	- Bloquer au moyen de pare-feux & règles routages accès non autoriés & aux systèmes de l'entreprise
MAIS INSUFISANT à cause de la volatilité du périmètre, on doit la compléter par une **défense en profondeur**
- Ordi inconnues -> Cantonnés dans un sous-réseau virtuel aux droits limités
- Utilisateurs sensibles pas admins de leurs ordis
- Connexions aux serveurs ne sont pas autorisés avec des comptes privilégiés ni à distance
- Utiliser des systèmes d'authentification comme Kerberos.

## 10 - DMZ

DMZ : Zone démitilarisée, est un sous-réseau séparé du réseau local  par un pare-feu, qui contient une zone de service, qui peut-être d'ordre applicatif (web, messagerie) ou sécurités (proxy / reverse proxy), contant les machines susceptibles d'être accédées depuis l'internet.

Ainsi, si un pirate compromet l'un des services dans la DMZ, le pirate n'aura accès qu'aux machines de la DMZ et non au réseau local, en vue que le pare-feu bloque les accès au réseau local.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Demilitarized_Zone_Diagram.png/290px-Demilitarized_Zone_Diagram.png)



