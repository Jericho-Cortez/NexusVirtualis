
#  JOB 03 – RÉCUPÉRATION DES ISO HYPERVISEURS

Absolument ! Je vais te produire un guide ultra-détaillé pour le Job 03.

***

##  1. OBJECTIF TECHNIQUE

**Mission** : Télécharger et organiser les images ISO des 4 hyperviseurs Type 1 cibles :

- **VMware ESXi 8.0 U3e** (version gratuite)
- **Windows Server 2022** (avec rôle Hyper-V)
- **Proxmox VE 8.3** (dernière stable)
- **XCP-ng 8.3 LTS** (Long Term Support)

**Pourquoi c'est crucial** : Ces fichiers ISO sont les fondations de ton projet. Sans eux, impossible de déployer les hyperviseurs dans VMware Workstation Pro.

***

##  2. JUSTIFICATION DES CHOIX

### Pourquoi ces versions spécifiques ?

| Hyperviseur        | Version    | Justification                                                       |
| :----------------- | :--------- | :------------------------------------------------------------------ |
| **ESXi**           | 8.0 U3e    | Première version gratuite depuis le rachat Broadcom (2025) [^2][^3] |
| **Windows Server** | 2022       | Version évaluation 180 jours gratuite [^4][^5]                      |
| **Proxmox VE**     | 8.x stable | Open source, interface web moderne [^6]                             |
| **XCP-ng**         | 8.3 LTS    | Support long terme, basé sur XenServer [^7][^8]                     |

### Impact sur tes 8 Go de RAM

Chaque ISO pèse entre **600 Mo et 5 Go**. Aucun impact RAM pour le téléchargement, mais organise-les proprement pour gagner du temps sur les 7 jours.

***

##  3. ÉTAPES PRÉCISES DE TÉLÉCHARGEMENT

###  Étape 1 : Créer la structure de dossiers

```
C:\ISO_Nexus\
├── ESXi\
├── Windows_Server_2022\
├── Proxmox_VE\
├── XCP-ng\
└── Debian\
```

**Commande PowerShell** :

```powershell
New-Item -Path "C:\ISO_Nexus\ESXi","C:\ISO_Nexus\Windows_Server_2022","C:\ISO_Nexus\Proxmox_VE","C:\ISO_Nexus\XCP-ng","C:\ISO_Nexus\Debian" -ItemType Directory
```


***

###  Étape 2 : Télécharger VMware ESXi 8.0 U3e

**Pré-requis critiques** :

- **Compte Broadcom gratuit** obligatoire
- **Nested virtualization** activée dans le BIOS (Intel VT-x / AMD-V)


#### Procédure détaillée :

1. **Créer un compte Broadcom** :
    - Va sur : `https://support.broadcom.com`
    - Clique sur **"Register"**
    - Remplis avec une adresse email valide
2. **Accéder aux téléchargements** :
    - Connecte-toi
    - Va dans **"My Downloads"**
    - Clique sur **"Free Software Downloads available HERE"**
3. **Rechercher ESXi** :
    - Tape `VMware vSphere Hypervisor` dans la barre de recherche
    - Sélectionne **8.0U3e**
4. **Télécharger** :
    - Accepte les conditions générales
    - Clique sur l'icône **nuage**
    - Fichier : `VMware-VMvisor-Installer-8.0U3e-*.iso` (~400 Mo)
    - Enregistre dans `C:\ISO_Nexus\ESXi\`

**Note importante** : La clé de licence gratuite est **intégrée dans l'ISO**.
![[Pasted image 20260216152507.png]]
***

###  Étape 3 : Télécharger Windows Server 2022

#### Procédure :

1. **Accéder au site Microsoft** :
    - URL : `https://www.microsoft.com/fr-fr/evalcenter/evaluate-windows-server-2022`
2. **Sélectionner l'édition** :
    - Langue : **Français**
    - Format : **ISO 64 bits**
    - Édition : **Standard Evaluation**
