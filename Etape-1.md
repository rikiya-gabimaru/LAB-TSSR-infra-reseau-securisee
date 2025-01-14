## 1 - Sécurisation des équipements d'interconnexions

<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  
Nous utiliserons un switch pour cet exemple et nous essaierons de réaliser les étapes dans un ordre chronologique logique.

**1 - Nommer l'équipement :**  
`Switch> enable`  
`Switch# configure terminal`  
`Switch(config)# hostname newNameSwitch`

**2 - Attribuer le domaine à l'équipement :**  
**Obligatoire pour créer une clé RSA nécessaire pour utiliser le SSH qu'on implémentera lus tard.**  
`Switch(config)# ip domain-name nonDedomaine.local`

**3 - Créer un VLAN aléatoire pour sortir toutes les interfaces du vlan par défaut :**  
`Switch(config)# vlan 3000`  
`Switch(config-if)# exit`

> Si tu ne parviens pas à créer le vlan 3000 c'est probablement dû au mode "Server" du protocole VTP. Tu peux soit modifier le mode VTP à "Transparent" avec ce que ça implique soit continuer sans tenir compte de l'étape 3 qui ne sera pas bloquante pour la suite.

**4 - Eteindre toutes les interfaces de l'équipement :**  
**Profitons-en avant de commencer la configuration des interfaces pour n'oublier aucune interface.**  
`Switch(config)# interface range FastEthernet 0/1-24`  
`Switch(config-if-range)# switchport mode access`  
`Switch(config-if-range)# switchport access vlan 3000`  
`Switch(config-if-range)# shutdown`  
`Switch(config-if-range)# exit`  
`Switch(config)# interface range GigabitEthernet 0/1-2`  
`Switch(config-if-range)# switchport mode access`  
`Switch(config-if-range)# switchport access vlan 3000`  
`Switch(config-if-range)# shutdown`  
`Switch(config-if-range)# exit`  

**5 - Créer un VLAN d'administration pour accéder depuis le réseau à l'équipement :**  
`Switch(config)# vlan 100`  
`Switch(config-vlan)# name NomVlan100`  
`Switch(config-vlan)# exit`  
`Switch(config)# interface vlan 100`  
`Switch(config-if)# ip address 10.100.100.252 255.255.255.248`  
`Switch(config-if)# no shutdown`  
`Switch(config-if)# exit`  
`Switch(config)# ip default-gateway 10.100.100.254`  

**6 - Configurer un mdp sur le "Mode Privilégié" (Privileged EXEC Mode) :**  
`Switch(config)# enable algorithm-type sha-256 secret motDePasse`  
> Si sha-256 non implémenté dans l'équipement alors commande suivante :

`Switch(config)# enable secret motDePasse`  

**7 - Implémentation du SSH :**  
`Switch(config)# username nonUser privilege 15 algorithm-type sha-256 secret motDePasse`  
> Si sha-256 non implémenté dans l'équipement alors commande suivante :

`Switch(config)# username nonUser secret motDePasse`  
`Switch(config)# ip ssh version 2`  
`Switch(config)# crypto key generate rsa`  
`Switch(config)# 2048`  
`Switch(config)# ip ssh time-out 120`  
`Switch(config)# ip ssh authentication-retries 3`  

**8 - Sécurisation des connexions :**  
`Switch(config)# line console 0`  
`Switch(config-line)# login local`  
`Switch(config-line)# exec-timeout 5`  
`Switch(config-line)# exit`  
`Switch(config)# line vty 0 15`  
`Switch(config-line)# login local`  
`Switch(config-line)# exec-timeout 5`  
`Switch(config-line)# transport input ssh`  
`Switch(config-line)# transport output none`  
`Switch(config-line)# end`  

**9 - Sauvegarde de la configuration :**  
`Switch# copy running-config startup-config`  

<a href="README.md">(Retourner au sommaire)</a>
