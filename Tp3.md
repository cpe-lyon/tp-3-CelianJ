# TP 2

## Exercice 1

#### 1
On ajoute les groupes avec la commande `sudo groupadd infra`  et de même avec `dev`.
    
#### 2
    
On utilise le paramètre `-m` pour créer le dossier personnel de l'utilisateur et `-s` pour spécifier un shell.
Ainsi on obtient les commandes a taper : `sudo useradd -m -s /bin/bash [user]`

#### 3 
    
On ajoute les utilisateurs à leurs groupes respectifs avec `sudo usermod -a -G [groupe] [user]`.
    
#### 4

Une première méthode pour afficher les utilisateurs d'un groupe est de faire `grep 'infra' /etc/group`. En effet le fichier `/etc/group` contient la liste de tous les groupes et ses membres. 
    
Une seconde commande permettant de faire la même chose est `getent group infra` ce qui permet également d'afficher les membres du groupe 'infra'. 
    
#### 5

Pour changer la propriété des `$home/[user]` ajoutés, on fait `sudo chgrp [groupe] [dossier home]` dans le dossier `$home`. 
    
#### 6

Pour changer le groupe primaire d'un utilisateur, on fait `sudo usermod -g [Groupe primaire] [User]`, donc par exemple pour Alice on fait `sudo usermod -g dev alice`.

#### 7
    
Avec la commande `sudo mkdir`, on ajoute les répertoires `/home/infra` et `/home/dev`.
    
Et on ajoute les permissions aux groupes respectifs avec `sudo chgrp dev dev` et `sudo chgrp infra infra` .

#### 8

Afin de limiter le renommage et la suppression des fichiers a leurs propriétaires il faut activer le sticky bit. Ce qui donne : `chmod +t /home/infra` et `chmod +t /home/dev`

#### 9
    
Il est impossible de se connecter au compte Alice avec la commande `su alice`.

#### 10
    
Il faut d'abord mettre un mot de passe sur ce compte avec `sudo passwd alice`, pour que son compte soit activé. Ensuite la commande `su alice`fonctionne.

#### 11    

C'est avec les commandes `id -u alice` on obtient l'uid et  `id -g alice` on obtient le gid. 
Les deux id sont `1002`.

#### 12
    
Avec la commande `id -nu 1003`, on trouve à qui appartient l'uid 1003 soit Bob.

#### 13

L'id du groupe dev est 1002 on peut le retrouver en faisant `getent group dev`

#### 14

Pour avoir le nom d'une groupe en fonction d'un gid, on fait `getent group [gid]`.  Ainsi `getent group 1002` renvoi dev

#### 15
Pour supprimer un membre d'un groupe, on exécute la commande `sudo gpasswd -d charlie infra`. Même si la commande est bien exécuté Charlie n'est pas exclu du répertoire  `infra` en effet infra étant son unique groupe le système ne peux pas l'en virer. 

#### 16

On utilise la commande `chage dave` pour éditer les paramètres d'expiration du mot de passe de Dave.

<img
src="https://cdn.discordapp.com/attachments/750759699942735884/1021349032095916082/unknown.png">

#### 17

Si oon utilise la commande `sudo cat /etc/user`, on peut voir que l'utilisateur root utilise `/bin/bash`. 

#### 18

L'utilisateur `nobody` (qui est dans le groupe `nobody`), possède le minimum de permissions possible.

#### 19

La commande `sudo` garde en mémoire le mot de passe pendant 15 min, pour forcer la demande de mot de passe on utilise `sudo -k`.

## Exercice 2 

#### 1

Les droits du dossier test sont : `drwxr-xr-x` Les droits du fichier fichier sont : `-rw-r--r--` Cependant, le dossier test appartenant à l'utilisateur root, le propriétaire a en réalité tous les droits sur tout en tant que root. 

#### 2

Comme attendu, même en retirant tous les droits via la commande : `sudo chmod 000 [fichier]`, l'utilisateur root permet malgré tout de modifier et d'afficher le fichier. 

#### 3

On commence par rendre les droits au fichier via la commande : `sudo chmod 644 [fichier]`. Les droits de lecture et d'écriture sont configurés sur le fichier. Par conséquent l'ajout de contenu dans le fichier fonctionne bien. 

#### 4

Le droit d'exécution sur le fichier n'étant pas activé, l'exécution ne fonctionne pas sans `sudo`. Avec la commande `sudo` elle fonctionne bien étant donné que root a tous les droits. 

#### 5

Retirer le droit de lecture d'un fichier empêche d'en lire le contenu. Apres avoir utilisé la commande : `chmod 355 test`, il est impossible d'afficher le contenu du dossier via la commande `ls`. Pour rétablir les droits du dossier, j'utilise la commande : `chmod 755 test`. 

#### 6

La modification ou la suppression d'un fichier dépend des droits du fichier lui même et non des droits du répertoire dans lequel se trouve ce fichier.
 
#### 7

Apres suppression du droit d'exécution d'un répertoire, il est impossible d'entrer dans ce dernier ou d'y faire quoi que ce soit. le droit d'exécution d'un répertoire permet de faire toute les actions qui lui sont lié. 

#### 8

Apres avoir coupé le droit d'exécution d'un dossier en étant dedans, on ne peut rien y faire y compris naviguer dans les fichiers qui sont dedans. On peut cependant revenir dans le dossier parent. On en conclut donc que la navigation via `cd` dépend des droits du dossier dans lequel on veut naviguer. 

#### 9

Pour qu'un autre utilisateur de mon groupe puisse lire le fichier _fichier_ mais pas le modifier, on utilise la commande : `chmod 644 fichier` 

#### 10

Le umask par défaut est le umask 002. Cependant ce umask est trop permissif car il donne au groupe des droits d'écriture et a tout le monde la traversée de mes répertoires. Un umask plus adapté serait le umask 033 qui ne donne qu'a nous les droits d'écriture et de traverser nos répertoires. 

#### 11 

Le umask le plus adapté pour que tout le monde puisse traverser nos répertoires mais pour que nous soyons les seuls a pouvoir modifier nos fichiers est le umask 022. 

#### 12

Le umask le plus adapté afin que les membres du groupe ai un accès en lecture mais que d'autres utilisateurs n'ai aucun accès est le umask 027. 

#### 13

`chmod u = rx, g=wx,o=r fic` = `chmod 534 fic` `chmod uo+w,g-rx fic` en sachant que les droits initiaux de fic sont `r--r-x---` = `chmod 602 fic` `chmod 653 fic` en sachant que les droits initiaux de fic sont `711` = `chmod u-x,g+r,o+w fic` `chmod u+x,g=w,o-r fic` en sachant que les droits initiaux de fic sont `r--r-x---` = `chmod 520 fic` 

#### 14

Le fichier passwd a les droits `-rw-r--r--` . Il est lisible par tout le monde mais modifiable uniquement par le propriétaire d'où le fait que le programme puisse le modifier.
