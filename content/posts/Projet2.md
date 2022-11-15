+++
title = "HAProxy"
author = "Pupin Mathéo"
date = "2022-11-14"
+++

Ici, le HAProxy sera la porte d'entrée pour le reséau public vers le réseau privée (les 2 serveurs webs)

Chacun des 2 serveurs auront une page différentes

Le HAProxy disposera d'un équilibrage de charge entre les deux serveurs webs

Ainsi qu'une connexion en HTTPS grâce à un certificat qui nous appartient

    frontend http-in
        bind 172.25.40.2:80 --> déclarer l'IP sur laquelle le HAProxy va écouter, c'est-a-dire, définir la porte d'entrée
        mode http
        option httplog
        acl pupin1 hdr(host) www.pupintp.tld
        acl pupin2 hdr(host) www.pupintp.tld
        redirect scheme https code 301 if !{ ssl_fc } pupin1 or pupin2 --> ici on redirige la connexion HTTP en HTTPS 
        use_backend backend1 if pupin1 or pupin2

    backend backend1
        mode http
        option httpchk
        option forwardfor except 127.0.0.1
        http-request add-header X-Forwarded-Proto https if { ssl_fc }
        balance roundrobin --> selectionne les serveurs à tour de rôle
        server web-server1 10.0.0.101:80 maxconn 32  --> on déclare ici les serveurs
        server web-server2 10.0.0.102:80 maxconn 32

    frontend lehttps
        bind 172.25.40.2:443 ssl crt /etc/haproxy/cert/ no-sslv3 --> on déclare le chemin ou notre certificat est pour permettre l'HTTPS
        mode http
        option httplog
        acl pupin1 hdr(host) www.pupintp.tld
        acl pupin2 hdr(host) www.pupintp.tld
        use_backend backend1 if pupin1 or pupin2
