---
tags:
  - note
  - computer-science
year: 2023
---

## Les outils nécessaires

### Symfony : Un projet PHP

Un projet Symfony étant un projet PHP, les éléments suivants sont requis :
- Un serveur web disposant de PHP
- Une base de données
Cependant, l'installateur Symfony peut se substituer au serveur web (sûrement dans des cas basiques de développement).
Afin d'installer tous ces éléments, deux options s'offrent à nous :
- Installer séparément le serveur web, PHP et la base de données
- Installer une plateforme tout-en-un
Globalement, il faut choisir la première option sur un système Linux et la deuxième sur Windows. Les étapes pour l'installation sur Linux du serveur Apache sont détaillées [[Services HTTP|ici]].
Sur Windows, les plateformes tout-en-un recommandées sont :
- [[https://www.wampserver.com/|WampServer]]
- [[https://www.apachefriends.org/fr/index.html|XAMPP]]
Cependant, une dernière méthode passant par un conteneur Docker est à approfondir #go-deeper.

### Symfony CLI

Symfony CLI est un outil permettant la création d'un projet Symfony par défaut. Ce n'est pas un outil obligatoire pour le développement d'une application, cependant, elle fournit des petits plus comme :
- Un serveur web embarqué
- Un utilitaire de vérification des prérequis avant de démarrer un projet
Symfony CLI se lance avec la commande `symfony` depuis un terminal.

### Composer

Composer est un gestionnaire de dépendances pour PHP. Il permet d'installer des librairies tierces mais également de créer des projets PHP en s'appuyant sur des modèles. Composer peut donc servir directement à créer un projet Symfony. Il sera cependant nécessaire même avec l'utilisation de Symfony CLI d'avoir Composer pour gérer les dépendances.
Composer peut s'installer facilement depuis le [[https://getcomposer.org/|site officiel]]. ll se lance ensuite avec la commande `composer` depuis un terminal.

### Les IDE

Avant de se lancer dans un projet, il faut mettre en place un dernier élément de son environnement de travail : l'IDE.
L'IDE recommandé ici est [[https://www.jetbrains.com/phpstorm/|PhpStorm]] avec l'extension Symfony ou [[https://code.visualstudio.com/|VSCode]].

## Création d'un projet Symfony

### Création via Symfony CLI

[[#Symfony CLI|Symfony CLI]] s'utilise en ligne de commande. Avant de créer un nouveau projet, nous pouvons vérifier les prérequis avec la commande :

```bash
symfony check:requirements
```

Le résultat peut afficher des erreurs dans l'installation de PHP ou des optimisations. Tant que le résultat est "OK", nous pouvons continuer.
La création du projet se fait avec la commande :

```bash
symfony new mon_projet --version=lts --webapp
```

Cela permet de générer un projet avec la dernière version LTS de Symfony. Le projet est alors créé dans le répertoire **mon_projet**. En plus de la création du projet, Symfony CLI initialise un dépôt Git.

### Création via Composer

Pour initialiser un projet Symfony avec Composer, nous utilisons la commande `create-project` :

```bash
composer create-project symfony/website-skeleton mon_projet 5.4.*
```

Cela créé le projet avec la version 5.4 la plus récente de Symfony. La création via Composer n'initialise pas de dépôt Git.

### Configurer son serveur web

Après avoir configuré le projet, il ne reste plus qu'à démarrer l'application. Cette dernière nécessite un serveur web.

La manière la plus simple est d'utiliser le serveur web interne à PHP. On peut le lancer avec la commande :

```bash
# Lancement sur le port 8000 par défaut
symfony server:start

# Lancement avec choix du port
symfony server:start --port 8080
```

Le serveur web interne à PHP est avantageux de par sa légèreté et sa rapidité de lancement et d'installation. Cependant, dans le cadre d'application devant exécuter des tâches spécifiques, il est préférable de développer directement sur un serveur similaire à celui utilisé en production.

Avec un serveur web classique (Apache ou Nginx), la configuration du serveur doit pointer sur le répertoire **public** du projet.
La première étape est de créer un domaine local, ainsi pour accéder à notre site **monprojet**, on pourra entrer l'URL : http://monprojet.local.
Il faut pour cela ajouter l'hôte au fichier **hosts** de notre système d'exploitation :
- Sous Windows : **C:\Windows\System32\Drivers\etc\hosts**
- Sous Linux : **/etc/hosts**
Il faut ajouter un hôte pointant sur **127.0.0.1**.
Une fois ces modifications faites, nous devons configurer le serveur web. Nous supposons que le projet est installé dans le répertoire **/var/www/mon_projet**.
Voici la configuration d'un VirtualHost Apache :

```
<VirtualHost *:80>
	ServerName monprojet.local

	DocumentRoot /var/www/monprojet/public
	<Directory /var/www/monprojet/public>
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>
```

Symfony facilite la configuration d'Apache avec un fichier **.htaccess** présent dans le répertoire **public**. Ce fichier n'est pas présent par défaut et doit être installé avec une recette Flex via Composer. Ceci est réalisé à l'aide de la commande suivante :

```bash
composer require symfony/apache-pack
```

Afin que le fichier **.htaccess** soit bien pris en compte par Apache, il est important que l'option **AllowOverride** soit placée à **All** ou au minimum **FileInfo**. Le module **mod_rewrite** doit également être activé.

L'application Symfony peut également fonctionner grâce à Nginx, nous ne verrons cependant pas comment le configurer ici #go-deeper.

On peut maintenant vérifier l'accès à l'application en entrant l'URL dans notre navigateur :
	http://monprojet.local/

## Structure de l'application

### Arborescence du projet

Une application Symfony contient différents répertoires, chacun ayant un but bien précis. Nous dressons ici la liste de ces derniers :
- **bin** : ce répertoire contient les exécutables du projet ainsi que des dépendances. Il contient la console Symfony.
- **config** : ce répertoire contient les fichiers de configuration de l'application.
- **migrations** : ce répertoire contient des scripts PHP générés par Symfony.
- **public** : ce répertoire contient toutes les ressources publiques accessibles directement depuis internet via l'URL http://monprojet.local/fichier. Les fichiers contenus dans ce répertoires sont statiques mis à part le contrôleur PHP principal : **index.php**.
- **src** : ce répertoire contient toutes les classes métiers ainsi que les contrôleurs. C'est le cœur du projet.
- **templates** : ce répertoire contient les templates Twig.
- **tests** : ce répertoire contient les différents tests logiciels, unitaires, d'intégration ou fonctionnels qui seront par la suite exécutés par **PHPUnit**.
- **translations** : ce répertoire contient les fichiers de traduction du site.
- **var** : ce répertoire contient le cache et les logs de notre application.
- **vendor** : ce répertoire est généré par Composer et contient toutes les dépendances.

### Règles et conventions d'organisation du projet

Les normes PSR sont des standards définis par les responsables des différents framework PHP. Elles sont ensuite adoptées par chaque framework pour une meilleure interopérabilité.
La norme PSR-4 est un standard concernant le chargement automatique des classes. Ainsi, si l'on définit un chargeur automatique de classes se basant sur la norme PSR-4, la classe **Eni\MonEspace\MaClasse** devra être définie à l'emplacement suivant :
	**src/Eni/MonEspace/MaClasse.php**
Et son contenu pourrait être :

```php
namespace Eni\MonEspace

class MaClasse {
	public function maMethode() {
		// ...
	}
// ...
}
```

Attention, il ne faut pas utiliser d'underscore dans le nom des classes car il n'est pas pris en charge par la norme PSR-4.