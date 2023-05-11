+++
title = "Créer un serveur DHCP sous Linux"
author = "Pupin Mathéo"
date = "2023-01-11"
+++

### Installer le paquet
apt install isc-dhcp-server

### Ouvrir le fichier de configuration
nano /etc/dhcp/dhcpd.conf

### Ajout de l'attributaion

subnet 172.16.0.0 netmask 255.255.0.0 {

### rang d'attribution (100 machines)

range 172.16.100.101 172.16.100.200;

### masque de sous-réseau

option subnet-mask 255.255.0.0;

### broadcast pour les informations générales

option broadcast-address 172.16.100.255;

### adresse ip du routeur

option routers 172.16.100.254;

### Serveur DNS

option domain-name-servers 172.25.254.12, 172.25.254.15;
}


### Ajouter le subnet de notre serveur sinon il affichera une erreur

subnet 10.0.100.0 netmask 255.255.255.0 {

option subnet-mask 255.255.255.0;

option routers 10.0.100.254;
}

nano /etc/default/isc/default/isc-dhcp-server

INTERFACESv4="eth0"