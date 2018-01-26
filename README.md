# Firewall

## Mots clés :

* DMZ : (Demilitarized Zone) un sous-réseau, séparé du réseau local et isolé de celui-ci et d'Internet par un pare feu. Il contient des machines suceptible d'être attaquées via Internet. De fait, un pirate n'aura accès qu'à cette zone démilitarisée en ca de compromission d'un ordinateur.
* Pare-feu : Symbolisé par un mur de briques
* NGFW : New Generation FireWall
* SI : Système d'information, ensemble organisé de ressources qui permet de collecter, stocker, traiter et distribuer de l'information
* DSI : Directeur des Systèmes d'Information, responsable de l'ensemble des composants matériels du système d'information d'une entreprise
* Carte heuristique : Une carte heuristique, ou carte mentale, est un schéma, supposé refléter le fonctionnement de la pensée, permettant de représenter visuellement et de suivre le cheminement associatif de la pensée
* Firewalling : Disposition d'un pare-feu filtrant les paquets et n'acceptant que ceux qui correspondent aux attentes
* Access list : Critères d'accès à un réseau (configuré sur un routeur)
* Système de gestion unifiée : UTM
* Sécurisation périmétrique : Antivirus de flux ; Passerelle Firewall VPN ; Firewall UTM (Unified Threat Management) ; IPS (Intrusion Prevention System) / IDS.
* Sécurité en profondeur
* ASA : Logiciel Cisco Adaptive Security Appliance, système d'exploitation de la gamme ASA, pare-feu Cisco
* UTM : United Threat Management, approche de la gestion de la sécurité qui permet à un administrateur de suivre et de gérer un large éventail d'applications de sécurité et de composants d'infrastructure à partir d'une seule et même console.
* Fortinet : Entreprise américaine fournissant des équipements de protection du réseau (firewall, wi-fi sécurisés, etc)
* Netfilter : Module Linux permettant de filtrer et de manipuler les paquets sous-réseau passant dans le système
* Appliance
* IP table : Interface en ligne de commande permettant de configurer Netfilter
* Kerio : Entreprise spécialisée dans la protection des échanges (messgerie, pare-feu et telephone)
* check point : Founisseur mondial de solution de sécurité des systèmes d'information
* Pfsense : Pare-Feu open source basé sur le systeme d'exploitation freeBSD, disponible via license apache
* IPCop : Firewall Linux
* IPFire : Distribution de linux faisant office de pare-feu

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

### Études

