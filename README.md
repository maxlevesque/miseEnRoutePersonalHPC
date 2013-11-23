miseEnRoutePersonalHPC
======================

Description étape par étape des choses à faire pour installer un Personal HPC




## Configurer SSH

Avec ubuntu server, à chaque login ssh est affiché tout un tas d'information de ce type :
```


```
Pour qu'il n'y ait plus que le "last login" affiché, il faut éditer le fichier `/etc/pam.d/sshd` et commenter les deux lignes suivantes :

session    optional     pam_motd.so  motd=/run/motd.dynamic noupdate
session    optional     pam_motd.so # [1]

