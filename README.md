# Itescia ANSIBLE-INFRA
> Le projet est de déployer une infrastructure via ansible avec l'installation de deux serveurs web qui hébergerons un glpi et de deux serveurs mariadb qui stockerons la base de donnée de glpi et d'un serveur ldap qui sera utiliser pour s'authentifier dans glpi. L'infrastructure dispose aussi d'un hearbeat permettant de faire fonctionner l'infrastructure en HA via une IP virtuelle rendant ainsi hautement disponible le serveur WEB GLPI.

## Prérequis d'installation :

> Installation des clées ssh de l'utilisateur courant de ansible sur toutes les machines pour pouvoir exécuter les playbooks

```sh
ssh-copy-id user@192.168.20.XX
```

> Création des clées ssh pour les bases de données à effectuer sur les deux serveurs de BDD

```sh
ssh-keygen
```

> Exportation de la clé ssh de SRV-DB1 vers SRV-DB2

```sh
ssh-copy-id user@192.168.20.24
```

> Exportation de la clé ssh de SRV-DB2 vers SRV-DB1

```sh
ssh-copy-id user@192.168.20.23
```

> Configuration du fichier hosts de ansible

Il faudra faire correspondre vos noms d'hôtes dans le fichier hosts d'ansible

> Adressage IP de l'infrastructure

SRV-WEB1=192.168.20.21/24
SRV-WEB2=192.168.20.22/24
SRV-DB1=192.168.20.23/24
SRV-DB2=192.168.20.24/24
SRV-LDAP-1=192.168.20.25/24
 

## Installation des services via ansible-playbook

Linux :

> Installation & configuration des systèmes (iptables, update)

```sh
ansible-playbook 1_system.yml -i hosts --ask-become-pass
```
> Installation et configuration mariadb

```sh
ansible-playbook 2_mariadb_glpi_database.yml -i hosts --ask-become-pass
```

> Configuration de la réplication

```sh
ansible-playbook 3_replication_database.yml -i hosts --ask-become-pass
```

> Installation de GLPI

```sh
ansible-playbook 4_glpi_installation.yml -i hosts --ask-become-pass
```


> Installation de HEARBEAT pour glpi avec configuration de l'ip virtuelle 192.168.20.50

```sh
ansible-playbook 5_hearbeat_glpi.yml -i hosts --ask-become-pass
```

> Installation & configuration du serveur LDAP

```sh
ansible-playbook 6_ldap.yml -i hosts --ask-become-pass
```

## Exemple d'usages avec glpi

Le site est disponible en http sur le port 80 : http://192.168.20.50 (lien de GLPI via l'ip virtuelle Hearbeat)
* Login: glpi
* Password: glpi

## Pour tester glpi avec un utilisateur ldap il faut d'abord ajouter le ldap dans glpi --> Onglet Authentification

* Nom : ldap-ansible-infra
* Serveur par défaut : OUI
* Actif : OUI
* Serveur : 192.168.20.25
* BaseDN : dc=ansible-infra,dc=local

## On peux maintenant s'authentifier via le ldap sur GLPI

* Login: grp1
* Password: grp1

## Voir la base GLPI

> Vous pouvez vérifier la présence de la base glpi et de sa réplication.

* Username: root
* password: grp1
* server: 192.168.20.23 et 192.168.20.24
* Database: glpidb

## Contributeurs
* M3-Res-M1 Itescia

* Maxime.C
* Alexis.O
* Sébastien.M
* Nicolas.D

