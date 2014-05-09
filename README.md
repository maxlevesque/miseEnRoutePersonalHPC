Installation d'une machine Personal HPC
========================================

Description étape par étape des choses à faire pour installer un Personal HPC

Par défaut, nous installons et recommandons Ubuntu Server 14.04 Long Term Support (LTS).

#### Créer un utilisateur *igor* et son répertoire personnel /home/igor
```
sudo adduser igor
```
#### Installer vim
```
sudo apt-get install vim
```
#### Changer vim comme éditeur par défaut de visudo
1/ Ouvrir visudo
```
sudo visudo
```
2/ Ajouter la ligne suivante
```
Defaults        editor=/usr/bin/vim
```
#### Ajouter un sudoer
Donner à l'utilisateur `igor` les droits sudo, i.e., lui permettre de faire `sudo <cmd>`.
Executer la commande `visudo` et rajouter la ligne :
```
igor  ALL=(ALL) ALL
```
#### Mettre le système à jour
```
sudo apt-get update && sudo apt-get upgrade
```


#### Installer les compilateurs GCC et Gfortran 4.8
TOUT CE PARAGRAPHE EST INUTILE SOUS UBUNTU 14.04LTS. IL EST VALABLE POUR 12.04LTS uniquement :

Dans Ubuntu Server 12.04 LTS, gcc est en version 4.6. La version 4.8 est nécessaire pour nombre de nos softs.
```
sudo apt-get update
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.8
sudo apt-get install gfortran-4.8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-4.8 50
```

#### Générer une clé ssh
Chaque utilisateur doit générer une clé ssh:
```
cd
ssh-keygen -t dsa
```

#### Transférer sa clé ssh au serveur
Remplacer MONLOGIN et SSHSERVER :
```
cat .ssh/id_dsa.pub | ssh MONLOGIN@SSHSERVER "cat - >>.ssh/authorized_keys"
```

Il est parfois nécessaire de créer un dossier ~/.ssh par `mkdir ~/.ssh`.

#### Interdir le login ssh par root
Dans ubuntu server, dans `/etc/ssh/sshd_config`, il faut mettre `no` à l'option:
```
PermitRootLogin no
```


#### Simplifier le ssh banner, c'est à dire la bannière de connection

Avec ubuntu server, à chaque login ssh est affiché tout un tas d'information de ce type :
```
Welcome to Ubuntu 12.10 (GNU/Linux 3.5.0-18-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sat Nov 23 17:11:25 CET 2013

  System load:  0.08              Processes:           399
  Usage of /:   40.4% of 2.44TB   Users logged in:     1
  Memory usage: 0%                IP address for eth0: 134.157.169.155
  Swap usage:   0%

  Graph this data and manage this system at https://landscape.canonical.com/

New release '13.04' available.
Run 'do-release-upgrade' to upgrade to it.

Last login: Sat Nov 23 17:02:17 2013 from ptah.li2c.jussieu.fr

```
Pour qu'il n'y ait plus que la dernière ligne affichée, "last login [...]", il faut éditer le fichier `/etc/pam.d/sshd` et commenter (avec #) les deux lignes suivantes :
```
session    optional     pam_motd.so  motd=/run/motd.dynamic noupdate
session    optional     pam_motd.so # [1]
```


#### Installer et configurer Denyhosts

Denyhosts restreint les tentatives de connexion "louches": multiples tentatives de mot-de-passes, logins, etc.

Pour installer denyhosts:
```
sudo apt-get install denyhosts
```
Denyhosts est un démon qui tourne automatiquement en tache de fond et se relance tout seul au démarrage.
La configuration par défaut est bien, mais cela vaut le coup de paufiner (au coup par coup, il suffit de lire le fichier) :
```
sudo vi /etc/denyhosts.conf
```

Pour vérifier que le démon denyhosts tourne bien en tache de fond :
```
sudo service denyhosts status
```

#### Installer SGE, le gestionnaire de queue/jobs
```
sudo apt-get install gridengine-* xfonts-*
```
- Automatic configuration => *Yes*
- SGE cell name => *default*
- SGE master hostname => *hostname*

- Set fonts to traditional => *No*, *No*





