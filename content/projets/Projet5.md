+++
title = "Créer un serveur téléphonique sur Linux"
author = "Pupin Mathéo"
date = "2023-01-12"
+++

### Installer le paquet
apt install asterisk

### Configuration de base extensions.conf
```
[general]
static = yes
writeprotect = yes
clearglobalvars = yes
```

### Configuration de base users.conf

```
[general]
hasvoicemail = yes
hassip = yes

[template]
type = friend
host = dynamic
dtmfmode = rfc2833
isallow = all
allow = ulaw
allow = alaw
```

### Configuration de base voicemail.conf

```
[general]
maxmsg = 100
maxsecs = 0
minsecs = 0
maxlogins = 3
review = no
saycid = no
```
### Aller plus loin !

### Possibilité d'appel entre 2 serveurs

### Ajouter dans extensions.conf :
```
[finance]
exten => _1XXX,1,Dial(IAX2/thomas/${EXTEN}) # si le numéro commence par 1 alors rediriger vers le contexte thomas dans iax.conf

[liaison-thomas]
;routage des appels sortants vers le site distant
exten => _2XXX,1,Goto(finance,${EXTEN},1) #permettre que les appels entrent dans le contexte finance du serveur distant
```
### Ajouter dans iax.conf
```
[general]
register => matheo:1234@172.25.117.1 #ce sont les logins qui vont nous permettre de nous connecter au serveur distant

[thomas]
type = friend ;voir explication dans l’activité 1.
host = dynamic ;ip fixe (static) ou dynamique pour le serveur distant.
trunk = yes
secret = 1234
context = liaison-thomas ;contexte associé à la gestion des appels distants.
qualify = yes
```