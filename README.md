
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


## 2  - Les différents types de firewall

**Firewall Bridge**






