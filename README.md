# Firewall

## Mots clés :

* DMZ : zone démilitarisée
* Pare-feu : logiciel/matériel qui permet de faire respecter la politique de sécurité du réseau
* NGFW : next generation firewall, fw + système d'intrusion
* SI : système d'information, ensemble organisé de ressources
* DSI : directeur des systèmes d'information, responsable du matériel, des saves et des logiciels du SI
* Carte heuristique : carte mentale (XMind), représente le lien entre les idées
* Firewalling : utiliser un firewall
* Access list : liste représentant différentes infos permettant d'autoriser/interdire le passage à un firewall. Liste car il prends les règles séquentiellement ce qui ralentit le réseau.
* Système de gestion unifiée : UTM 
* Sécurisation périmétrique : sécu à l'aide de fw et règles de routage, insufisante de nos jours. Ne concerne que les équipements périmétrique
* Sécurité en profondeur : ensemble de règles pour renforcer la sécurité au delà de la sécu périmétrique, on suppose qu'il y a déjà un intru. Concerne les équipements périmétriques et à l'intérieur du réseau
* ASA : pare feu propriétaire cisco, Adaptative security appliance
* UTM : fw posédant des fonctionalitées supplémentaires
* Fortinet : entreprise américaine qui fournit des équipements réseau
* Net filter : module linux pr filtrer les paquets
* Appliance : package matériel et logiciel (commercial)
* IP table : interface pour config netfilter sur linux
* Kerio : entreprise spé dans la sécurisation des échanges
* check point : fournisseur mondial de solutionsde sécu informatique. Pionnier des pare-feu et à créé la statefull
* Pfsense : firewall applicatif opensource
* IPCop : distrib linux fournissant un fw simple à gérer
* IPFire : distrib linx fw
* proxy: permet de rediriger le trafic

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

####**ACLs**

**fantou**

Access control ist: autoriser/bloquer selon des règles

dès qu'une règle dans la liste corresponds on applique l'action (permit/denny) et arrète le parcourt

2 types :

standard:
action
addresse réseau
wildcard

étuendues
ation
protocole
ip destination
port destination
ip source
port source

commandes :

standard 

  R1(config)#access-list [n°] permit|deny [adress] [wildcard]
  
étendues :



1-99 standard
1300-1999 ip standard

100-199 étendues
2000-2699 étendues

*commande pour ajouter des règles*

  show access-list
  
on peut en mettre 2 par interface au max, pour le flu entrant et le flux sortant

les ACL sont particulièrement efficace si elsp ort sont statiques mais on perds cet avantage si ces protocoles accordent des port dynamiques


####**NGFW**

**Gilly**

nextgen firewall: proposent pls de services, sont plus adaptées pour les entrprises de grandes taille. Ils détectent les intrusions

on à:
__________________________________________________________________________________________________________________________
Firewall | Intrusion Prevention System | Intrusion Prevention System | contrôle application et DPI (depacket incryption) | filtrage url | VPN | QoS | antivirus | Mail/ anti-spam |

on peut acheter suelement certaines briques

on dit next gen car on est dans la 5eme gen de fw et le fw doit être proche du client, les anciennes génération étaient juste un mur à l'entrée (émilien)


####**Firewalls (fonctionnement)**

**gilly**
**max**
dispositif permettant de détecter les flux entrants ou sortant


stateless:
 ip source et destination
 port source et destination
 protocole 4
 
utilise les ACL
facile à contourner
vite impossible à gérer pour l'admin, il doit écrire TOUTES les règles mais les ports sont attribués au pif donc on doit autoriser tous les ports
 
statefull : 
mémorise les session dans une table et on autorie uniquement celles initées depuis notre machine mais ça peut posser des prolèmes pour des protocoles (ex: FTP)
empêche certains DoS car il va voir que la même session spam


applicatif:
désencapsule les paquets pour les traiter

types de FW: **émilien**

bridge : sans ip transfère les paquets avec des règles spécifiques
  * indétectables
  * inévitable

matériel : fait office de boite noire
  * dépendance du constructeur
  
logiciel:
  * personnel :
      * peu sécurisé
      * dificile à départager de par leur nombre
      
  * 'sérieux' généralement sur linux:
      * personnalisables
      * très sécu
      * nécessite des connaissances
      
pb des fw logiciel: on peut les contourner en passant par les couches inférieures 


####**DMZ**

zone démilitarisée

séparation d'un réseau ds serveur et du réseau local séparé par un fw

*schéma d'une DMZ* 
trafic: internet vers dmz, client vers dmz, client vers internet, dmz vers internet. Le dmz peut répondre au client avec un fw statefull


Hors-sujet : orga en entreprise

OPération <- dev, système | maintenance <- appli | prod <- dev, supervision

maintenance + prod ont un CTO, OP : responsable des opérations COO

####**Différents périmètres de défense**

**gilly*

largeur: cf les def

profondeur : cf les def

### Réalisations

####**Définir les critères du firewall**

####**Comparer les pare-feux**

####**Configurer le pare-feu (packet-tracer)**

skraaah