3. **Téléchargement** :
    - Fichier : `SERVER_EVAL_x64FRE_fr-fr.iso` (4,71 Go)
    - Destination : `C:\ISO_Nexus\Windows_Server_2022\`

**Licence** : 180 jours d'évaluation gratuite. Extensible 5 fois avec `slmgr /rearm`.
![[Pasted image 20260216161957.png]]
***

###  Étape 4 : Télécharger Proxmox VE 8.x

#### Procédure :

1. **Site officiel** :
    - URL : `https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso`
2. **Télécharger** :
    - Clique sur la dernière version stable (**8.x**)
    - Fichier : `proxmox-ve_*.iso` (~1 Go)
    - Destination : `C:\ISO_Nexus\Proxmox_VE\`

**Important** : Après installation, **toujours faire** `apt update && apt dist-upgrade`.
![[Pasted image 20260216161938.png]]
***

###  Étape 5 : Télécharger XCP-ng 8.3 LTS

#### Procédure :

1. **Téléchargement direct** :
    - URL : `https://mirrors.xcp-ng.org/isos/8.3/xcp-ng-8.3.0-20250606.iso?https=1`
2. **Alternative GitHub** :
    - `https://github.com/xcp-ng/xcp/releases`
    - Télécharge le **Full version** (~661 Mo)
3. **Vérification SHA256** (optionnelle mais recommandée) :

```
4d6f5a99da0d70920bc313470ad2b14decab66038f0863ca68a2b81126ee2977
```


4. **Destination** : `C:\ISO_Nexus\XCP-ng\`

**Astuce** : XCP-ng propose aussi une version **Netinstall** (172 Mo) pour connexions lentes.

***

###  Étape 6 : Télécharger Debian 12 (Bookworm)

**Pourquoi Debian ?** : C'est la VM test qui tournera sur chaque hyperviseur.

#### Procédure :

1. **Site officiel** :
    - URL : `https://www.debian.org/distrib/netinst`
2. **Télécharger** :
    - Version : **Debian 12 netinst** (amd64)
    - Fichier : `debian-12.x.x-amd64-netinst.iso` (~400 Mo)
    - Destination : `C:\ISO_Nexus\Debian\`

***

##  4. PARAMÉTRAGE RECOMMANDÉ

### Organisation finale des fichiers :

```
C:\ISO_Nexus\
├── ESXi\
│   └── VMware-VMvisor-Installer-8.0U3e.iso (400 Mo)
├── Windows_Server_2022\
│   └── SERVER_EVAL_x64FRE_fr-fr.iso (4,71 Go)
├── Proxmox_VE\
│   └── proxmox-ve_8.x.iso (1 Go)
├── XCP-ng\
│   └── xcp-ng-8.3.0-20250606.iso (661 Mo)
└── Debian\
    └── debian-12.x.x-amd64-netinst.iso (400 Mo)
```

**Total espace disque** : ~7 Go sur ton SSD 100 Go.

***

##  5. CONFIGURATION RÉSEAU (À CE STADE)

**Aucune configuration réseau n'est requise** pour le Job 03. C'est uniquement du téléchargement.

**Pour plus tard** (Jobs 4-8) :

- Mode **NAT** dans VMware Workstation pour Internet
- Mode **Bridged** avec IP statique pour le cluster (Job final)

***

##  6. EXPLICATION DES CONCEPTS CLÉS

###  Qu'est-ce qu'une image ISO ?

Une **image ISO** est une copie exacte d'un CD/DVD sous forme de fichier unique. Elle contient :

- Le bootloader (GRUB, isolinux)
- Le système d'exploitation
- Les pilotes matériels
- L'installateur

**Pourquoi en virtualisation ?** : VMware Workstation peut "monter" un ISO comme si c'était un CD physique dans un lecteur.

###  Type 1 vs Type 2

| Caractéristique | Type 1 (Bare Metal) | Type 2 (Hosted) |
| :-- | :-- | :-- |
| **Installation** | Directement sur le matériel | Sur un OS existant (Windows, Linux) |
| **Performance** | Accès direct au matériel | Passe par l'OS hôte |
| **Exemples** | ESXi, Hyper-V, Proxmox, XCP-ng | VMware Workstation, VirtualBox |
| **Usage** | Production entreprise | Labs, développement |

**Dans ton cas** : Tu fais du **nested virtualization** = Type 1 dans Type 2.

###  Nested Virtualization (Virtualisation imbriquée)

**Schéma conceptuel** :

```
PC Physique (Windows 11)
└── VMware Workstation Pro (Type 2)
    └── VM ESXi (Type 1)
        └── VM Debian
