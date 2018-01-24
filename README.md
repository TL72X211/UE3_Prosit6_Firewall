
# Firewall

## Mots clés :

* DMZ :
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

test
