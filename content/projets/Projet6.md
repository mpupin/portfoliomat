+++
title = "Créer un serveur NTP sous Linux"
author = "Pupin Mathéo"
date = "2023-01-13"
+++
Installer un serveur NTP
### Sur une machine Debian 10 installer un serveur NTP
### Installer le package chrony
```
sudo -s
apt install chrony
```
### A partir de ce moment la on doit vérifier le contenu de "chrony.conf"
```
nano /etc/chrony/chrony.conf
```
### Ensuite nous devons déclarer nos pool NTP donc nous allons nous rendre à la ligne 3 pour faire ajouter ces modification :
```
pool 2.debian.pool.ntp.org iburst
# devient :

#pool 2.debian.pool.ntp.org iburst"
```
**et maintenant nous pouvons ajouter nos pool en entrant les lignes suivantes :**
```
server 0.fr.pool.ntp.org
server 1.fr.pool.ntp.org
server 2.fr.pool.ntp.org
server 3.fr.pool.ntp.org
```
### Une fois cela fait on va pouvoir se rendre un peut plus bas pour 
vérifier l'existance (ou non) des lignes suivantes:
```
# This directive specify the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift
local stratum 8
manual
allow all
```
**Si ces lignes ne sont pas présente il faudra les rajoutés.**

Le "local stratum 8" déclare la "couche" de notre serveur NTP. Pour terminer cette installation il faut lancer le serveur.
### Démarrer NTP 
```
systemctl start chrony
```
### On vérifie si tout fonctionne correctement :
```
systemctl status chrony
```
