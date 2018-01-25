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
* Appliance : 
* IP table :
* Kerio :
* check point :
* Pfsense :
* IPCop :
* IPFire :

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

####**NGFW**

####**Firewalls (fonctionnement)**

####**DMZ**

####**Différents périmètres de défense**



### Réalisations

####**Définir les critères du firewall**

####**Comparer les pare-feux**

####**Configurer le pare-feu (packet-tracer)**