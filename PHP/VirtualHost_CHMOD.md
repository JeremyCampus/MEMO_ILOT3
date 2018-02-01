# Création d'un Virtual Host
### Définition :
en informatique le Virtual Host est une méthode que les serveurs tel que les serveur web utilise pour accueillir plus d'un nom de domaine sur le même ordinateur, parfois sur la même adresse IP tout en maintenant une gestion séparée de chacun de ces nom de domaine.
ure de Création du Virtual Host

### Création par étapes :


Pour créer un Virtual host on va taper dans GitBash :

    nano /etc/hosts

on rajoute les ligne :

     AdresseIP monnomdedomaine.fr
     AdresseIp monNmdedomaine2.fr

Puis nous allons créer deux dossiers dans le dossier /var/www qui est par défaut la racine d'apache. Vous pouvez les créer en interface graphique ou en ligne de commande via ces deux commandes :     
	    
	    mkdir /var/www/monsite1
	    mkdir /var/www/monsite2
	    
Ces dossiers faits, nous allons créer les fichiers de configurations pour apache. Dans /etc/apache2/sites-available se trouve un fichier nommé default, ouvrez-le avec votre éditeur de texte favori  puis modifiez de cette façon le fichier :

  

      <VirtualHost *:80>
        	ServerAdmin votre-mail@monsite1.fr
        	ServerName monsite1.fr
        	ServerAlias www.monsite1.fr
    	DocumentRoot /var/www/monsite1
	  </virtualHost>
          
    

si vous utiliser le module apache mod_rewrite, vous devez corriger le fichier `default dans /etc/apache2/sites-available` qui est mentionné ci-dessus, et remplacer toutes les occurences:
remplacer dans le .conf :

    AllowOverride None 
    
par

    AllowOverride All

Pour terminer, il vous suffit de créer des liens des deux fichiers nouvellement créés dans le dossier `/etc/apache2/sites-enabled`. Pour ce faire, une commande a été faite spécialement:
 
     a2ensite monsite1.conf
     a2ensite monsite2.conf

puis afin de recharger la configuration d'apache:

    service apache2 restart


# Mod_Rewrite comment l'activer ?

Premièrement, avec Ubuntu, lorsque l'on veut installer un serveur Web Apache 2, le fichier de configuration n'est pas httpd.conf, mais bien apache2.conf, qui est situé dans le dossier `/etc/apache2/`.

## Première étape :

Le module mod_rewrite est déjà "pré-installé" avec Apache sous Ubuntu. Vous pouvez vérifiez si c'est le cas en exécutant la commande:

    ls -l /usr/lib/apache2/modules/

et si vous voyez le fichier `mod_rewrite.so` c'est que le module est pré-installé

## Deuxième étape : 

Ensuite, la commande suivante crée un lien logique entre ce module et les fichiers de modules que comprend votre serveur Apache (ils sont placés dans le dossier `/etc/apache2/mods-available/`) :

    a2enmod rewrite

## Troisième étape :

Maintenant, ouvrez le fichier apache2.conf :

    nano /etc/apache2/apache2.conf

et rajouter à la fin de ce dernier, le code suivant afin de bien s'assurer que le module sera activer :

    <ifModule mod_rewrite.c>
    RewriteEngine On
    </ifModule>

Finalement, redémarrez votre serveur Apache et le tour et joué !

    service apache2 restart

# Gérer les permission d'accés sur un fichier ou un répertoire :

Un utilisateur a le droit de faire un chmod sur un fichier 
si il est `root` ou s'il est le propriétaire du fichier en question.

Les options passées à la commande chmod sont indiquées comme ceci :
chmod options modes fichiers:

#### légende : 
u = **user**
g = **group**
o = **other**
a = **all**

Pour un fichier : `chmod [u g o a] [+ - =] [r w x] nom_du_fichier`

Pour le contenu d'un répertoire (de façon récursive) : chmod -R [u g o a] [+ - =] [r w x] nom_du_répertoire

## Option du CHMOD

chmod a un certain nombre d'options qui peuvent modifier le résultat. Certaines de ces options sont :

`-c, --changes` : comme verbeux (-v) mais n'affiche que les changements effectués.
`--no-preserve-root` : ne traite pas / (la racine du système de fichiers) spécialement (option par défaut).
`--preserve-root` : échec du traitement récursif (-R) sur / (la racine du système de fichiers).
`-f, --silent, --quiet` : supprime la plupart des messages d'erreur.
`-v, --verbose` : mode verbeux. Affiche la liste de tous les fichiers en cours de modification.
`-R, --recursive` : change les modes de tous les fichiers dans les sous-répertoires de manière récursive.
`--help` : affiche l'aide de la commande chmod.
`--version` : affiche les informations sur la version de chmod.

Les modes peuvent être spécifiés de deux façons, avec des lettres ou avec des nombres en octal. Pour les lettres, il existe les opérateurs de changement d'état + et - pour ajouter ou retirer un type de droit aux droits courants, et l'opérateur = pour les écraser. Pour l'octal, il faut additionner les nombres pour chaque type de possesseur.

Les permissions sont (valeurs octales entre parenthèses) :

r (4) = autorisation de lecture
w (2) :  autorisation d'écriture
x (1) :  autorisation d'exécution. La permission d'exécution régit également l'accès à un répertoire : si 			    l'exécution n'est pas autorisée sur un répertoire, on ne peut faire un chdir (commande cd) sur ce          		    répertoire.

### Exemple :

`chmod u+rw mon_fichier` (donne au propriétaire les droits en écriture et en lecture au fichier mon_fichier)

`chmod 755 mon_dossier` (donne au propriétaire tous les droits, aux membres du groupe et aux autres les droits de ``lecture et d'accès``. C'est un droit utilisé traditionnellement sur les répertoires).







    
         
    
        
    
        
    
    

