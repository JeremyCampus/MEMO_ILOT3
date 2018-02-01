# 1 - Installation du serveur
### installer Debian
Ramener les paquets :

    apt-get update
Installer Debian 9 :

    apt-get upgrade
---
### Installer Apache2

    apt-get install apache2
Lancer Apache :

    service apache2 start
---
### Installer PHP

    apt-get install php
Vérifier la version de php :

    php -v
Relancer Apache pour prendre en compte php 

    service apache2 restart
L'index par défaut se trouve dans :

    cd /var/www/html/
On peut le modifier pour tester :

    nano index.html
Et créer une page test dans laquelle on met les balises et un texte :

    nano toto.php
On peut se connecter à la page dans le browser, exemple :

    http://15.51.214.161/toto.php
---
### Intaller MySQL (MariaBD dans Debian 9)

    apt-get install mysql
    // ou éventuellement
    apt-get install mariadb-common mariadb-server mariadb-client
   Lancer le serveur MySQL :
   

    service mysql start
    mysql
---
### Dans le SGBDR (MariaDB / MySQL):
Vérifier la connexion en affichant les tables :
    show databases ;
Créer un nouvel utilisateur et lui donner tous les droits :

    CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
    GRANT AL PRIVILEGES ON *.* TO 'user'@'localhost';

**!! Apache, Php et MySQL étant sur la même machine, l'utilisateur se connecte en 'localhost' !!**

---
### Installer PhpMyAdmin

    apt-get install phpmyadmin
Suivre les instructions (>> 'yes' puis renseigner user et password )
Relancer Appache si besoin :

    service apache2 restart

# 2 - Cloner son projet Laravel 

    cd /var/www/user
    git clone https://github.com/monprojet.git
   
# 3 - Créer un VirtualHost
   ### Configurer le DNS sur son registrar (Gandi, ovh...)
   Créer le sous domaine directement chez l'hébergeur (=registrar)
   
   ---
   ### Créer le dossier cible
   

    mkdir /var/www/user

---
   ### Créer et mettre à jour le fichier user.conf
   
Aller dans le dossier sites-available et dupliquer le fichier .conf par défaut :

    cd /etc/apache2/sites-available/
    cp 000-default.conf user.conf
    nano user.conf

Dans user.conf, renseigner le *DocumentRoot* et le *ServerName*:

    <VirtualHost *:80>
	    ServerName user.monsite.com
	    DocumentRoot /var/www/user/public
    </VirtualHost>
Et autoriser l'override :

    <Directory /var/www/user>
	    Allowoverride All
    </Directory>


**!! Dans notre cas, le DocumentRoot est le dossier *public* du projet Laravel ( dans lequel se trouve *index.php*) !!**

---
### Activer le VirtualHost

    a2ensite user.conf
---
### Donner les droits d'accès 
Pour pouvoir utiliser Laravel, il faut redefinir les droits d'accès au dossier *storage/*:

    chmod -R a+w storage/
Le -R signifie 'récursif', c'est à dire que la modification concernera tous les dossiers enfants.

---
### Activer le mode Rewrite
Pour que les routes Laravel fonctionnent, il faut activer le mode Rewrite.

    a2enmod rewrite
Et passer le *RewriteEngine* de *off* à *on* dans le fichier *.htaccess* qui se situe dans le dossier *public/* de mon projet Laravel :

    nano /var/www/user/Laravel/public/.htaccess

    >> RewriteEngine On

# 4 - MàJ de la config Laravel sur le serveur
### Créer le .env à partir du .env.example 
A la racine du projet Laravel 

    cp .env.example .env
    nano .env
Renseigner le nom de la DB ainsi que le user et le password.
### Générer la clé de sécurité
La commande suivante crée une clé dans le .env pour protéger l'envoi de formulaire

    php artisan key:generate

