# FACULTE DE GESTION, ECONOMIE & SCIENCES

MASTER : M1 IPII

Date : du 14/11/2015 au 18/12/2015

Professeur : [Fabien Meurillon `<fabien.meurillon@smile.fr>`](mailto:fabien.meurillon@smile.fr)

# Projet

## J2EE 1

[TOC]

### Introduction

Ce document constitue l'énoncé du projet de fin de module J2EE. Il porte sur la réalisation en binômes d'une application de type "url shortener".

Les livrables du projet devront être transmis avant le vendredi 18 décembre sur l'adresse mail [Fabien Meurillon `<fabien.meurillon@smile.fr>`](mailto:fabien.meurillon@smile.fr).

La totalité des livrables devra être compressée dans une unique archive (`zip` ou `tar.gz`) nommée avec le nom des deux étudiants impliqués.
Il pourra être téléchargeable depuis un service cloud (Dropbox, Google Drive, Box.net...) ou depuis tout autre URL accessible sur Internet.

### Spécifications générales

#### Vue d'ensemble

Le but de ce projet est de réaliser une application permettant des raccourcir les URL dans le but de les partager sur les réseaux sociaux.

Des exemples de ce type de service peuvent être trouvés sur Internet :

* http://tinyurl.com/
* https://goo.gl/
* https://bitly.com/

#### Gestion des utilisateurs

Une gestion basique des utilisateurs sera mise en place.
Les utilisateurs peuvent se connecter au site avec leur compte, ou créer un compte si ils n'en ont pas encore.

Un utilisateur est constitué au minimum :

* d'un identifiant interne
* d'un *login*
* d'un *mot de passe*

#### Gestion des redirections

À l'inverse des principaux services concurrents, ce raccourcisseur d'URL sera un service privé : seuls les utilisateurs connectés pourront créer des redirections.

Lorsqu'il se connecte, l'utilisateur peut :

* créer une nouvelle redirection en saisissant une nouvelle adresse
* lister les redirections qu'il a déjà créé

Une redirection est constituée au minimum :

* un identifiant interne
* une URL "de base"
* une chaîne de caratères unique, qui servira à générer l'URL raccourcie

De préférence, la chaîne de caractères unique ne devra pas pouvoir être devinée et devrait donc comporter un certain degré d'aléatoire. Afin de garder l'URL la plus courte possible, cette chaîne devra faire un maximum de 4 caractères. Tous les caractères autorisés dans une URL pourront être utilisés.

Soit, d'après la [RFC 1738](https://www.ietf.org/rfc/rfc1738.txt) :

> Thus, only alphanumerics, the special characters "``$-_.+!*'(),``", and reserved characters used for their reserved purposes may be used unencoded within a URL.

Par exemple :

1. l'utilisateur saisi dans le champ de texte l'adresse `http://www.estcequecestbientotleweekend.fr`
1. si il s'agit d'une adresse HTTP valide, l'application lui propose une nouvelle adresse plus courte
1. sinon, l'application lui retourne une erreur

En développement, l'adresse raccourcie pourra prendre la forme `http://localhost:8080/r/ab1c`. La chaîne `r` permet à l'application d'identifier le *Controller* chargé de la redirection, la chaîne `ab1c` est la partie variable, générée par l'application.

#### Accès à une redirection

Les redirections créées pourront être utilisées par tous les utilisateurs, y compris ceux qui ne sont pas connectés.

L'ouverture dans un navigateur de l'adresse citée en exemple `http://localhost:8080/r/ab1c` provoquera une redirection vers la page `http://www.estcequecestbientotleweekend.fr`.

Cette redirection pourra être effectuée :

* de préférence, en utilisant le code retour HTTP `301` assorti de l'*en-tête* `location:`
* sinon, en JavaScript dans le document HTML retourné

L'accès à une redirection inexistante doit provoquer une erreur 404.

#### Charte graphique

Aucune charte graphique n'est imposée. Néanmoins l'application devra être confortable à l'œil et à l'utilisation.

### Contraintes techniques

L'application devra :

* être réalisée uniquement avec les technologies :
  * JavaEE 7 / JDK 8
  * HTML / CSS / Javascript
* respecter les principes de conceptions vus en cours, en particulier :
  * Model - View - Controller
  * Séparation des couches du *Model* (*service*, *domain*, *repository*)

Tous les frameworks MVC *Java* sont autorisés (pas de Scala ou d'autres langages). Sont recommandés Spring MVC et/ou l'utilisation "pure" de JavaEE.
Il est permis d'utiliser un framework CSS pour assurer la cohérence visuelle.
Il est permis d'utiliser un gestionnaire de dépendances (Maven ou Gradle).

Tous les frameworks et composants utilisés seront soumis à l'évaluation. Assurez-vous de bien utiliser ceux que vous choisissez.

### Livrables attendus

L'archive qui constituera le livrable contiendra 3 éléments :

* `sources/` un repository *Git* contenant les sources de l'application
* `doc/` une documentation technique présentant :
  - le fonctionnement global de l'application
  - son architecture (patterns implémentés, frameworks choisis, rôle des packages Java, emplacement des données...)
  - les instructions permettant de générer le package à partir des sources
* `url-shortener.war` le package compilé, au format *war*

### Évaluation

#### Barême

L'évaluation, sur 20 points, se décompose de la façon suivante :

| Critère         | Exemples attendus | Points associés |
|-----------------|-------------------|----------------:|
| Qualité du code | Indentation<br/>Cohérence dans le nommage des variables<br/>Respect des [Conventions de nommage](http://www.oracle.com/technetwork/java/codeconventions-135099.html)<br/>Code en anglais (classes, variables, méthodes)<br/>Code auto-documenté (javadoc) | 6 |
| Qualité de la conception | Pas de code métier dans les contrôleurs ni dans les repositories<br/>Présence des couches *domain*, *service* et *repository*<br/>S.O.L.I.D<br/>Modèle de données, requêtes SQL | 8 |
| Respect du cahier des charges | Les fonctionnalités demandés sont présentes<br/>L'interface graphique est présentable | 6 |

#### Malus

En plus de ce barême certains malus pourront s'appliquer :

| Critère                      | Points associés        |
|------------------------------|-----------------------:|
| Respect des délais           | -4 par jour de retard  |
| Les sources ne compilent pas | -5                     |
| Le *WAR* ne se déploie pas   | -5                     |
| `Exception` envoyée à l'utilisateur  | -2 par `Exception` |
| `Exception` non catchée dans la console (un *log* `ERROR` est acceptable) | -1 par `Exception` |

#### Bonus

Certains bonus pourront également s'appliquer :

| Critère                           | Points associés |
|-----------------------------------|----------------:|
| l'URL courte est aléatoire        | 1               |
| La redirection est une HTTP `301` | 1               |

# FIN DU DOCUMENT