<div align="center">

# 🖥️ NexusVirtualis

**Projet de virtualisation imbriquée — Formation Administration Système**

![VMware](https://img.shields.io/badge/VMware-Workstation_Pro-607078?style=for-the-badge&logo=vmware)
![ESXi](https://img.shields.io/badge/VMware-ESXi-607078?style=for-the-badge&logo=vmware)
![Proxmox](https://img.shields.io/badge/Proxmox-VE-E57000?style=for-the-badge&logo=proxmox)
![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D6?style=for-the-badge&logo=windows)
![Debian](https://img.shields.io/badge/Debian-Linux-A81D33?style=for-the-badge&logo=debian)

</div>

---

## 📌 À propos du projet

**NexusVirtualis** est un projet de lab pédagogique axé sur la **virtualisation imbriquée (nested virtualization)**. Il consiste à installer et configurer des hyperviseurs de **Type 1** (ESXi, Proxmox VE) au sein de machines virtuelles hébergées par un hyperviseur de **Type 2** (VMware Workstation Pro), le tout sur un poste de travail Windows 11.

L'objectif est de comprendre, déployer et documenter plusieurs environnements virtualisés afin de maîtriser les architectures utilisées en datacenter, sans nécessiter de serveur physique dédié.

---

## 🗂️ Structure du projet

```

NexusVirtualis/
│
├── Job1_concepts_de_base.md               \# Théorie : hyperviseurs Type 1 vs Type 2
├── Job2_installer_les_outils_virtualisation.md  \# Installation de VMware Workstation Pro
├── Job3_récupérer_les_images_ISO.md       \# Récupération et vérification des ISOs
├── Job4_Windows_Server_2022.md            \# Installation de Windows Server 2022
├── Job5_INSTALLATION_ESXi.md             \# Installation de VMware ESXi
├── Job6_Proxmox_VE.md                    \# Installation de Proxmox VE
│
├── pdf/                                  \# Documentation PDF associée
├── screen Job2/                          \# Captures d'écran – Job 2
├── screen Job3/                          \# Captures d'écran – Job 3
├── screen Job4/                          \# Captures d'écran – Job 4
├── screen Job5/                          \# Captures d'écran – Job 5
└── screen Job6/                          \# Captures d'écran – Job 6

```

---

## 📋 Contenu des Jobs

| Job | Titre | Description |
|-----|-------|-------------|
| 01 | Concepts de base | Théorie hyperviseurs Type 1 vs Type 2, virtualisation imbriquée, formats de disques |
| 02 | Installer les outils | Installation et configuration de VMware Workstation Pro |
| 03 | Récupérer les ISO | Téléchargement et vérification des images ISO (ESXi, Proxmox, Debian, WinServer) |
| 04 | Windows Server 2022 | Déploiement d'une VM Windows Server 2022 sous VMware Workstation |
| 05 | VMware ESXi | Installation d'ESXi en nested virtualization |
| 06 | Proxmox VE | Installation de Proxmox VE en nested virtualization |

---

## 🧰 Technologies utilisées

- **VMware Workstation Pro** — Hyperviseur Type 2 (hôte)
- **VMware ESXi** — Hyperviseur Type 1 Bare Metal (nested)
- **Proxmox VE** — Hyperviseur Type 1 open-source (nested)
- **Windows Server 2022** — Système invité
- **Debian Linux** — Système invité CLI
- **Windows 11** — Système hôte physique

---

## ⚙️ Prérequis techniques

Avant de commencer les labs, assure-toi de disposer de :

- **CPU** compatible virtualisation : Intel VT-x ou AMD-V activé dans le BIOS
- **RAM** : 8 Go minimum (16 Go recommandé)
- **Stockage** : 100 Go d'espace libre minimum
- **VMware Workstation Pro** : option **"Virtualiser Intel VT-x/EPT"** activée dans les paramètres de chaque VM
- **Accès Internet** pour le téléchargement des images ISO

> ⚠️ Avec 8 Go de RAM, n'exécuter **qu'un seul hyperviseur nested à la fois**.

---

## 🏗️ Architecture de la virtualisation imbriquée

```

┌────────────────────────────────────────────────────┐
│             WINDOWS 11 (Hôte physique)             │
│  ┌──────────────────────────────────────────────┐  │
│  │        VMware Workstation Pro (Type 2)       │  │
│  │  ┌──────────────┐   ┌──────────────────────┐ │  │
│  │  │  ESXi (T1)   │   │    Proxmox VE (T1)   │ │  │
│  │  │  ┌─────────┐ │   │  ┌────────────────┐  │ │  │
│  │  │  │ VM Deb  │ │   │  │  VM Win Server │  │ │  │
│  │  │  └─────────┘ │   │  └────────────────┘  │ │  │
│  │  └──────────────┘   └──────────────────────┘ │  │
│  └──────────────────────────────────────────────┘  │
│              MATÉRIEL PHYSIQUE (CPU, RAM)           │
└────────────────────────────────────────────────────┘

```

---

## 📊 Hyperviseur Type 1 vs Type 2

| Critère | Type 1 (Bare Metal) | Type 2 (Hosted) |
|---------|---------------------|-----------------|
| Installation | Directement sur le matériel | Sur un OS existant |
| Performances | Excellentes (~2-5% overhead) | Moyennes (~10-15% overhead) |
| Isolation | Maximale | Dépend de l'OS hôte |
| Cas d'usage | Production, datacenter | Labs, développement, formation |
| Exemples | ESXi, Proxmox, Hyper-V | VMware Workstation, VirtualBox |

---

## 🚀 Démarrage rapide

1. Cloner ce dépôt :
   ```bash
   git clone https://github.com/Jericho-Cortez/NexusVirtualis.git
```

2. Consulter les fichiers dans l'ordre des Jobs (Job1 → Job6)
3. Suivre les étapes documentées dans chaque `.md` en vous appuyant sur les captures d'écran associées

---

## 📸 Captures d'écran

Les captures d'écran de chaque étape d'installation sont disponibles dans les dossiers `screen JobX/` correspondants.

---

## 👤 Auteur

**Jericho Cortez**
Étudiant en Bachelor Administration Système \& Cybersécurité
[GitHub](https://github.com/Jericho-Cortez)

---

## 📄 Licence

Ce projet est réalisé dans un cadre pédagogique. Toute réutilisation à des fins éducatives est la bienvenue.

```
