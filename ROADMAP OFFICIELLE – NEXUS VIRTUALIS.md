_(Optimisée 8 Go RAM – 7 jours – Soutenance collective)_

---

# 🔹 PHASE 0 – PRÉREQUIS TECHNIQUES (Jour 1 – Matin)

## 🎯 Objectif

Préparer un environnement stable pour la virtualisation imbriquée.

## Actions

- Activer Intel VT-x / SVM dans BIOS
    
- Installer VMware Workstation Pro
    
- Télécharger ISO :
    
    - Windows Server 2022
        
    - VMware ESXi
        
    - Proxmox VE
        
    - XCP-ng
        
    - Debian netinst
        

## 📸 Screens obligatoires

- BIOS → Virtualization Enabled
    
- VMware installé
    
- Dossier ISO organisé
    
- Paramètre NAT configuré
    

---

# 🔹 PHASE 1 – BASES THÉORIQUES (Jour 1 – Après-midi)

## 🎯 Objectif

Comprendre Type 1 vs Type 2 avant pratique.

## À produire

- Tableau comparatif
    
- Schéma architecture
    
- Cas d’usage entreprise
    

## 📸 Screens

- Schéma comparatif
    
- Tableau synthèse
    
- Slide "Pourquoi entreprise choisit Type 1"
    

---

# 🔹 PHASE 2 – HYPER-V (Jour 2)

Hyperviseur : Hyper-V

## Architecture

VMware (Type 2)  
→ Windows Server 2022  
→ Activation Hyper-V  
→ VM Debian

## Dimensionnement (adapté 8 Go RAM)

|Élément|Valeur|
|---|---|
|vCPU|2|
|RAM|4 Go|
|Disque|60 Go|

## 📸 Screens clés

- Création VM Windows
    
- Activation rôle Hyper-V
    
- Interface Hyper-V Manager
    
- Création VM Debian
    
- Debian en CLI
    
- Utilisation RAM/CPU
    

🧠 Supprimer la VM après capture pour libérer RAM.

---

# 🔹 PHASE 3 – ESXi (Jour 3)

Hyperviseur : VMware ESXi

## Paramètre critique

Activer "Expose hardware virtualization"

## 📸 Screens

- Paramètre virtualisation imbriquée
    
- Installation ESXi
    
- Interface Web
    
- VM Debian créée
    
- VM en fonctionnement
    

Supprimer ensuite.

---

# 🔹 PHASE 4 – PROXMOX VE (Jour 4)

Hyperviseur : Proxmox VE

| vCPU | 2 |  
| RAM | 4 Go |  
| Disk | 40 Go |

## 📸 Screens

- Installation
    
- Interface Web
    
- Création VM Debian
    
- Vue stockage
    
- Vue ressources
    

---

# 🔹 PHASE 5 – XCP-ng (Jour 5)

Hyperviseur : XCP-ng

## 📸 Screens

- Installation
    
- Interface gestion
    
- VM Debian
    
- Monitoring ressources
    

---

# 🔹 PHASE 6 – MIGRATIONS (Jour 6)

🎯 Objectif : Comprendre formats disques & compatibilité

## Migration réelle conseillée

Proxmox → ESXi

## Autres migrations : documentées théoriquement

## 📸 Screens

- Export OVA / QCOW2
    
- Conversion disque
    
- Import VM
    
- VM fonctionnelle
    
- Vérification IP
    

---

# 🔹 PHASE 7 – BACKUP (Jour 7 – Matin)

Installer :  
Proxmox Backup Server

## À configurer

- Backup toutes les 2h
    
- Rétention : 3 backups
    

## 📸 Screens

- Datastore créé
    
- Job planifié
    
- Historique sauvegardes
    
- Test restauration
    

---

# 🔹 PHASE 8 – CLUSTER (À 2)

Cluster sur Proxmox VE

## Mode réseau

Bridged + IP statique (à l’école)

## 📸 Screens

- Création cluster
    
- Ajout nœud
    
- Migration VM entre nœuds
    
- Vérification quorum
    

---

# 🎤 STRUCTURE SOUTENANCE

1. Introduction & architecture
    
2. Comparaison hyperviseurs
    
3. Démonstration migration
    
4. Démonstration backup
    
5. Cluster
    
6. Choix final pour entreprise