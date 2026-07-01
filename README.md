# 🏦 CCNA2 — VLANs + Router-on-a-Stick + STP + DHCPv4 | AfriBank Bénin

![Cisco](https://img.shields.io/badge/Cisco-CCNA2-blue?style=for-the-badge&logo=cisco&logoColor=white)
![PacketTracer](https://img.shields.io/badge/Packet%20Tracer-8.x-orange?style=for-the-badge&logo=cisco)
![Status](https://img.shields.io/badge/Status-✅%20Completed-brightgreen?style=for-the-badge)
![VLANs](https://img.shields.io/badge/VLANs-4-purple?style=for-the-badge)
![Switches](https://img.shields.io/badge/Switches-3-red?style=for-the-badge)
![PCs](https://img.shields.io/badge/PCs-12-yellow?style=for-the-badge)
![DHCP](https://img.shields.io/badge/DHCP-Automatique-green?style=for-the-badge)
![STP](https://img.shields.io/badge/STP-Rapid%20PVST+-blue?style=for-the-badge)

---

## 📋 Description

Ce TP simule le réseau de la banque **AfriBank Bénin** 🏦 basée
à Cotonou. Le réseau est segmenté en **4 VLANs** représentant
les différents départements de la banque. Le routage inter-VLAN
est assuré par la méthode **Router-on-a-Stick**, la redondance
par **STP Rapid PVST+** et la distribution des IPs par **DHCPv4**.

### Objectifs
- ✅ Créer et configurer **4 VLANs**
- ✅ Configurer le routage inter-VLAN **Router-on-a-Stick**
- ✅ Configurer **STP Rapid PVST+** avec Root Bridge principal et secondaire
- ✅ Configurer **DHCPv4** pour distribuer les IPs automatiquement
- ✅ Tester la **redondance STP** en cas de panne
- ✅ Tester la **connectivité inter-VLAN**

---

## 🖥️ Équipements

| Équipement | Modèle | Nom | Rôle |
|-----------|--------|-----|------|
| 🔀 Routeur | Cisco 2911 | R1 | Routage inter-VLAN + DHCP |
| 🔌 Switch | Cisco 2960-24TT | SW1 | Distribution + Root Bridge STP |
| 🔌 Switch | Cisco 2960-24TT | SW2 | Accès étage 1 + Root Secondaire |
| 🔌 Switch | Cisco 2960-24TT | SW3 | Accès étage 2 |
| 💻 PC | PC-PT | PC1 à PC12 | Employés (3 par VLAN) |

**Total : 1 routeur + 3 switches + 12 PC**

---

## 🗺️ Topologie

```
                    [R1]
                     |
                   Gig0/0
                     |
                   Gig0/1
              [SW1] ← Root Bridge STP
             /              \
         Gig0/2            Fa0/23
           /                    \
        [SW2]──Fa0/24──────Fa0/24──[SW3]
        /    \                    /    \
    Fa0/1-3  Fa0/4-6         Fa0/1-3  Fa0/4-6
      |         |               |         |
   VLAN10    VLAN20          VLAN30    VLAN40
   PC1-3     PC4-6           PC7-9    PC10-12
```

<img width="1920" height="1080" alt="Capture d’écran du 2026-07-01 23-03-19" src="https://github.com/user-attachments/assets/d206eb39-b0fa-4074-987a-779433fbfad4" />

---

## 🔌 Câblage complet

| De | Port | Vers | Port | Rôle |
|----|------|------|------|------|
| R1 | Gig0/0 | SW1 | Gig0/1 | Lien trunk routeur |
| SW1 | Gig0/2 | SW2 | Gig0/2 | Trunk principal |
| SW1 | Fa0/23 | SW3 | Fa0/23 | Trunk principal |
| SW2 | Fa0/24 | SW3 | Fa0/24 | Lien redondant STP |
| SW2 | Fa0/1 | PC1 | Fa0 | VLAN 10 |
| SW2 | Fa0/2 | PC2 | Fa0 | VLAN 10 |
| SW2 | Fa0/3 | PC3 | Fa0 | VLAN 10 |
| SW2 | Fa0/4 | PC4 | Fa0 | VLAN 20 |
| SW2 | Fa0/5 | PC5 | Fa0 | VLAN 20 |
| SW2 | Fa0/6 | PC6 | Fa0 | VLAN 20 |
| SW3 | Fa0/1 | PC7 | Fa0 | VLAN 30 |
| SW3 | Fa0/2 | PC8 | Fa0 | VLAN 30 |
| SW3 | Fa0/3 | PC9 | Fa0 | VLAN 30 |
| SW3 | Fa0/4 | PC10 | Fa0 | VLAN 40 |
| SW3 | Fa0/5 | PC11 | Fa0 | VLAN 40 |
| SW3 | Fa0/6 | PC12 | Fa0 | VLAN 40 |

---

## 📊 Plan d'adressage

| VLAN | 🏷️ Nom | Réseau | Passerelle | Pool DHCP | PCs | Switch |
|------|--------|--------|------------|-----------|-----|--------|
| 10 | 👔 DIRECTION | 192.168.10.0/24 | 192.168.10.1 | .11 → .50 | PC1,PC2,PC3 | SW2 |
| 20 | 💰 FINANCE | 192.168.20.0/24 | 192.168.20.1 | .11 → .50 | PC4,PC5,PC6 | SW2 |
| 30 | 🏧 CAISSE | 192.168.30.0/24 | 192.168.30.1 | .11 → .50 | PC7,PC8,PC9 | SW3 |
| 40 | 🔒 SECURITE | 192.168.40.0/24 | 192.168.40.1 | .11 → .50 | PC10,PC11,PC12 | SW3 |
| 99 | 🔗 NATIF | — | — | — | — | SW1+SW2+SW3 |

---

## 🖥️ Assignation des ports PC

| PC | IP attendue (DHCP) | Masque | Passerelle | Switch | Port | VLAN |
|----|-------------------|--------|------------|--------|------|------|
| PC1 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 | SW2 | Fa0/1 | 10 |
| PC2 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 | SW2 | Fa0/2 | 10 |
| PC3 | 192.168.10.13 | 255.255.255.0 | 192.168.10.1 | SW2 | Fa0/3 | 10 |
| PC4 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 | SW2 | Fa0/4 | 20 |
| PC5 | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 | SW2 | Fa0/5 | 20 |
| PC6 | 192.168.20.13 | 255.255.255.0 | 192.168.20.1 | SW2 | Fa0/6 | 20 |
| PC7 | 192.168.30.11 | 255.255.255.0 | 192.168.30.1 | SW3 | Fa0/1 | 30 |
| PC8 | 192.168.30.12 | 255.255.255.0 | 192.168.30.1 | SW3 | Fa0/2 | 30 |
| PC9 | 192.168.30.13 | 255.255.255.0 | 192.168.30.1 | SW3 | Fa0/3 | 30 |
| PC10 | 192.168.40.11 | 255.255.255.0 | 192.168.40.1 | SW3 | Fa0/4 | 40 |
| PC11 | 192.168.40.12 | 255.255.255.0 | 192.168.40.1 | SW3 | Fa0/5 | 40 |
| PC12 | 192.168.40.13 | 255.255.255.0 | 192.168.40.1 | SW3 | Fa0/6 | 40 |

---

## ⚙️ Configuration complète

### 🔧 SW1 — Distribution + Root Bridge STP

```cisco
enable
configure terminal
hostname SW1

vlan 10
name DIRECTION
exit
vlan 20
name FINANCE
exit
vlan 30
name CAISSE
exit
vlan 40
name SECURITE
exit
vlan 99
name NATIF
exit

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,40 root primary

interface gig0/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface gig0/2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface fa0/23
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

end
write
```

---

### 🔧 SW2 — Accès étage 1 + Root Bridge secondaire

```cisco
enable
configure terminal
hostname SW2

vlan 10
name DIRECTION
exit
vlan 20
name FINANCE
exit
vlan 99
name NATIF
exit

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,40 root secondary

interface gig0/2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface fa0/24
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface range fa0/1-3
switchport mode access
switchport access vlan 10
spanning-tree portfast
exit

interface range fa0/4-6
switchport mode access
switchport access vlan 20
spanning-tree portfast
exit

end
write
```

---

### 🔧 SW3 — Accès étage 2

```cisco
enable
configure terminal
hostname SW3

vlan 30
name CAISSE
exit
vlan 40
name SECURITE
exit
vlan 99
name NATIF
exit

spanning-tree mode rapid-pvst

interface fa0/23
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface fa0/24
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
exit

interface range fa0/1-3
switchport mode access
switchport access vlan 30
spanning-tree portfast
exit

interface range fa0/4-6
switchport mode access
switchport access vlan 40
spanning-tree portfast
exit

end
write
```

---

### 🔧 R1 — Routeur 2911 (Router-on-a-Stick + DHCP)

```cisco
enable
configure terminal
hostname R1

interface gig0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gig0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface gig0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface gig0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
exit

interface gig0/0.99
encapsulation dot1Q 99 native
exit

interface gig0/0
no shutdown
exit

ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10-DIRECTION
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp pool VLAN20-FINANCE
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp pool VLAN30-CAISSE
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp pool VLAN40-SECURITE
network 192.168.40.0 255.255.255.0
default-router 192.168.40.1
dns-server 8.8.8.8
exit

end
write
```

---

### 🖥️ Configuration PC en DHCP

```
Sur chaque PC :
1. Cliquez sur le PC
2. Desktop
3. IP Configuration
4. Cochez DHCP ← automatique !
```

---

## 🔍 Commandes de vérification

```cisco
! Sur R1
R1# show ip interface brief
R1# show ip dhcp binding
R1# show ip dhcp pool
R1# show ip route

! Sur SW1
SW1# show vlan brief
SW1# show interfaces trunk
SW1# show spanning-tree
SW1# show spanning-tree vlan 10

! Sur SW2
SW2# show spanning-tree
SW2# show interfaces trunk

! Sur SW3
SW3# show spanning-tree
SW3# show interfaces trunk
```

---

### 📊 Résultat attendu — show ip dhcp binding

```
IP address      Client-ID          Lease expiration  Type
192.168.10.11   0100.0c29.xx.xx    --                Automatic
192.168.20.11   0100.0c29.xx.xx    --                Automatic
192.168.30.11   0100.0c29.xx.xx    --                Automatic
192.168.40.11   0100.0c29.xx.xx    --                Automatic
```

### 📊 Résultat attendu — show spanning-tree SW1

```
VLAN0010
  This bridge is the root ✅
  Interface    Role  Sts
  Gig0/1       Desg  FWD  ← vers R1
  Gig0/2       Desg  FWD  ← vers SW2
  Fa0/23       Desg  FWD  ← vers SW3
```

### 📊 Résultat attendu — show spanning-tree SW3

```
Interface    Role  Sts
Fa0/23       Root  FWD  ← chemin vers Root Bridge
Fa0/24       Altn  BLK  ← bloqué par STP ✅
```

---

## 🧪 Tests de connectivité

```
✅ PC1  → ping 192.168.10.1   (passerelle VLAN10)
✅ PC1  → ping 192.168.20.11  (VLAN10 → VLAN20)
✅ PC1  → ping 192.168.30.11  (VLAN10 → VLAN30 via SW3)
✅ PC1  → ping 192.168.40.11  (VLAN10 → VLAN40 via SW3)
✅ PC7  → ping 192.168.10.11  (VLAN30 → VLAN10)
✅ PC10 → ping 192.168.20.11  (VLAN40 → VLAN20)
```

---

## 🎭 Scénario — Test de redondance STP

### Simuler panne lien SW1 → SW3

```cisco
SW1(config)# interface fa0/23
SW1(config-if)# shutdown
```

### Observer STP en action

```cisco
SW3# show spanning-tree
→ Fa0/24 passe de BLK à FWD ✅
→ Trafic redirigé via SW3 → SW2 → SW1
```

### Retester

```
PC7 → ping 192.168.10.11 ✅
→ Fonctionne encore !
```

### Réactiver

```cisco
SW1(config)# interface fa0/23
SW1(config-if)# no shutdown
```

---

## 💡 Points clés à retenir

| 🔑 Commande | 📖 Rôle |
|-------------|---------|
| `encapsulation dot1Q X` | Associe sous-interface au VLAN |
| `no shutdown` sur Gig0/0 | Active l'interface physique |
| `spanning-tree vlan X root primary` | Élit le Root Bridge |
| `spanning-tree portfast` | Activation immédiate ports PC |
| `ip dhcp excluded-address` | Réserve des IPs pour la passerelle |
| `ip dhcp pool` | Crée le pool DHCP par VLAN |
| `default-router` | Passerelle donnée aux PC |
| `show ip dhcp binding` | Vérifie les IPs distribuées |

---

## 🛠️ Outils

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-8.x-orange?style=flat-square&logo=cisco)
![Cisco IOS](https://img.shields.io/badge/Cisco%20IOS-15.x-blue?style=flat-square)
![GitHub](https://img.shields.io/badge/GitHub-black?style=flat-square&logo=github)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420?style=flat-square&logo=ubuntu)

---

## 👨‍💻 Auteur

**Urbain Sedami Landjidé**
🎓 Étudiant en 2ème année — Licence Professionnelle
📡 Réseaux Informatique Mobilité Sécurité (RMS)
🏫 Cisco Networking Academy
📍 Cotonou, Bénin 🇧🇯

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connecter-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/urbain-sedami-landjide-9b49043a8/)

---

## 📄 Licence

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Libre d'utilisation pour l'apprentissage et la formation réseau.
