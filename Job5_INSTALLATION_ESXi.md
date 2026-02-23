## **📋 Objectif du Job**

- Installer VMware ESXi 6.7 dans une VM sur VMware Workstation Pro
    
- Configurer la virtualisation imbriquée (nested virtualization)
    
- Créer une VM Debian sans interface graphique dessus
    
- Documenter chaque étape avec screenshots pour la soutenance
    

---

## **⚙️ ÉTAPE 1 – DIMENSIONNEMENT VM ESXi (contrainte 8 Go RAM)**

## **Configuration recommandée :**

|Composant|Valeur|Justification|
|---|---|---|
|**vCPU**|2 cœurs|Minimum pour ESXi + nested|
|**RAM**|**4 Go**|ESXi min 4 Go + VM Debian (1 Go)|
|**Disque**|**60 Go**|ESXi (8 Go) + Debian (8 Go) + marge|
|**Réseau**|**NAT**|Accès Internet + isolation|
|**Carte réseau**|**E1000e**|Compatible ESXi 6.7|

⚠️ **Note RAM** : Avec 8 Go total, tu auras :

- Windows 11 : ~3 Go
    
- VM ESXi : 4 Go
    
- **Total : 7 Go → reste 1 Go pour le système**
    

**Solution** : Ferme Chrome, Discord, etc. pendant l'installation !

---

## **🔧 ÉTAPE 2 – CRÉATION DE LA VM ESXi DANS VMWARE WORKSTATION**

## **Actions détaillées :**

1. **Ouvre VMware Workstation Pro**
    
2. **Clic** sur **"Create a New Virtual Machine"**
    
3. **Configuration type** : Sélectionne **"Custom (advanced)"** ➔ Next
    

## **Paramètres à configurer :**

## **Étape 1 : Hardware compatibility**

- Sélectionne **"Workstation 16.x"** ou supérieur ➔ Next
    

## **Étape 2 : Guest Operating System**

- Sélectionne **"I will install the operating system later"** ➔ Next
    
- **Guest OS** : **VMware ESXi 6.7** (dans la liste déroulante)
    
- Si pas dans la liste : **VMware ESXi 6** ou **Other 64-bit**
    

## **Étape 3 : Nom et emplacement**

- **Nom** : `ESXi-6.7-Nexus`
    
- **Location** : Un dossier dédié (ex : `D:\VMs\ESXi`)
    

## **Étape 4 : Processeur**

- **Number of processors** : 1
    
- **Cores per processor** : 2
    
- **Total** = 2 vCPU
    

## **Étape 5 : Mémoire**

- **4096 Mo** (4 Go)
    

## **Étape 6 : Réseau**

- Sélectionne **NAT**
    

## **Étape 7 : I/O Controller**

- **LSI Logic SAS** (recommandé pour ESXi)
    

## **Étape 8 : Virtual Disk Type**

- **SCSI** (Recommended)
    

## **Étape 9 : Disque**

- **Create a new virtual disk**
    
- **Taille** : **40 Go**
    
- **Store as single file** (plus rapide)
    

## **Étape 10 : Disk File**

- Nom : `ESXi-6.7-Nexus.vmdk`
    

## **Étape 11 : Finalisation**

- **Customize Hardware** AVANT de finir
    

---

## **🔥 ÉTAPE 3 – PARAMÈTRE CRITIQUE : ACTIVER NESTED VIRTUALIZATION**

## **Dans "Customize Hardware" :**

1. **Processeur** :
    
    - ✅ **Cocher "Virtualize Intel VT-x/EPT or AMD-V/RVI"**
        
    - ⚠️ **SANS CE PARAMÈTRE, TU NE POURRAS PAS CRÉER DE VM DANS ESXi**
        
2. **CD/DVD (IDE)** :
    
    - ✅ **Use ISO image file**
        
    - Parcourir et sélectionner **`ESXi 6.7.iso`**
        
3. **Display** :
    
    - Décoche **"Accelerate 3D graphics"** (incompatible ESXi)
        
4. **USB Controller** :
    
    - Peut être supprimé (inutile)
        
5. **Clic** sur **Close** ➔ **Finish**
    

![[Pasted image 20260223151331.png]]