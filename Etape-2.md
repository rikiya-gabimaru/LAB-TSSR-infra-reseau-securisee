## 2 - Configuration des VLANs

<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  
Nous allons procéder à la configuration des VLANS

**1 - Création des VLANs sur les switchs (exemple) :**  
`Switch> enable`  
`Switch# configure terminal`  
`Switch(config)# vlan 10`  
`Switch(config-vlan)# name DIR`  
`Switch(config-vlan)# exit`  
`Switch(config)# vlan 20`  
`Switch(config-vlan)# name FIN`  
`Switch(config-vlan)# exit`  
Etc..

**2 - Création des trunks entre les switchs :**  
`Switch> enable`  
`Switch# configure terminal`  
`Switch(config)# interface GigabitEthernet 0/1`  
`Switch(config-if)# switchport mode trunk`  

> La commande précédente suffit à laisser passer les trames VLANs. On pourrait cependant filter les trames des VLANs avec la commande suivante. Mais on préférera centraliser ce filtrage sur un seul équiement plus tard dans ce lab.

**Exemple :**  
`Switch(config-if)# switchport trunk allowed vlan 10`  

**3 - Basculement des interfaces de switch sur les bons VLANs pour que les hosts soient directement reliés à leur VLANs :**  
`Switch> enable`  
`Switch# configure terminal`  
`Switch(config)# interface FastEthernet 0/1-12`  
`Switch(config-if)# switchport mode access`  
`Switch(config-if)# switchport access vlan 10`  
`Switch(config-vlan)# end`  
Etc..

**4 - Sauvegarde de la configuration :**  
`Switch# copy running-config startup-config`  

<a href="README.md">(Retourner au sommaire)</a>
