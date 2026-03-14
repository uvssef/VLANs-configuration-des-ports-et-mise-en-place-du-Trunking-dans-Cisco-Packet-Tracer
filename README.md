# TP Réseau : Mise en œuvre de VLANs et Trunking

## 1. Auteur
- **Nom :** [À compléter par l'étudiant]
- **Prénom :** [À compléter par l'étudiant]

## 2. Objectif du TP
Ce travail pratique a pour objectif de configurer et de vérifier le fonctionnement des VLANs (Virtual Local Area Networks) et des liaisons trunk dans un environnement réseau commuté. En suivant le tutoriel, vous apprendrez à :
- Créer et nommer des VLANs sur des commutateurs Cisco.
- Assigner des ports d'accès à des VLANs spécifiques.
- Configurer des liens trunk pour transporter le trafic de plusieurs VLANs entre les commutateurs.
- Comprendre le rôle du VLAN natif sur un trunk.
- Configurer des adresses IP sur des PC et tester la connectivité au sein d'un même VLAN.
- Vérifier l'isolation entre les différents VLANs.

## 3. Topologie du réseau
La topologie à reproduire dans Cisco Packet Tracer est la suivante, basée sur le tutoriel :
- **3 commutateurs** : SWA, SWB, SWC (modèles 2960 recommandés).
- **7 PC** : Répartis sur SWB et SWC.
- **Liens** :
    - SWA (G0/1) <--> SWB (G0/1) : Lien trunk statique.
    - SWA (G0/2) <--> SWC (G0/2) : Lien trunk dynamique (négocié avec DTP).

*(Il est conseillé d'insérer ici une capture d'écran de la topologie réalisée sous Packet Tracer.)*

## 4. Plan d'adressage et VLANs

### 4.1. VLANs créés
| Numéro de VLAN | Nom du VLAN   |
| :-------------- | :------------ |
| 10             | Admin         |
| 20             | Comptes (Accounts) |
| 30             | Ressources humaines (HR) |
| 40             | Voix (Voice)  |
| 99             | Management    |
| 100            | Natif (Native) |

### 4.2. Table d'adressage des périphériques
| Appareil | Interface | Adresse IP   | Masque sous-réseau | Port du commutateur | VLAN |
| :------- | :-------- | :----------- | :------------------ | :------------------ | :--- |
| PC1      | Carte réseau | 192.168.10.10 | 255.255.255.0       | SWB F0/1            | 10   |
| PC2      | Carte réseau | 192.168.20.20 | 255.255.255.0       | SWB F0/2            | 20   |
| PC3      | Carte réseau | 192.168.30.30 | 255.255.255.0       | SWB F0/3            | 30   |
| PC4      | Carte réseau | 192.168.10.11 | 255.255.255.0       | SWC F0/1            | 10   |
| PC5      | Carte réseau | 192.168.20.21 | 255.255.255.0       | SWC F0/2            | 20   |
| PC6      | Carte réseau | 192.168.30.31 | 255.255.255.0       | SWC F0/3            | 30   |
| PC7      | Carte réseau | 192.168.10.12 | 255.255.255.0       | SWC F0/4            | 10   |
| SWA      | Interface VLAN 99 | 192.168.99.252 | 255.255.255.0       | N/A                 | 99   |
| SWB      | Interface VLAN 99 | 192.168.99.253 | 255.255.255.0       | N/A                 | 99   |
| SWC      | Interface VLAN 99 | 192.168.99.254 | 255.255.255.0       | N/A                 | 99   |

## 5. Configuration des équipements

### 5.1. Configuration des VLANs (sur SWA, SWB, SWC)
```bash
enable
configure terminal
vlan 10
  name Admin
vlan 20
  name Accounts
vlan 30
  name HR
vlan 40
  name Voice
vlan 99
  name Management
vlan 100
  name Native
end
