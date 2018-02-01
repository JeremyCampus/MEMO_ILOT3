# Se connecter au serveur
Dans le Bash : 

    ssh user@51.15.214.161 (adresse IP du server)

# Cloner son projet depuis github
  
    git clone https://github.com/SylvainThibault/PHP-Laravel.git
    (= mon dépôt git)

# Installer les dépendances

    composer install

# Créer et mettre à jour le .env

Dans le dossier Laravel :

    cp .env.example .env
    nano .env
Puis mettre à jour les infos de connexion à la BDD...
# Donner la propriété du dossier storage à Apache
Pour voir les droits et propriétés des dossier et fichiers, dans le dossier Laravel :

    ls -la
    total 940
    drwxr-xr-x 12 root     root       4096 Jan 30 08:23 .
    drwxr-xr-x  5 root     root       4096 Jan 29 15:27 ..
    drwxr-xr-x  6 root     root       4096 Jan 29 15:30 app
    ...
   
On voit ici que c'est le *user* 'root' du groupe 'root' qui est propriétaire de tous les dossiers.
le *d* en début de ligne signifie *directory*.
le *user* a les droits *rwx* (read - write - execute).
le *group* a les droits *r-x*.
les *other* ont les droits *r-x*.

Pour changer le propriétaire d'un dossier (-R signifie récursif, cad que l'ordre va s'appliquer à tous les sous-dossiers ) :
   `chown [-R] user:group directory`

Apache est le *user* 'www-data' du *group* 'www-data' , et on veut appliquer l'ordre à tous les dossiers enfants donc :

    chown -R www-data:www-data storage/

> !! Il faut laisser le droit d’exécution sur les dossiers sans quoi il devient impossible pour les utilisateurs de naviguer dedans !!

  
  

  
