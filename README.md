# Firewall

## Mots clés :

* DMZ : zone démilitarisée: 
* Pare-feu :
* NGFW :
* SI :
* DSI :
* Carte heuristique :
* Firewalling :
* Access list :
* Système de gestion unifiée :
* Sécurisation périmétrique :
* Sécurité en profondeur :
* ASA :
* UTM :
* Fortinet :
* Net filter :
* Appliance :
* IP table :
* Kerio :
* check point :
* Pfsense :
* IPCop :
* IPFire :

## Contexte :

** Quoi ?**

* Mettre en place un firewall
* Protéger le serveur de la DMZ
* Modifier l'architecture
* Gérer les accès

** Pourquoi ?**

* Sécuriser l'accès au SI
* Empêcher les attaques sur son site

** Comment ?**

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

####** ACLs**

####** NGFW**

####** Firewalls (fonctionnement)**

Sécurise le réseau local, détecte les tentatives d'intrusion et y pare au mieux possible. Permet aussi de restreindre l'accès interne vers l'extérieur (comme bloquer les jeux en ligne). 
Il existe différents types de filtrages:

1. filtrage simple de paquet (Stateless)

méthode de filtrage la plus simple, elle opère au niveau de la couche réseau et transport.  Il accorde ou refuse le passage de paquets selon:

* l'ip source/destination
* le numéro de port source/destination
* le protocole de niveau 3 ou 4

Nécessite de configurer le Firewall ou le routeur avec des ACLs, le problème est donc que l'admin doit accorder beaucoup d'accès; Ex: pour autorisé les co depuis un réseau privé l'admin devra accepter toutes les connexions TPC provenant d'internet avec un port supérieur à 1024



####** DMZ**

Zone démilitarisée, zone de service. 
Il peut s'agir de services applicatifs (serveur web, serveur de messagerie) ou de services de sécurité (serveurs  mandataires - proxy - ou reverse proxy). Cette zone tient son nom de sa position classique dans l'architecture d'une passerelle

Sous réseau séparé du réseau local et isolé de celui-ci et d'internet par un firewall. Il contient les machines susceptibles d'être accédées depuis internet. LE firewall bloquera l'accès au réseau local et les services pouvant être accédés depuis internet seront dans la DMZ

Si le pare feu est compromis plus rien n'est sécurisé on peut donc en mettre en cascade afin d'éviter ce risque

![](picsNico/DMZ.png)

####** Différents périmètres de défense**



### Réalisations

####** Définir les critères du firewall**

####** Comparer les pare-feux**

####** Configurer le pare-feu (packet-tracer)**
