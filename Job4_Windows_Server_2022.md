***

#  JOB 04 – HYPER-V (JOUR 2)

## Installation Windows Server 2022 + Rôle Hyper-V + VM Debian


***

##  OBJECTIF TECHNIQUE

Installer **Windows Server 2022** dans une VM VMware, activer le rôle **Hyper-V**, puis créer une **VM Debian sans interface graphique** à l'intérieur (nested virtualization).

***

##  DIMENSIONNEMENT RECOMMANDÉ

### VM Windows Server 2022 (Host Hyper-V)

| Ressource | Valeur | Justification |
| :-- | :-- | :-- |
| **vCPU** | 2 | Minimum pour Windows Server + Hyper-V + VM invitée |
| **RAM** | 4 Go | Windows Server (2 Go) + VM Debian (1 Go) + Overhead (1 Go) |
| **Disque** | 60 Go | OS Windows (40 Go) + VM Debian (8 Go) + Marge (12 Go) |
| **Type disque** | Thin Provisioning | Économie d'espace réel sur ton SSD |
| **Réseau** | NAT | Accès Internet simple pour Windows Update + apt |


***

##  ÉTAPE 1 : CRÉATION DE LA VM WINDOWS SERVER 2022

### Dans VMware Workstation Pro

1. **Nouvelle VM** → Typical → Next
2. **ISO** : Sélectionne `Windows Server 2022.iso`
3. **Système d'exploitation** : Microsoft Windows / Windows Server 2022
4. **Nom** : `WinSrv2022-HyperV`
5. **Disque** : 60 GB / Single file
6. **Customize Hardware** :
    - RAM : 4096 MB
    - Processors : 2 cores
    - Network Adapter : NAT
    - **CRUCIAL** : Coche **"Virtualize Intel VT-x/EPT or AMD-V/RVI"**
        - VM Settings → Processors →  Virtualize Intel VT-x/EPT
          ![[Pasted image 20260217224009.png]]
7. **Finish** → Power On

***

##  ÉTAPE 2 : INSTALLATION WINDOWS SERVER 2022

1. **Langue** : Français (ou Anglais)
2. **Install Now**
3. **Version** : **Windows Server 2022 Standard (Desktop Experience)**
    -  Prends la version avec GUI, pas Core
4. **Type d'installation** : Custom
5. **Disque** : Next (partition automatique)
6. **Attendre installation** (10-15 min)
7. **Mot de passe Administrateur** : `P@ssw0rd!` (note-le !)

### Première connexion

1. Ctrl+Alt+Del → Mot de passe
2. **Windows Update** (optionnel mais recommandé) :
    - Settings → Windows Update → Check for updates
    - Redémarre si nécessaire

***

##  ÉTAPE 3 : ACTIVATION DU RÔLE HYPER-V

### Via Server Manager

1. **Server Manager** s'ouvre automatiquement
2. Clique **"Add roles and features"**
3. Next → Next → Next
4. **Server Roles** :  **Hyper-V**
    - Une popup apparaît → **Add Features**
5. Next → Next → Next
6. **Virtual Switches** : Coche **"Default Switch"** (ou skip)
7. **Virtual Machine Migration** : Skip (Next)
8. Next → **Install**
9. **Redémarrage obligatoire** → Coche "Restart automatically" → Install
10. Le serveur redémarre automatiquement
![[Pasted image 20260217230749.png]]
### Vérification

Après redémarrage :

- Ouvre **Hyper-V Manager** :
    - Start Menu → Recherche "Hyper-V Manager"
    - Tu dois voir ton serveur dans la liste

![[Pasted image 20260217231731.png]]


***

##  ÉTAPE 4 : CRÉATION DE LA VM DEBIAN (SANS GUI)

### Télécharge Debian Netinst (si pas déjà fait)

- ISO : `debian-12.x.x-amd64-DVD.iso`
- Transfère-la dans la VM Windows via :
    - VM → Install VMware Tools
    - Ou partage réseau/clé USB


### Dans Hyper-V Manager

1. **Action** → **New** → **Virtual Machine**
2. **Wizard** :
    - Name : `Debian-Nexus`
    - Generation : **Generation** 1
    - Memory : **1024 MB** (1 Go) /  Décoche "Dynamic Memory"
    - Network : **Default Switch**
    - Create Virtual Hard Disk : **8 GB** (Fixed ou Dynamic, Dynamic recommandé)
    - Installation Options : **Install from ISO** → Browse → Sélectionne Debian ISO
3. **Finish**

![[Pasted image 20260218005451.png]]
### Configuration AVANT démarrage

 **CRUCIAL** : Désactive Secure Boot

1. Right-click sur VM `Debian-Nexus` → **Settings**
2. **Security** →  Décoche **"Enable Secure Boot"**
3. **Processor** : 2 Virtual Processors
4. **OK**

***

##  ÉTAPE 5 : INSTALLATION DEBIAN (MODE CLI)

### Démarrage

1. Right-click → **Connect**
2. **Start**

### Installation guidée

1. **Debian GNU/Linux installer menu** → **Install** (pas Graphical Install)
2. **Langue** : French
3. **Pays** : France
4. **Clavier** : Français
5. **Nom de machine** : `debian-nexus`
6. **Domaine** : (laisser vide)
7. **Mot de passe root** : `toor` (ou autre, note-le)
8. **Utilisateur** : `admin` / Mot de passe : `admin`
9. **Partitionnement** : **Assisté - utiliser le disque entier** → Continue → Continue → Oui
10. **Miroir Debian** : France → deb.debian.org
11. **Proxy** : (laisser vide)
12. **Popularity contest** : Non
13. **Logiciels** :
    -  Décoche **"Debian desktop environment"**
    -  Décoche **"GNOME"**
    -  **"SSH server"**
    -  **"Standard system utilities"**
