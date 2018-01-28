
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
