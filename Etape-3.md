## 3 - Techniques de routage

<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  
Nous allons voir plusieurs méthodes de routage en fonction des équipements disposinibles.

**1 - Router on a Stick :**  

> Cette méthode consiste à subdiviser l'interface physique d'un routeur en plusieurs sous interfaces logiques. Cette méthode peut être utilisée quand l'équipement à disposition ne possède pas suffisement d'interfaces physiques pour servir de passerelle par défaut à tous les VLANs de l'infrastructure.

`Router> enable`  
`Router# configure terminal`  
`Router(config)# interface GigabitEthernet 0/0/0`  
`Router(config-if)# no shutdown`  
`Router(config-if)# exit`  
`Router(config)# interface GigabitEthernet 0/0/0.10`  
`Router(config-subif)# encapsulation dot1Q 10`  
`Router(config-subif)# ip address 10.10.10.254 255.255.255.0`  
`Router(config-subif)# exit`  
`Router(config)# interface GigabitEthernet 0/0/0.20`  
`Router(config-subif)# encapsulation dot1Q 20`  
`Router(config-subif)# ip address 10.20.20.254 255.255.255.0`  
`Router(config-subif)# exit`  
Etc..

 `suite à venir...`