14. **GRUB** : Oui → `/dev/sda`
15. **Finish** → Reboot
![[Pasted image 20260219164152.png]]
***

##  ÉTAPE 6 : VÉRIFICATION ET TESTS

### Connexion à Debian

1. Login : `root` / Password : `toor`
2. Vérifie IP :

```bash
ip a
```

3. Teste Internet :

```bash
ping -c 4 google.com
```

4. Update système :

```bash
apt update && apt upgrade -y
```

![[Pasted image 20260221155802.png]]

### Depuis Windows Server

1. Ouvre **Hyper-V Manager**
2. Vérifie l'état de la VM (Running)
3. Regarde la RAM/CPU utilisés

***

##  SCREENSHOTS OBLIGATOIRES POUR LA SOUTENANCE

| \# | Screenshot | Explication orale |
| :-- | :-- | :-- |
| 1 | VMware : Config VM Windows (CPU/RAM + **Virtualize VT-x coché**) | "Virtualisation imbriquée activée pour permettre Hyper-V" |
| 2 | Windows Server : Installation du rôle Hyper-V | "Activation du rôle Hyper-V via Server Manager" |
| 3 | Hyper-V Manager : Interface principale avec serveur listé | "Console de gestion Hyper-V opérationnelle" |
| 4 | Hyper-V Manager : Création VM Debian (Settings avec 2 vCPU, 1 Go RAM) | "Dimensionnement selon les specs du projet" |
| 5 | Hyper-V Manager : VM Debian en état Running | "VM Debian fonctionnelle sous Hyper-V" |
| 6 | Console Debian : `ip a` et `uname -a` | "Système Debian sans GUI, réseau fonctionnel" |
| 7 | Gestionnaire des tâches Windows : Utilisation RAM/CPU globale | "Monitoring des ressources avec 8 Go RAM total" |


***

##  POINTS CLÉS À EXPLIQUER À L'ORAL

### Architecture

> "Nous avons créé une VM Windows Server 2022 dans VMware Workstation (hyperviseur Type 2), puis activé le rôle Hyper-V (hyperviseur Type 1) à l'intérieur. Cela s'appelle de la **virtualisation imbriquée** ou **nested virtualization**."

### Pourquoi Generation 2 ?

> "Pour Debian, on a choisi Generation 2 car c'est UEFI moderne, plus performant. On a dû désactiver Secure Boot car Debian n'a pas de certificat signé Microsoft par défaut."

### Dimensionnement

> "Avec 8 Go de RAM totale, on alloue 4 Go à Windows Server : 2 Go pour l'OS, 1 Go pour Hyper-V et la VM Debian, et 1 Go de marge. Le reste (4 Go) reste disponible pour l'hôte Windows 11."

### Réseau NAT

> "Le mode NAT permet à la VM d'accéder à Internet via l'hôte sans configuration complexe, idéal pour un lab de test."

***

##  ERREURS FRÉQUENTES ET SOLUTIONS

| Erreur                      | Cause                       | Solution                                         |
| :-------------------------- | :-------------------------- | :----------------------------------------------- |
| Hyper-V n'installe pas      | VT-x pas activé dans VMware | VM Settings → Processors →  Virtualize VT-x      |
| Debian ne boot pas en Gen 2 | Secure Boot activé          | Settings → Security →  Disable Secure Boot       |
| VM Debian très lente        | Trop de VMs actives         | Éteins les autres VMs, garde seulement Hyper-V   |
| Pas d'accès Internet        | Switch réseau non configuré | Utilise "Default Switch" dans Hyper-V            |
| Windows freeze              | RAM insuffisante            | Ferme Chrome/apps inutiles sur l'hôte Windows 11 |


***

##  OPTIMISATION RAM (CRUCIAL AVEC 8 GO)

### Après avoir pris tous les screenshots

1. **Arrête la VM Debian** :

```bash
shutdown -h now
```

2. **Ferme Hyper-V Manager**
3. **Arrête la VM Windows Server** dans VMware
4. **Suspend** (plutôt que Shut Down) pour reprendre rapidement plus tard

### Avant de passer au Job 05

- Supprime la VM Debian dans Hyper-V (garde les screenshots)
- Ou arrête complètement Windows Server si tu ne reviens pas dessus

***

##  CONCEPTS CLÉS À RETENIR

### Type 1 vs Type 2

- **Hyper-V** = Type 1 (bare-metal, même si ici il est "imbriqué")
- **VMware Workstation** = Type 2 (hosted, tourne sur Windows 11)


### Virtualisation imbriquée

- Permet de tester des hyperviseurs sans serveur physique dédié
- Performance réduite mais suffisante pour lab pédagogique


### Dynamic Memory vs Static

- Dynamic = Hyper-V alloue la RAM à la demande
- Static = RAM fixe (recommandé pour les labs avec ressources limitées)

***

##  CHECKLIST AVANT DE PASSER AU JOB 05

- ✅ Windows Server 2022 installé et fonctionnel
- ✅ Rôle Hyper-V activé et Hyper-V Manager accessible
- ✅ VM Debian créée (2 vCPU, 1 Go RAM, 8 Go Disk)
- ✅ Debian installé en CLI (sans GUI)
- ✅ Debian a accès Internet (`ping google.com` OK)
- ✅ 7 screenshots capturés et organisés
- ✅ Notes prises pour expliquer l'architecture à l'oral
- ✅ VM Windows Server arrêtée/suspendue pour libérer RAM