+++
title = "Installer wordpress sur une machine Linux"
author = "Pupin Mathéo"
date = "2023-01-10"
+++

### 1ere étape: instalation de WordPress.

Mettez à jour votre machine:

```
apt-get update && apt-get upgrade
```

Positionnez-vous dans le dossier "/tmp" et téléchargez la dernière version de WordPress :

```
cd /tmp
wget https://wordpress.org/latest.zip
```

### 2ieme étape: création de la base de donnée

*Dans ce tuto, j'utilise MariaDB, mais vous pouvez aussi le faire avec MySQL, les commandes sont les mêmes.

Connectez-vous à la console de MariaDB:

```
sudo mariadb -u root -p
```

Nous allons maintenant créer la base de données du site web:

```
CREATE DATABASE toto_com;
```

Vous pouvez faire la comande `SHOW DATABASES;` afin de vérifier l'existence de la base de donnée.

Nous allons ensuite créer un utilisateur qui sera administrateur de cette base de donnée Wordrpress. Il sera nommé `admin` et son mot de passe sera `toto`, _mais vous mettrez évidemment des valeurs beaucoup plus sécurisées_ :

```
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'toto';
```

On doit ensuite donner tous les droits à cet utilisateur:

```
GRANT ALL PRIVILEGES ON tuto_com.* TO admin@localhost;
```

Puis on active les nouveaux privilèges:

```
FLUSH PRIVILEGES;
```

Notre base de donnée est maintenant prête. Vous pouvez quitter la console avec `exit` .

Nous allons utiliser le site par défaut d'Apache, qui a pour racine "_/var/www/html_" afin de stocker les données de notre site WordPress. Au préalable, on supprime la page d'index crée par défaut par Apache :

```
sudo rm /var/www/html/index.html
```

Installer le paquet _zip_, afin de décompresser l'archive de WordPress:

```
sudo apt-get install zip
```

Vous pouvez ensuite dézipper l'archive:

```
sudo unzip latest.zip -d /var/www/html
```

Le problème, c'est que là on vient de décompresser le contenu de l'archive ZIP dans un dossier nommé "_wordpress_", ce qui donne : _/var/www/html/wordpress_. Du coup, pour accéder à notre site, il faudra faire : "http://domaine.fr/wordpress/". Ce n'est pas top, nous allons corriger cela dès maintenant.

Déplacez-vous dans le répertoire `html` .

```
cd /var/www/html
```

Puis transférez tout le contenus du dossier `wordpress` à la racine du site:

```
sudo mv wordpress/* /var/www/html/
```

Le dossier `worpress` nous est maintenant inutile. Supprimons-le:

```
sudo rm wordpress/ -Rf
```

Puis on donne les droits à Apache sur notre dossier:

```
sudo chown -R www-data:www-data /var/www/html/
```

Pour être sur que les droits sont correctement paramétrés:

```
sudo find /var/www/html/ -type f -exec chmod 644 {} \;
```

et

```
sudo find /var/www/html/ -type d -exec chmod 755 {} \;
```

Vous pouvez alors mettre `http://<ip-de-votre-serveur>` dans un navigateur et commencer à paramétrer WordPress!