```

**Pré-requis matériel** :

- CPU avec **Intel VT-x** ou **AMD-V** activé dans BIOS
- Option **"Expose hardware virtualization"** cochée dans VMware

***

##  7. SCREENSHOTS À PRENDRE (POUR LA SOUTENANCE)

###  Liste exacte des captures obligatoires :

1. **Dossier ISO organisé** :
    - Explorateur Windows montrant `C:\ISO_Nexus\` avec les 5 sous-dossiers
    - Vue détaillée (nom + taille + date) de chaque ISO
2. **Compte Broadcom créé** :
    - Page "My Downloads" avec ton nom/email visible (floute email si besoin)
3. **Page de téléchargement ESXi** :
    - Section "VMware vSphere Hypervisor 8.0U3e" avec le bouton download
4. **Page Microsoft Evaluation Center** :
    - Sélection "Windows Server 2022 ISO 64 bits Français"
5. **Page Proxmox downloads** :
    - Section downloads avec version 8.x sélectionnée
6. **Page XCP-ng** :
    - GitHub releases ou site officiel avec version 8.3 LTS
7. **Propriétés des fichiers téléchargés** :
    - Clic droit > Propriétés sur chaque ISO pour montrer la taille réelle

***

## 🎤 8. POINTS À EXPLIQUER À L'ORAL (JURY)

### Partie théorique (2-3 min)

**Question probable** : *"Pourquoi avez-vous choisi ces versions d'hyperviseurs ?"*

**Ta réponse** :
> "ESXi 8.0 U3e est la première version gratuite depuis le rachat par Broadcom en 2025, ce qui nous permet de tester un standard industriel. Windows Server 2022 offre 180 jours d'évaluation pour Hyper-V. Proxmox VE et XCP-ng sont open source, très utilisés en PME et startup. Cela nous donne une vision complète du marché actuel."[^2][^5][^6][^8]

### Partie technique (1-2 min)

**Question probable** : *"Quelle est la différence entre un ISO standard et un ISO netinstall ?"*

**Ta réponse** :
> "L'ISO standard (Full) contient tous les packages (~661 Mo pour XCP-ng). Le netinstall est léger (~172 Mo) et télécharge les packages depuis Internet pendant l'installation. En nested virtualization, on préfère le Full car les VMs virtualisées ont parfois des problèmes réseau au démarrage."[^7][^8]

### Justification du choix

**Question probable** : *"Pourquoi ne pas tout faire avec un seul hyperviseur ?"*

**Ta réponse** :
> "En entreprise, le choix de l'hyperviseur dépend du budget (VMware = cher mais complet), de la stack (Hyper-V = écosystème Microsoft), ou du besoin de liberté (Proxmox/XCP-ng = open source). Tester les 4 nous permet de comparer performances, interfaces, et compatibilité pour conseiller un client selon ses besoins."[^1]

***

##  9. RISQUES ET ERREURS FRÉQUENTES

###  Erreur \#1 : Compte Broadcom non vérifié

**Symptôme** : "Access Denied" sur la page de téléchargement ESXi.

**Solution** :

- Vérifie ton email (spam/courrier indésirable)
- Clique sur le lien de confirmation
- Reconnecte-toi


###  Erreur \#2 : Téléchargement interrompu

**Symptôme** : ISO corrompu (erreur au montage dans VMware).

**Solution** :

- Utilise un gestionnaire de téléchargement (Free Download Manager)
- Vérifie le SHA256 avec PowerShell :

```powershell
Get-FileHash C:\ISO_Nexus\XCP-ng\xcp-ng-8.3.0.iso -Algorithm SHA256
```


###  Erreur \#3 : Mauvaise version d'ESXi

**Symptôme** : Tu télécharges ESXi 7.x au lieu de 8.0 U3e.

**Solution** : Sur Broadcom, cherche **spécifiquement "8.0U3e"** dans le nom du fichier.

###  Erreur \#4 : ISO Debian trop gros

**Symptôme** : Tu télécharges le DVD complet (4 Go) au lieu du netinst.

**Solution** : Le netinst Debian fait ~400 Mo. Si ton fichier dépasse 1 Go, ce n'est pas le bon.

###  Erreur \#5 : Windows Server non-évaluation

**Symptôme** : Le site demande une clé produit.

**Solution** : Utilise **spécifiquement** le lien Evaluation Center (`evalcenter`).

***

##  10. CONSEILS POUR OPTIMISER (8 Go RAM)

###  Bonnes pratiques

1. **Télécharge en arrière-plan** :
    - Lance tous les téléchargements en même temps (pas d'impact RAM)
    - Continue à travailler sur le Job 01 (partie théorique) pendant ce temps
2. **Vérifie l'espace disque AVANT** :

```powershell
Get-PSDrive C | Select-Object Used,Free
```

    - Assure-toi d'avoir **minimum 15 Go libres** (7 Go ISO + 8 Go VMs futures)
3. **Organise dès le départ** :
    - Structure claire = gain de temps sur les 7 jours
    - Renomme les ISO avec des noms courts :
        - `esxi-8.0u3e.iso`
        - `win2022-eval.iso`
        - `proxmox-8.iso`
        - `xcpng-8.3.iso`
        - `debian-12.iso`
4. **Prépare un fichier `checksums.txt`** :

```
ESXi: [hash SHA256]
Windows: [hash SHA256]
Proxmox: [hash SHA256]
XCP-ng: 4d6f5a99da0d70920bc313470ad2b14decab66038f0863ca68a2b81126ee2977
Debian: [hash SHA256]
```

5. **Sauvegarde sur clé USB** :
    - Une fois téléchargés, copie tout sur une clé USB 16 Go
    - Si ton PC plante, tu ne re-télécharges pas 7 Go

***

##  CHECKLIST DE VALIDATION JOB 03

Avant de passer au Job 04, vérifie :

- [ ] Les 5 ISO sont téléchargés et organisés dans `C:\ISO_Nexus\`
- [ ] Chaque ISO a la bonne taille (voir tableau ci-dessous)
- [ ] Les 7 screenshots obligatoires sont capturés et nommés (`01_dossier_iso.png`, etc.)
- [ ] Le compte Broadcom est créé et vérifié
- [ ] Tu as noté les liens de téléchargement dans un fichier `sources.md` pour ton GitHub
- [ ] Tu comprends la différence Type 1 vs Type 2 (pour l'oral)


### Tailles attendues :

| ISO | Taille attendue |
| :-- | :-- |
| ESXi 8.0 U3e | ~400 Mo |
| Windows Server 2022 | 4,71 Go [^4] |
| Proxmox VE 8.x | ~1 Go |
| XCP-ng 8.3 LTS | 661 Mo [^8] |
| Debian 12 netinst | ~400 Mo |
| **TOTAL** | **~7 Go** |


***

##  RESSOURCES POUR LE GITHUB

Crée un fichier `JOB03_SOURCES.md` dans ton repo avec :

```markdown
# Sources Job 03

## VMware ESXi 8.0 U3e
- Site officiel : https://support.broadcom.com
- Documentation : https://docs.broadcom.com
- Article IT-Connect : https://www.it-connect.fr/la-version-gratuite-de-vmware-esxi-free-est-de-retour-en-2025/

## Windows Server 2022
- Evaluation Center : https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
- Guide installation : https://windows8facile.fr/telecharger-windows-server-2022/

## Proxmox VE
- Site officiel : https://www.proxmox.com/en/downloads
- Wiki : https://pve.proxmox.com/wiki/Downloads

## XCP-ng
- Site officiel : https://xcp-ng.org
- Documentation : https://docs.xcp-ng.org
- GitHub : https://github.com/xcp-ng/xcp/releases

## Debian
- Site officiel : https://www.debian.org/distrib/netinst
```

***

##  TEMPS ESTIMÉ

**Job 03 complet** : **2h-3h** (selon ta connexion Internet)

- Téléchargements : 1h-2h (en parallèle)
- Organisation + screenshots : 30 min
- Documentation GitHub : 30 min

**Conseil** : Fais ça **Jour 1 matin** avec le Job 00 (BIOS + VMware).

***

