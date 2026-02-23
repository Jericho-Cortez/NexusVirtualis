## 1. Objectif du Job 06

- Installer Proxmox VE dans une VM sous VMware Workstation (nested).[^1]
- Créer une VM Debian sans interface graphique (2 vCPU, 1 Go RAM, 8 Go disque).[^1]
- Produire une documentation d’installation de l’hyperviseur + de la VM.[^1]

***

## 2. Dimensionnement conseillé (8 Go RAM)

Pour ton PC (4 vCPU / 8 Go RAM) :[^2]

- VM Proxmox VE :
    - vCPU : 2
    - RAM : 3 Go (4 Go serait confortable, mais 3 Go tient mieux sur 8 Go physiques).
    - Disque : 40 Go.[^2]
- VM Debian dans Proxmox :
    - vCPU : 2
    - RAM : 1 Go
    - Disque : 8 Go.[^1]

Logique : il faut garder de la RAM pour Windows 11 + VMware Workstation, sinon tout swappe.[^2]

***

## 3. Étapes d’installation Proxmox VE (dans VMware)

### 3.1. Création de la VM Proxmox VE

- Type : Custom / Typical VM Linux 64 bits.
- CPU : 2 vCPU.
- RAM : 3 Go.
- Disque : 40 Go en disque unique, format recommandé par VMware (SCSI).[^2]
- Réseau : 1 carte en NAT (pour que Proxmox ait Internet via ton PC).[^2]
- Monter l’ISO Proxmox VE dans le lecteur CD virtuel.[^2]

![[Pasted image 20260223152016.png]]
### 3.2. Installation de Proxmox VE

Dans l’installateur :

- Accepter la licence.
- Choisir le disque de 40 Go comme cible.
- Mot de passe root fort + mail (peu importe, mais utile pour la forme).
- Hostname type : `pve01.local.lab`.
- IP :
    - Soit DHCP (plus simple),
    - Soit IP statique sur le réseau NAT type `192.168.x.y`, gateway = routeur NAT VMware.[^2]
![[Pasted image 20260223152444.png]]
![[Pasted image 20260223152508.png]]
![[Pasted image 20260223152604.png]]
![[Pasted image 20260223152740.png]]

***

## 4. Paramétrage réseau recommandé

Pour ce Job uniquement (pas le cluster) :

- Mode VMware : NAT sur la VM Proxmox.[^2]
- IP Proxmox :
    - DHCP si tu veux aller vite,
    - Statique si tu veux maîtriser (exemple : `192.168.200.10/24`, gateway `192.168.200.2`).[^2]

Concept clé : Proxmox est un hyperviseur type 1, mais ici il tourne dans une VM (virtualisation imbriquée). Il fournit une interface web d’admin sur le port 8006 en HTTPS.[^1][^2]
![[Pasted image 20260223152803.png]]
![[Pasted image 20260223153518.png]]
***

## 5. Création de la VM Debian dans Proxmox

Depuis l’interface web Proxmox (https://IP_DE_PVE:8006) :[^1]

- Upload de l’ISO Debian netinst dans le stockage ISO de Proxmox.
- Créer une VM :
    - 2 vCPU, 1 Go RAM, 8 Go disque.[^1]
    - OS : Linux, Debian 64 bits.
    - Interface réseau : VirtIO ou e1000, bridge par défaut `vmbr0`.[^2]
- Démarrer la VM et installer Debian en mode serveur (sans interface graphique).[^1]
![[Pasted image 20260223153647.png]]
![[Pasted image 20260223155135.png]]
![[Pasted image 20260223153938.png]]
![[Pasted image 20260223155215.png]]
![[Pasted image 20260223155238.png]]
![[Pasted image 20260223155317.png]]
![[Pasted image 20260223161850.png]]