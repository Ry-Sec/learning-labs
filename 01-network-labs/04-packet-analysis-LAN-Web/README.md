# Lab 04 : Observation et analyse de trafic dans un LAN non routé puis routé
## 1. Informations générales
* **Date** : 25/11/2026
* **Outil** : Cisco Packet Tracer (.pka du module 14 de "Notions de base sur les réseaux")
* **Objectifs** : 
    * Observer le flux de trafic dans un LAN non routé.
    * Reconfigurer le réseau pour router entre les LANs.
    * Observer le flux de trafic dans le réseau routé.

## 2. Étapes 
### Etape 1 : Observer et analyser le flux de trafic dans un LAN non routé
* Topologie du réseau LAN :
![alt text](images/topologie-reseau-LAN.png)  
* Vérifier qu'il n'y a pas d'adresse MAC dans le cache ARP de Sales 1 avec la commande arp -a (arp -d pour les supprimer si il y en a)
* Ping Sales 1 (192.168.1.7) à partir de Sales 2 (192.168.1.5) en mode simulation.  

![alt text](images/PDU-packet-sales-switch.png)
| At Device  | Src MAC        | Dest. MAC      | Src IPv4    | Dest. IPv4  |
| ---------- | -------------- | -------------- | ----------- | ----------- |
| Sales (in) | 00E0.8FA9.B373 | FFFF.FFFF.FFFF | 192.168.1.5 | 192.168.1.7 |
* On voit bien la requête ARP de Sales 2 avec l'adresse de destination MAC FFFF.FFFF.FFFF (adresse de diffusion) depuis l'adresse MAC de Sales 2 afin de connaître l'adresse MAC associé à l'adresse IPv4 (Sales 2) qu'on a ping (absent de la table ARP).
* Actuellement l'ensemble du réseau (Accounting, Finance et Sales) reçoit la requête ARP, ce qui réduit son efficacité dû au nombres de requête ARP diffusées (150 appareils connectés en LAN) et ce qui réduit sa sécurité.
* Une fois la réponse ARP reçu à Sales 2, le ping est envoyé à Sales 1.

## Etape 2 : Reconfigurer le réseau pour acheminer entre les LAN

* Connecter les switchs Finance et Sales au routeur Edge.
* Entrez la commande ipconfig /renew sur chacun des périphériques de Finance et Sale afin d'accélérer le processus d'obtention de nouvelle adresse IP à partir du protocole DHCP.
* Le réseau Finance est attribué le réseau IPv4 192.168.168.2.0/24 et le réseau Sales est attribué le réseau IPv4 192.168.3.0/24
![alt text](images/topologie-reseau-routeur.png)
## Etape 3 : Observer et analyser le flux de trafic dans le réseau avec un routeur
* Vérifier qu'il n'y a pas d'adresse MAC dans le cache ARP de Sales 1 avec la commande arp -a (arp -d pour les supprimer si il y en a)
* Ping Sales 1 à partir de Sales 2 en mode simulation.  
![alt text](images/routeur-drop-diffusion.png)  
* On voit bien que la requête ARP (diffusion) est bien stoppé à partir du routeur. Seul les périphériques de Sales reçoivent la diffusion.
* Ping Finance 1 à partir de Sales 2 en mode simulation.
* Sales 1 calcul qu'il n'est pas sur le même réseau que l'adresse IPv4 de destination, ainsi il doit passer par sa passerelle par défaut.
* Sales 1 envoi une requête ARP pour connaître l'adresse MAC associé à l'adresse IPv4 de la passerelle par défaut puisque sa table ARP est vide, en envoyant une trame dont l'adresse de destination IPv4 est sa passerelle par défaut et l'adresse de destination MAC est une adresse de diffusion.
* Une fois que le routeur reçoit cette trame, il renvoi à Sales 2 l'adresse MAC du port qui à reçu la trame.
* Sales 1 associe l'adresse IPv4 de la passerelle par défaut avec l'adresse MAC du port depuis Sales du routeur.
* Ensuite, Sales 2 envoi une trame ICMP à l'adresse de destination IPv4 de Finance 1 et avec l'adresse de destination MAC de la passerelle par défaut.
* Quand le routeur reçoit cette trame, il regarde dans sa table de routage pour identifier l'interface de sortie (port) qui correspond au réseau de destination (192.16.2.0)
* Le routeur reconstruit une nouvelle trame Ethernet et regarde dans sa propre table ARP s'il connaît l'adresse MAC du PC de destination.
* Ici il ne la connaît pas, et envoie une requête ARP au réseau de diffusion Finance, qui lui renvoi l'adresse MAC associé à l'adresse IPv4 de Finance 1.
* Le 1er ping time out.  
![alt text](images/1er-ping-timeout-finance.png)
* Sales 2 renvoie un ping et cette fois-ci, le routeur connaît l'adresse MAC de Finance 1 et lui envoi directement le ping, qui va renvoyer un ping à Sales 2. Ce dernier le reçoit bien.
![alt text](images/2nd-ping-reception-sales2.png)

### Envoyer un ping à un serveur Web sur Internet
* Ping le serveur Web 209.165.200.112, à partir de Sales 2 192.168.3.3
* 1er ping : le routeur demande en diffusion avec un paquet ARP : "Quelle est l'adresse MAC de l'adresse de destination IPv4 209.165.200.112 ?"
L'ISP lui répond : "cette adresse est associé à l'adresse MAC 00D0.BAE0.A476". Puisque l'ISP le possède dans sa table de routage, il renvoie l'adresse MAC de l'interface par laquelle le message est arrivé.  
![alt text](images/PDU-1er-ping-web.png)
* Le 1er ping time out.
* 2nd ping : L'ISP demande en diffusion avec un paquet ARP : "Quelle est l'adresse MAC de l'adresse de destination IPv4 209.165.200.112 ?" 
Le serveur web lui répond : "cette adresse est associé à l'adresse MAC 0006.2AED.806E".  
![alt text](images/PDU-2nd-ping-web.png)
* Le 2nd ping time out.
* 3ème ping : le ping est bien reçu par le serveur Web mais l'ISP demande en diffusion avec un paquet ARP : "Quelle est l'adresse MAC de l'adresse de destination IPv4 192.168.3.3 ?" Puisqu'il ne la pas dans sa table ARP. Il a le routeur mais pas Sales 2.  
![alt text](images/PDU-3rd-ping-web.png)
* Le 3ème ping time out.
* 4ème ping : le ping est bien reçu par le serveur Web puis renvoyé et reçu par Sales 2 étant donné que les adresses de destination MAC et IPv4 sont connues par les routeurs dans les deux sens.
![alt text](images/PDU-4th-ping-web-cmd.png)

## 3. Bilan
L'exercice à permis une bien meilleur compréhension des communications entre périphériques hôtes et intermédiaires, notamment avec le protocole ARP, la table de routage et la table ARP. Il y avait une confusion importante lors du 3ème ping qui à time out, nécessitant de bien observer pour comprendre que l'ISP ne connaissait pas l'adresse MAC de l'adresse de destination de Sales 2. Je pensai que comme il connaissait l'adresse MAC du routeur EDGE il pouvait l'envoyer. Ce qui m'a aussi rendu perturbé, c'est que l'adresse IPv4 de destination était une adresse privée (192.168.0.0), au lieu de publique, alors que c'est sur internet.