#### **ACLs**
Acces Control List, liste de règles permettant de filtrer ou d'autoriser le trafic au travers d'un routeur selon certain critères comme l'IP Source, l'IP Destination, le port source/destination, protocle, ...)  
Dès qu'une règle correspond au trafic, l'action est appliquée (deny ou permit) et le reste de l'ACL n'est pas analysée. Par défaut, si aucune règle ne correspond, le paquet est rejeté.  
	 - ACL numérique standard : contient l'action, l'adresse de réseau et le wildcard mask correspondant uniquement.  
	 Cmd, en conf t : 'access-list *intDeL'accesList* *permit/deny* *adresse.ip* *wildcard mask*'  
	 - ACL numérique étendue : contient l'action à effectuer, le protocole, l'IP source, le wildcard, le port source, l'IP de destination, le wildcard, le port de destination, des options.  
	 Cmd, en conf t : 'access-list *intDeL'accesList* *permit/deny* *(provenance) adresse+wildcard /* Host *Adresse(d'un hote) / Any (n'importe)* *(destination) adresse+wildcard /* Host *Adresse(d'un hote) / Any (n'importe)*'  

 Les nombres à attribuer à une liste ne sont pas aléatoires, ils correspondent à une réglementation : 
 	 - Int <1 à 99> = ACL Standard
 	 - Int <100 à 199> = ACL Etendue
 	 - Int <1300 à 1999> = ACL IP Standard
 	 - Int <2000 à 2699> = ACL IP Etendue

 Nb : on peut également définir des ACL ayant un nom et pas un numéro, cmd : 'ip access-list standard *name*; *actionAEffectuer* *ip.destination* *wildcard*' = ACL Standard  
 cmd : 'ip access-list extended *name*; *permit/deny* *(provenance) adresse+wildcard /* Host *Adresse(d'un hote) / Any (n'importe)* *(destination) adresse+wildcard /* Host *Adresse(d'un hote) / Any (n'importe)*' = ACL Entendue

 Pour contrôler la liste des ACL on utilise la cmd 'show access-lists' en mode enable

 On doit ensuite appliquer l'ACL à une interface :  
 R1(config)#interface fastethernet 0/0  
 R1(config-if)#ip access-group *numéroOuNomdeL'ACL* in = ACL appliquée en entrée  
 OU  
 R1(config-if)#ip access-group *numéroOuNomdeL'ACL* out = ACL appliquée en sortie

####**NGFW**
NGFW = New Generation FireWall & UTM = Unified Threat Management  
Ces deux termes désignes deux types de protection réseau, le premier plus personnalisable, le second plus simplement administrable et déployable.  
NGFW ont les fonctions suivantes :  
Firewall ; SPI ; VPN (chiffrement et accélération) ; Routage ; Multicast ; Filtrage Web ; Mail Security ; Antivirus ; NAT ; VoIP "Hybrid networking" ; Proxy ; cache ; streaming.  
UTM sont des équipements, boitiers multifonctions, permettant une management des solutions de sécurités beaucoup plus simple. 

####**Firewalls (fonctionnement)**
Les différents types de filtrage :
	 - Le filtrage simple de paquet (Stateless) :  
	 Méthode la plus simple, opère sur la couche réseau et la couche transport. Consiste à accorder le passage ou a le refuser à un paquet en se basant sur : l'IP source/destination, le numéro de port source/destination, le protocole utilisé. On utilise pour cela les ACL.  
	 Limites : L'admin réseau en vient souvent à être obligé d'accepter un grand nombre d'adresses et de connexion TCP provenant d'Internet ce qui laisse du choix aux pirates. De plus, ce Firewall protège très mal des attaque DoS, Ip Spoofing / Ip Flooding ou la mutilation de paquets.
	 - Le filtrage de paquet avec état (StateFull) :  
	 Amélioration du filtrage simple, conservation des sessions et des connexions dans des tables internes du Firewall qui réagit en cas de situations protocolaires anormales. Ce Firewall permet aussi de protéger de certaines attaques DoS. Ici plus besoin d'accepter tout les paquets provenant d'Internet, on autorise l'établissement de connexion à la demande. Pour UDP et ICMP, pas de mode connecté, on autorise donc pendant un certain délai les réponses légitimes aux paquets envoyés. Les paquets ICMP sont normalement bloqués par le Firewall, qui doit en garder les traces. Cependant, il n’est pas nécessaire de bloquer les paquets ICMP de type 3 (destination inaccessible) et 4 (ralentissement de la source) qui ne sont pas utilisables par un attaquant. On peut donc choisir de les laisser passer, suite à l’échec d’une connexion Tcp ou après l’envoi d’un paquet Udp.  
	 Pour le protocole FTP (et les protocoles fonctionnant de la même façon), c’est plus délicat puisqu’il va falloir gérer l’état de deux connexions. Le protocole FTP, gère un canal de contrôle établi par le client, et un canal de données établi par le serveur. Le Firewall devra donc également laisser passer le flux de données établi par le serveur. Ce qui implique que le Firewall connaisse le protocole FTP, et tous les protocoles fonctionnant sur le même principe. Cette technique est connue sous le nom de filtrage dynamique (Stateful Inspection) et a été inventée par Checkpoint. Mais cette technique est maintenant gérée par d’autres fabricants.  
	 Limites : Il faut s'assurer que les deux techniques ont bien été implémentée. Un serveur WEB pourra être attaqué impunément car une fois la connexion effectuée, il n'y a plus aucun contrôle. De plus, les protocoles maison, ne seront pas acceptés puisqu'ils ne seront pas reconnus par le système de filtrage. 
	 - Le filtrage applicatif (ou proxy) :
	 Filtrage de couche 7 (Application), il extrait les données du protocole de couche 7 et les étudie. Chaque requête est traitée par un processus dédié (protocole http = traité par processus proxy http). De cette manière, le pare-feu rejette toutes les requetes ne correspondant pas au processus. Le PF (pare-feu) doit connaitre toutes les regles protocolaire de tous les processus qu'il doit filtrer.  
	 Limites : Problème de finesse de filtrage. Le filtrage applicatif apporte plus de sécurité que le filtrage de paquet avec état, mais cela se paie en performance. Ce qui exclut l’utilisation d’une technologie 100 % proxy pour les réseaux à gros trafic pour le moment.
	 -Quel choix réaliser ? :
	 Tout d’abord, il faut nuancer la supériorité du filtrage applicatif par rapport à la technologie Stateful. Les proxys doivent être paramétrés suffisamment finement pour limiter le champ d’action des attaquants, ce qui nécessite une très bonne connaissance des protocoles autorisés à traverser le firewall. Ensuite un proxy est plus susceptible de présenter une faille de sécurité permettant à un pirate d’en prendre le contrôle, et de lui donner un accès sans restriction à tout le système d’information.
	 Idéalement, il faut protéger le proxy par un Firewall de type Stateful Inspection. Il vaut mieux éviter d’installer les deux types de filtrage sur le même Firewall, car la compromission de l’un entraîne la compromission de l’autre. Enfin cette technique permet également de se protéger contre l’ARP spoofing.

Les différents types de firewall :
	 - Les firewall bridge
	 Relativement répandus, agissent comme des câbles réseaux avec une fonction de filtrage en plus. Leurs interfaces ne possèdent pas d'adresse IP et ne font que transférer les paquets paquets d'une interface à l'autre en appliquant les règles prédéfinie. Ils est donc innatacable, ne possédant pas d'adresse et ne répondant pas aux messages, les hackers devront faire avec ses règles sans pouvoir les annuler. Dans la plupart des cas, ces derniers ont une interface de configuration séparée. Un câble vient se brancher sur une troisième interface, série ou même Ethernet, et qui ne doit être utilisée que ponctuellement et dans un environnement sécurisé de préférence.  
	 Avantages : Impossible à éviter ; Peu coûteux  
	 Inconvenients : Contournement possible (passer outre ses regles) ; Config contraignante ; Fonctionnalités très basiques
	 - Les firewall matériels
	 Souvent sur des routeurs achetés dans le commerce à de grands constructeurs (Cisco), ils sont parfaitement intégrés dans le matériel. La config est relativement complèxe mais les intéractions avec le routeur sont simplifiée parce que sur le "même" équipement. Peu vulnérable aux attaques car sur le routeur. Très liés au matériel, l'accès à leur code est relativement difficile et souvent implémenté par le constructeur, les rendant très sûr. Config plus simple que les fw bridges, étant plus sûrs c'est souvent un argument de vente. Néanmoins totalement dépendant du constructeur du matériel pour le fix de màj / de failles de sécurité. Et les fonctionnalités ne sont pas disponibles "inter-marques", il faut bien déterminer à l'avance ses besoin et choisir en fonction.  
	 Avantages : Intégré au matériel réseau ; Administration relativement simple ; Bon niveau de sécurité  
	 Inconvénients : Dépendant du constructeur pour les mises à jour ; Souvent peu flexible
	 -  Les firewall logiciels
	 	 - Personnels : 
	 	 Souvent commerciaux avec pour but de sécuriser un ordinateur particulier et non pas un groupe d'ordinateurs. Souvent payant souvent contraignants et très peu sécurisés. Ils s'orientent plus vers la simplicité d'utilisation plutôt que vers l'exhaustivité afin de rester accessible à l'utilisateur final.  
	 	 Avantages : Sécurité en bout de chaine (poste client) ; Personnalisable facilement  
	 	 Inconvénients : Facilement contournables ; Beaucoup d'autres
	 	 - Professionnels, plus "sérieux" :
	 	 Généralement sous Linux car cet OS offre une sécurité réseau plus élevée et un contrôle plus adéquat. Le plus courant est iptables qui utilise directement le kernel Linux.  
	 	 Avantages : Personnalisable ; Niveau de sécurité très bon  
	 	 Inconvénients : Nécessite une administration supplémentaire

Les différents types d'attaques :
[Site](http://www.frameip.com/firewall/#4-8211-attaques-outils-defenses) contenant des informations diverses sur les types d'attaques et les mode de découverte des routeurs sur un réseau

####**DMZ**
Demilitarized Zone : Contient les services (souvent serveurs) devant pouvoir être accédés depuis internet (serveur WEB, serveur de messagerie). Séparés du réseau interne de l'entreprise qui lui doit rester inaccessible depuis l'extérieur.  
On configure donc le pare-feu de manière à ce qu'il ne laisse pas passer les paquets en direction du réseau internet et qu'on laisse entrer les paquets (après vérification) dans la zone démilitarisée.


####**Différents périmètres de défense**
Défense périmétrique : Constite à bloquer grâce à des pare-feux et de règles de routage les accès non autorisées au réseau et aux systemes de l'entreprise. Avec la prolifération des nouveaux équipements de connexion au réseau (ordi portable, smartphone, tablettes, ...) cette protection est désormais insuffisante.

Défense en profondeur : Listes de précaution à prendre pour arréter une intrusion après qu'elle est passé le périmètre extérieur (ce qui fini **toujours** par arriver) :
	 - Les ordinateurs inconnus (visiteurs, machines privées) sont cantonnées dans un sous-réseau virtuel aux droits d'accès restreint
	 - Les utiliseurs de systèmes sensibles ne sont pas administrateurs de leurs ordinateurs
	 - Les connexions aux serveurs de systèmes sensibles ne sont autorisées ni avec des comptes privilégiés ni à distance
	 - Utiliser des système d'aythentification robuste, comme Kerberos

Ces mesures ont un coût organisationnel et technique non négligeable et ne doivent de fait être appliquées qu'au systèmes le nécessitant. Comme partout, le mieux et l'ennemie du bien, des règles trop contraignantes pour les utilisateurs seraient contournées et nuiraient au SI.


### Réalisations

####**Définir les critères du firewall**

####**Comparer les pare-feux**

####**Configurer le pare-feu (packet-tracer)**