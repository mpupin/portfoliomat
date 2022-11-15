+++
title = "Créer un DNS sous Linux avec BIND9"
author = "Pupin Mathéo"
date = "2022-11-14"
+++
 
 Créer et configurer un serveur DNS sur Linux avec Bind9.

-Installer Bind9 :

sudo apt-get install bind9

Tout d’abord, nous mettons ces 3 lignes dans le fichier named.conf .  

include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";


-Dans le fichier de configuration de bind9 « named.conf.local », déclarer ses zones :

zone "example.com" {
        type master;
        file "/etc/bind/db.example.com";
};


→ On vient ici de dire que la configuration de notre zone example.com se fera dans le fichier « db.example.com »

Dans db.example.com ; configurer sa zone (l’IP de mon serveur DNS est 192.168.1.15 ):

$TTL    604800
@       IN      SOA     example.com. root.example.com. (
                              2         ; Serial

                         604800         ; Refresh

                          86400         ; Retry

                        2419200         ; Expire

                         604800 )       ; Negative Cache TTL

        IN      A       192.168.1.15

@       IN      NS      ns.example.com.

ns      IN      A         192.168.1.15

serv1   IN      A       192.168.1.1

serv2   IN      A       192.168.1.2

Le premier paragraphe correspond à l’enregistrement SOA, on peut le laisser tel quel.

Dans le deuxième paragraphe, on va donner des informations à notre serveur DNS, c’est-à-dire , que nous allons entrer manuellement des noms de machines et leurs IP utiles à mon serveur.  

⚠ Il est fortement recommandé de faire l’enregistrement NS, c’est la norme.

Premièrement, j’ai donné à mon serveur DNS les IP des autres serveurs DNS du domaine example.com (c’est  l’enregistrement NS). Malgré le fait qu’il n’y en ai qu’un seul (mon serveur DNS), je le met quand même.   

Ensuite, j’ai déclaré 2 serveurs web, serv1 et serv2.
 Je rappelle qu’il faudra adapter cette configuration en fonction de ces besoins, cette configuration n’est qu’un exemple.

Si notre serveur DNS n’est pas capable de résoudre certains nom de domaine, il faut qu’il envoie ses requêtes à d’autres DNS qui pourront le faire. Ces serveurs s’appellent des « forwarders ». On doit les indiquer dans le fichier « named.conf.options ». 

    forwarders {
        8.8.8.8;
        172.25.254.15 ;
    };
