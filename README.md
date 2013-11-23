miseEnRoutePersonalHPC
======================

Description étape par étape des choses à faire pour installer un Personal HPC




## Configurer SSH

### Empêcher un login ssh par l'utilisateur root

Dans ubuntu server, dans `/etc/ssh/sshd_config`, il faut mettre `no` à l'option:
```
PermitRootLogin no
```


### Simplifier le ssh banner, c'est à dire la bannière de connection

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
