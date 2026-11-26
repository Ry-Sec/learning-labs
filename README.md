# Learning Labs : Réseaux & Cybersécurité (SOC L1)

## 1. Objectif de ce dépôt
Afin d'illustrer ma démarche d'apprentissage concret vers le métier d'Analyste SOC Niveau 1, et pour maintenir une pratique régulière visible, vous trouverez ici mes dépôts de travaux pratiques détaillant mes laboratoires en réseau et en cybersécurité, à partir des formations suivantes :
* **Cisco Junior Cybersecurity Analyst** :
    * Networking Basics (*certificat obtenue le 25/11/2026*)
    * **Networking devices and Initial Configuration** (actuellement ici)
    * Introduction to Cybersecurity 
    * Endpoint Security
    * Network Defense
    * Cyber Threat Management

A venir :
* *LetsDefend*
* *TryHackMe*

---

## 2. Outils et Environnements utilisés
* **Analyse réseau :** Wireshark (avec Npcap)
* **Simulation réseau :** Cisco Packet Tracer
* **Documentation & Versioning :** Visual Studio Code, Markdown, Git & GitHub
* **Virtualisation & Infrastructure :** Proxmov VE, Win11 Enterprise
    * Infrastructure Virtualisée 
        * Hyperviseur T1 : Proxmox VE 9.2.2
        * CPU : I7-7700K (4 coeurs / 8 threads avec Hyper-Threading @4,2 Ghz)
        * RAM : 16 Go DDR4 CL16 (3200 Mhz)
        * Stockage : 1 To SSD NVMe
    * Machines Virtuelles Principales (VM 100 et VM 101)
        * OS : Windows 11 Enterprise (64-bit)
        * vCPU : 4 vCPU 
        * RAM : 4 Go alloués
        * Stockage : 32 Go 
---

## 3. Index des Labs réalisés

### Réseaux 
* **[Lab01](./01-network-labs/01-basic-router-ip-config/)** : Configuration fondamentale de routeur et familiarisation avec le logiciel Cisco Packet Tracer.
*  **[Lab02](./01-network-labs/02-DHCP-staticIP-config-packet-inspection/)** : Configuration DHCP et IPv4 statique ainsi qu'inspection de paquets.
* **[Lab03](./01-network-labs/03-packet-analysis-NAT-local-distant/)** : Inspection et analyse de paquets dans un réseau local et distant.
* **[Lab04](./01-network-labs/04-packet-analysis-LAN-Web/)** : Analyse de paquets en réseau LAN et réseau Internet.
* **[Lab05](./01-network-labs/05-ARP-Frames-analysis-Wireshark-Win11-ProxMoxVE/)** : Analyse de trames ARP avec Wireshark, Win11 Entreprise et ProxMoxVE.

### Cybersécurité 
* *(À venir)*


