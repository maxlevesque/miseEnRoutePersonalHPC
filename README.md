Installation d'une machine Personal HPC
========================================

Description étape par étape des choses à faire pour installer un Personal HPC

Par défaut, nous installons et recommandons Ubuntu Server 14.04 Long Term Support (LTS).

#### Créer un utilisateur nommé *igor* et son répertoire personnel /home/igor
```
sudo adduser igor
```

#### Donner à l'utilisateur nommé *igor* les droits sudo
Donner à l'utilisateur nommé `igor` les droits sudo, c'est à dire lui permettre de faire `sudo <cmd>`.
Executer la commande `visudo` et rajouter la ligne :
```
igor  ALL=(ALL) ALL
```
#### Mettre le système à jour
```
sudo apt-get update && sudo apt-get upgrade
```


#### Installer les compilateurs GCC et Gfortran 4.8
```
sudo apt-get install gcc gfortran
```

#### Interdir le login ssh par root
Dans ubuntu server, dans `/etc/ssh/sshd_config`, il faut mettre `no` à l'option:
```
PermitRootLogin no
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



#### Simplifier le ssh banner, c'est à dire la bannière de connection

Avec ubuntu server, à chaque login ssh est affiché tout un tas d'information de ce type :
```
Welcome to Ubuntu 14.04 (GNU/Linux 3.5.0-18-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sat Nov 23 17:11:25 CET 2013

  System load:  0.08              Processes:           399
  Usage of /:   40.4% of 2.44TB   Users logged in:     1
  Memory usage: 0%                IP address for eth0: 134.157.169.155
  Swap usage:   0%

  Graph this data and manage this system at https://landscape.canonical.com/

New release '13.04' available.
Run 'do-release-upgrade' to upgrade to it.

Last login: Sat Nov 23 17:02:17 2013 from toto.fr

```
Pour qu'il n'y ait plus que la dernière ligne affichée, "last login [...]", il faut éditer le fichier `/etc/pam.d/sshd` et commenter (avec #) les deux lignes suivantes :
```
session    optional     pam_motd.so  motd=/run/motd.dynamic noupdate
session    optional     pam_motd.so # [1]
```



#### Installer SGE, le gestionnaire de queue/jobs
```
sudo apt-get install gridengine-* xfonts-*
```
- ?? (je ne me souviens pas du texte) => *No*
- Automatic configuration => *Yes*
- SGE cell name => *default*
- SGE master hostname => *localhost*
- Set fonts to traditional => *No*, *No*

Pour une configuration basique mais fonctionnelle, le mieux est de suivre ce lien:
http://scidom.wordpress.com/tag/parallel/


# How to create a live USB key

1.  Download a Fedora (or Ubuntu or ...) image, choose a USB stick that does not contain any data you need, and connect it 
2.   Run Nautilus (Files) - for instance, open the Overview by pressing the Start/Super key, and type Files, then hit enter 
3.    Find the downloaded image, right-click on it, go to Open With, and click Disk Image Writer 
4.     Double-check you're really, really sure you don't need any of the data on the USB stick! 
5.      Select your USB stick as the Destination, and click Start Restoring... 
6.       Wait for the operation to complete, then reboot your computer, and do whatever you need to do to boot from a USB stick - often this will involve pressing or holding down F12, F2 or Del. 

source: https://fedoraproject.org/wiki/How_to_create_and_use_Live_USB
