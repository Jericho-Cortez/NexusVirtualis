***
# JOB 02 – INSTALLATION VMWARE WORKSTATION PRO

**Contexte** : Depuis le rachat de VMware par Broadcom en 2024, VMware Workstation Pro est devenu **gratuit pour un usage personnel**.

***

##  Objectif technique

Installer VMware Workstation Pro sur Windows 11 et activer la virtualisation imbriquée (Nested Virtualization) pour héberger des hyperviseurs Type 1.

***

##  Justification des choix

- **VMware Workstation Pro** : Hyperviseur Type 2 stable, supportant nativement la virtualisation imbriquée (Nested Virtualization)
- **Gratuit depuis 2024** pour usage personnel → Plus besoin de licence
- **Nécessaire** pour faire tourner Hyper-V, ESXi, Proxmox VE et XCP-ng dans des VMs

***

##  Prérequis matériels

Avant toute installation, vérifier que ton PC supporte la virtualisation :


| Composant | Minimum requis      | Ton PC     |
| :-------- | :------------------ | :--------- |
| CPU       | Intel VT-x ou AMD-V | 4 cœurs    |
| RAM       | 8 Go                | 8 Go       |
| Stockage  | 100 Go SSD          | 100 Go     |
| OS        | Windows 10/11       | Windows 11 |


***

##  ÉTAPE 1 – Activer la virtualisation dans le BIOS

###  Objectif

Permettre au CPU d'exécuter des machines virtuelles.

### Actions précises

1. **Redémarrer le PC**
2. **Entrer dans le BIOS** :
    - Appuyer sur `F2`, `Suppr`, `F10` ou `Échap` au démarrage (selon fabricant)
3. **Chercher l'option de virtualisation** :
    - Intel : `Intel VT-x` ou `Intel Virtualization Technology`
    - AMD : `AMD-V` ou `SVM Mode`
4. **Activer l'option**
5. **Sauvegarder et redémarrer** (généralement `F10` puis `Yes`)

###  Risques et erreurs fréquentes

- **Erreur** : Option grisée → Windows utilise Hyper-V
    - **Solution** : Désactiver Hyper-V dans Windows d'abord
- **Erreur** : BIOS en chinois/autre langue
    - **Solution** : Chercher des termes comme "Virtualization", "VT-x", "SVM"


###  Screenshot obligatoire

- Capture d'écran du BIOS montrant **"Intel VT-x : Enabled"** ou **"AMD-V : Enabled"**
![[Pasted image 20260216104739.png]]
***

##  ÉTAPE 2 – Télécharger VMware Workstation Pro

###  Objectif

Récupérer l'installeur officiel gratuit.

### Actions précises

1. Aller sur [broadcom.com](https://support.broadcom.com)
2. Créer un compte gratuit (obligatoire depuis le rachat)
3. Chercher **"VMware Workstation Pro"**
4. Télécharger la version **17.6.1** ou plus récente pour Windows
5. Fichier téléchargé : `VMware-workstation-full-17.x.x-xxxxx.exe` (~700 Mo)

***

##  ÉTAPE 3 – Installer VMware Workstation Pro

### Actions précises

1. **Double-cliquer sur le fichier `.exe`**
2. Accepter les conditions (UAC Windows)
3. **Installation Standard** :
    - Suivant → Suivant
    - **Décocher** "Enhanced Keyboard Driver" (évite conflits)
    - **Cocher** "Add VMware to System PATH"
4. Cliquer sur **Install**
5. Redémarrer si demandé

###  Risques et erreurs fréquentes

- **Erreur** : "Hyper-V detected"
    - **Solution** : Désactiver Hyper-V dans Windows Features
    - Commande PowerShell (admin) :

```powershell
bcdedit /set hypervisorlaunchtype off
```

    - Redémarrer

***

## ÉTAPE 4 – Activer la licence gratuite

### Actions précises

1. Lancer **VMware Workstation Pro**
2. Au démarrage, choisir **"Use for personal use"** (gratuit)
3. Aucune clé nécessaire depuis 2024

### Screenshot obligatoire

- Interface principale de VMware Workstation Pro vide (aucune VM)

***

##  ÉTAPE 5 – Configurer le réseau NAT

###  Objectif

Créer un réseau isolé pour tes hyperviseurs imbriqués.

### Actions précises

1. Aller dans **Edit → Virtual Network Editor**
2. Cliquer sur **"Change Settings"** (droits admin)
3. Vérifier que **VMnet8 (NAT)** existe :
    - Subnet IP : `192.168.111.0/24` (par défaut)
    - DHCP activé
4. Cliquer sur **OK**

###  Concepts clés à retenir

- **NAT** : Les VMs partagent l'IP de ton PC pour accéder à Internet
- **Bridged** : Les VMs obtiennent une IP du routeur (comme un PC physique)
- Pour ce projet → **NAT suffit** (sauf pour le cluster final)


![[Pasted image 20260216113605.png]]
***

##  Points à expliquer à l'oral (soutenance)

1. **Pourquoi VMware Workstation Pro ?**
    - Supporte nativement la virtualisation imbriquée
    - Stable et professionnel
    - Gratuit depuis rachat Broadcom
2. **Différence Type 1 vs Type 2** :
    - **Type 2** (VMware Workstation) : s'installe sur un OS (Windows, Linux)
    - **Type 1** (ESXi, Hyper-V, Proxmox) : s'installe directement sur le hardware (Bare Metal)
3. **Virtualisation imbriquée** :
    - Permet de faire tourner un hyperviseur Type 1 dans une VM VMware
    - Nécessite activation VT-x dans BIOS + paramètre VMware

***

##  Commandes utiles

### Vérifier si la virtualisation est active (Windows PowerShell)

```powershell
Get-ComputerInfo | Select-Object HyperVisorPresent, HyperVRequirementVirtualizationFirmwareEnabled
```


### Désactiver Hyper-V (si conflit)

```powershell
bcdedit /set hypervisorlaunchtype off
```


### Réactiver Hyper-V

```powershell
bcdedit /set hypervisorlaunchtype auto
```


***

##  Organisation des fichiers

Crée cette arborescence pour ton projet :

```
C:\Nexus-Virtualis\
├── ISO\
│   ├── Windows_Server_2022.iso
│   ├── VMware-ESXi-8.0.iso
│   ├── proxmox-ve_8.x.iso
│   ├── XCP-ng_8.x.iso
│   └── debian-12.x-netinst.iso
├── VMs\
│   └── (VMs créées par VMware)
└── Screenshots\
    └── Job02-VMware-Installation\
```


***

##  Checklist finale Job 02

- [ ] Virtualisation activée dans BIOS
- [ ] VMware Workstation Pro installé
- [ ] Licence gratuite activée
- [ ] Virtual Network Editor configuré (NAT)
- [ ] Test virtualisation imbriquée OK
- [ ] Tous les screenshots pris et organisés
