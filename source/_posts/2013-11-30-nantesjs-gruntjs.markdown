---
layout: post
title: "Soirée NantesJS : Découverte de GruntJS"
date: 2013-12-04 08:00
comments: true
categories: nantesjs
---


[Grunt](http://gruntjs.com/) permet de lancer des tâches afin d'automatiser des traitements récurrents.
Il se configure via l'écriture d'un code javascript.
Il existe de nombreux outils permettant d'automatiser le lancement des tâches : [ANT](http://ant.apache.org/), [Maven](http://maven.apache.org/), [Gradle](http://www.gradle.org/), [SBT](http://www.scala-sbt.org/).
La particularité de [Grunt](http://gruntjs.com/) par rapport aux autres outils est qu'il est conçu principalement pour traiter des tâches récurrentes du web : minification, optimisation des images, génération de sprites, compilation Sass/Less, etc.

La soirée [NantesJS](http://nantesjs.org/) du 19 novembre dernier a eu comme objectif une présentation et une prise en main de [Grunt](http://gruntjs.com/). 

{% img center /images/nantesjs/nov13/nantesjs-speakers.jpg %}

C'est devant un public composé de professionnels que [Thomas Moyse](https://twitter.com/t8g) (à gauche dans la photo) commence une présentation de [Grunt](http://gruntjs.com/). [Xavier Seignard](https://twitter.com/xavier_seignard) (à droite) prendra de temps en temps la parole pour partager son expérience sur l'utilisation de [Grunt](http://gruntjs.com/).

[Thomas](https://twitter.com/t8g) va mettre l'accent sur le nombre important de plugins de Grunt : 1688. Dans [la page des plugins Grunt](http://gruntjs.com/plugins), je compte 1847 plugins au moment de l'écriture de l'article. [Grunt](http://gruntjs.com/) dispose donc d'un écosystème riche.

Après la présentation succincte de [Grunt](http://gruntjs.com/), la soirée se transforme en Workshop et nous allons découvrir progressivement cet outil.

## Installer Grunt
[Grunt](http://gruntjs.com/) s'installe via [NPM](https://npmjs.org/) (Node Packaged Modules) le gestionnaire de package de [NodeJS](http://nodejs.org/).

[Télécharger et installer NodeJS](http://nodejs.org/download/). Veuillez vérifier l'accès à la commande `npm` après l'installation.

Installer le client grunt via la commande :
```
npm install -g grunt-cli
```
A savoir : chaque projet va posséder son installation de [Grunt](http://gruntjs.com/) et `grunt-cli` permet d'avoir accès à la commande `grunt` de façon globale quelque soit le projet.

## Création d'un projet
Créer un répertoire.
```
mkdir nantesjs-grunt
cd nantesjs-grunt
```

Initialiser un projet avec [NPM](https://npmjs.org/).
```
npm init
```
Tapez la touche entrée à toutes les questions.
Le fichier `package.json` est créé avec le contenu suivant :

``` javascript
{
  "name": "nantesjs-grunt",
  "version": "0.0.0",
  "description": "ERROR: No README.md file found!",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": "",
  "author": "",
  "license": "BSD"
}
```
Ce fichier représente l'identité d'un projet dans l'univers [NodeJS](http://nodejs.org/).

Installer [Grunt](http://gruntjs.com/) pour le projet.
```
npm install grunt --save-dev
```
Le fichier `package.json` est mis à jour comme suit :
``` javascript
{
  "name": "nantesjs-grunt",
  "version": "0.0.0",
  "description": "ERROR: No README.md file found!",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": "",
  "author": "",
  "license": "BSD",
  "devDependencies": {
    "grunt": "~0.4.2"
  }
}
```
Un répertoire `node_modules` est créé avec un répertoire `grunt` qui contient une installation de [Grunt](http://gruntjs.com/) dans sa version 0.4.2.


## Hello World Grunt !
[Grunt](http://gruntjs.com/) se configure dans un fichier `Gruntfile.js` avec du code javasript.

Créer un fichier `Gruntfile.js`.

Modifiez ce fichier comme suit pour créer un _Hello World_.
``` javascript
module.exports = function(grunt) {
	grunt.registerTask('hello', 'Ma tâche à moi', function(){
		console.log('Hello World Grunt !')
	});
}
```
La fonction `registerTask` permet la création d'une tâche avec le nom _hello_, la description _Ma tâche à moi_ et une fonction qui sera exécutée lorsque cette tâche sera appelée.

`console.log` écrit du texte dans la console.

Pour lancer la tâche _hello_, exécuter la commande :
```
grunt hello
```
Le résultat dans la console
```
>grunt hello
Running "hello" task
---------------------
Hello World Grunt !

Done, without errors.
```

## Une tâche par défaut
Il possible de spécifier une tâche par défaut à [Grunt](http://gruntjs.com/).
Il suffit de lui donner le nom _default_ comme suit :
``` javascript
module.exports = function(grunt) {
	grunt.registerTask('default', 'Ma tâche à moi', function(){
		console.log('Hello World Grunt !')
	});
}
```
La tâche est lancée via la commande suivante sans avoir à préciser le nom d'une tâche.
```
grunt
```

## Connaitre la liste des tâches disponibles
```
grunt --help
```

## Un alias pour composer des tâches
Un alias permet de composer plusieurs tâches. Par défaut, les tâches sont exécutées de façon séquentielle.

Un alias est créé à l'aide d'une fonction `registerTask` en précisant un nom d'alias et un tableau de noms des tâches.

``` javascript
module.exports = function(grunt) {
	grunt.registerTask('step1',function(){
		console.log('Step 1')
	});

	grunt.registerTask('step2', 'ma tâche à moi', function(){
		console.log('Step 2')
	});

	grunt.registerTask('all', ['step1', 'step2']);
}
```

Lancer l'alias _all_
```
> grunt all
Running "step1" task
--------------------
Step 1

Running "step2" task
--------------------
Step 2

Done, without errors.
```

## Le _multi task_
Le _multi task_ sert à créer des tâches qui peuvent s'exécuter suivant plusieurs cibles.

Ci-dessous un exemple d'utilisation de _multi task_.

``` javascript
module.exports = function(grunt) {

	grunt.initConfig({
		multitask1: {
			conf1:[1,2,3],
			conf2: 'hello world'
		}
	})

	grunt.registerMultiTask('multitask1', function(){
		console.log('target = ' + this.target);
		console.log("data = " + this.data);
	})
}
```

La fonction `initConfig` crée les différentes configurations possibles. Ici la tâche _multitask1_ possède 2 contextes d'exécution `conf1`et `conf2`. Chaque contexte d'exécution possède son jeu de données.

Dans la fonction d'exécution de la tâche _multitask1_, `this.target` contient le nom du contexte d'exécution et `this.data` contient les données associées.

Lancer la tâche _multitask1_ entraine son exécution pour les différents contextes d'exécution définis.

```
>grunt multitask1
Running "multitask1:conf1" (multitask1) task
--------------------------------------------
target = conf1
data = 1,2,3

Running "multitask1:conf2" (multitask1) task
--------------------------------------------
target = conf2
data = hello world

Done, without errors.
```

Il est possible d'exécuter une tâche de type _multi task_ sur une configuration particulière comme suit :

```
>grunt multitask1:conf2
Running "multitask1:conf2" (multitask1) task
--------------------------------------------
target = conf2
data = hello world

Done, without errors.
```

## Partager de la configuration via l'API _options_
L'API _options_ offre la possibilité de mutualiser de la configuration [Grunt](http://gruntjs.com/). Cette configuration peut être surchargée par un contexte d'exécution.

Voici un exemple d'utilisation des _options_ via le partage de configuration _key_ pour les différents contextes d'exécution.

``` javascript
module.exports = function(grunt) {

	grunt.initConfig({
		multitask1: {
			options:{key:"v1"},
			conf1:[1,2,3],
			conf2:'hello world',
			conf3:{
				options:{key:"v2"}
			}
		}
	});

	grunt.registerMultiTask('multitask1', function(){
		console.log('target = ' + this.target);
		console.log("data = " + this.data);
		console.log("version = " + this.options().key)
	});
}
```
Dans cet exemple, _key_ vaut 

* _v1_ pour _conf1_ et _conf2_
* _v2_ pour _conf3_

L'appel de la fonction `this.options().key` renvoie la valeur de la donnée _key_ dans le contexte d'exécution courant.

Le résultat de l'exécution de _multitask1_ donne :

``` 
>grunt multitask1
Running "multitask1:conf1" (multitask1) task
--------------------------------------------
target = conf1
data = 1,2,3
version = v1

Running "multitask1:conf2" (multitask1) task
--------------------------------------------
target = conf2
data = hello world
version = v1

Running "multitask1:conf3" (multitask1) task
--------------------------------------------
target = conf3
data = [object Object]
version = v2

Done, without errors.
```

## Les templates
Il est possible d'utiliser un template avec [Grunt](http://gruntjs.com/) par exemple pour partager une valeur entre plusieurs contextes d'exécution.

La partie variable d'un template se déclare via les symboles : _<%= XXXX %>_.

Un exemple d'utilisation d'un template.

``` javascript
module.exports = function(grunt) {

	grunt.initConfig({
		multitask1: {
			options:{key:'<%= version %>'},
			conf1:[1,2,3]
		},
		version : 'v15'
	});

	grunt.registerMultiTask('multitask1', function(){
		console.log("version = " + this.options().key);
	});
}
```

L'exécution de la tâche _multitask1_ affiche bien le numéro de version _v15_.

```
>grunt multitask1
Running "multitask1:conf1" (multitask1) task
--------------------------------------------
version = v15

Done, without errors.
```

## Plusieurs modes pour déclarer des fichiers
Les différentes tâches utilisées avec [Grunt](http://gruntjs.com/) consistent la plupart du temps à manipuler des fichiers.

Il existe plusieurs modes pour désigner des fichiers avec [Grunt](http://gruntjs.com/) : mode _compact_, mode _object_, mode _array_ et le mode _dynamic_.

Afin d'illustrer ces différents modes, nous allons utiliser le plugin `grunt-contrib-copy` qui donne la possibilité de copier des fichiers dans un répertoire.

Installer le plugin
```
npm install grunt-contrib-copy --save-dev
```
Le plugin est installé dans le répertoire `node_modules`. Le fichier `package.json` est mis à jour avec la ligne `"grunt-contrib-copy": "~0.4.1"` qui vient compléter la section `devDependencies`.

Mettre à jour le fichier `Gruntfile.js` comme suit.

``` javascript
module.exports = function(grunt) {

	grunt.initConfig({
		copy: {
			main: {
				???	
			}
		}
	});

	grunt.loadNpmTasks('grunt-contrib-copy');
}
```

La fonction `grunt.loadNpmTasks` charge le plugin `grunt-contrib-copy`. Ce plugin expose la tâche `copy` sous la forme d'une _multi task_ configurable dans la fonction `grunt.initConfig`.

La tâche `copy` peut être exécutée avec la commande :

```
grunt copy
```

## Mode _Compact_

``` javascript
module.exports = function(grunt) {

	grunt.initConfig({
		copy: {
			main: {
				src:['*.js', '*.json'],
				dest:'tmp/'	
			}
		}
	});

	grunt.loadNpmTasks('grunt-contrib-copy');
}

```
Deux paramètres `src` (fichiers à copier) et `dest` (répertoire de destination) sont définis.


## Mode _Object_

```
module.exports = function(grunt) {

	grunt.initConfig({
		copy: {
			main: {
				files : {'tmp/': ['*.js', '*.json']}
			}
			
		}
	});

	grunt.loadNpmTasks('grunt-contrib-copy');
}
```

## Mode _Array_

```
module.exports = function(grunt) {

	grunt.initConfig({
		copy: {
			main: {
				files : [
					{
						src:['*.js', '*.json'],
						dest:'tmp/'	
					}
				]
			}
			
		}
	});

	grunt.loadNpmTasks('grunt-contrib-copy');
}
```

## Mode _Dynamic_
Ce mode permet une configuration plus fine.

```
files: [{
	expand:boolean, // true => mode dynamique sinon mode précédent
	cwd:string, // le répertoire de base
	src: [string], // les fichiers sources
	dest: string, // le répertoire de destination
	ext:string, // ajouter une extension aux fichiers 
	flatten:boolean, // ne pas garder l'arborescence et tout mettre au même endroit.
	rename:function // renommer à la volée
}]
```

Exemple d'utilisation.
```
module.exports = function(grunt) {

	grunt.initConfig({
		copy: {
			main: {
				files: [{
					expand:true, 
					cwd:'node_modules/', 
					src: ['**/*.js'],
					dest: 'temp/'
				}]
			}
		}
	});

	grunt.loadNpmTasks('grunt-contrib-copy');
}
```

## Quelques trucs & astuces

### Les plugins
Utiliser [Grunt](http://gruntjs.com/) consiste surtout à ordonnancer des tâches de plugins existants. Il est rare d'avoir besoin de créer ses propres tâches. Il est conseillé de privilégier l'utilisation des plugins dont le nom commence par `grunt-contrib`. Il s'agit de plugins maintenus officiellement par l'équipe [Grunt](http://gruntjs.com/).

### Asynchronisme
Regarder du côté du projet [async](https://github.com/caolan/async) pour ajouter de l'asynchronisme dans l'exécution des tâches.

### Fonctions utilitaires de [Grunt](http://gruntjs.com/)
[Grunt](http://gruntjs.com/) vient avec quelques fonctions utilitaires comme : `grunt.file.copy`, `grunt.file.readJSON`, `grunt.template.today("yyyy-mm-dd")`, etc. Ne pas hésiter à les utiliser.

### Construire un site statique
Que ce soit pour un site web statique ou pour exposer de la documentation, le plugin [grunt-carpenter](https://github.com/TxSSC/grunt-carpenter/) permet de générer un site web statique à partir de fichier markdown et HTML.

### Lancer des commandes
Le plugin [grunt-shell](https://github.com/sindresorhus/grunt-shell) donne la possibilité de lancer des commandes dans le shell. Ce qui donne un degré de liberté important pour ordonnancer l'exécution des scripts ou même d'autres systèmes de _build_ ;)


## Les slides
Elles sont publiées [ici](http://slid.es/t8g/gruntjs).

<iframe src="//slid.es/t8g/gruntjs/embed" width="800" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

## En somme
Par la richesse de son écosystème de plugins, [Grunt](http://gruntjs.com/) est l'outil du moment pour effectuer du _build_ côté front-end. Il est plutôt simple d'utilisation et offre de vraies opportunités de productivité dans le développement web. En donnant la possibilité d'avoir différents contextes d'exécution, il est pratique pour séparer les tâches de développement des tâches de constructions du livrable de production.

[Grunt](http://gruntjs.com/) est très lié à l'environnement [NodeJS](http://nodejs.org/). Ce qui peut constituer un frein à son adoption pour les personnes évoluant dans d'autres écosystèmes.
Cette problématique n'est pas propre à [Grunt](http://gruntjs.com/). Les développeurs Java qui vantent par exemple les mérites de [Maven](http://maven.apache.org/) auront également du mal à le faire adopter à des personnes qui n'ont pas de [JDK](http://en.wikipedia.org/wiki/Java_Development_Kit) sur leur machine. Alors soyons ouvert d'esprit et n'ayons pas peur des écosystèmes qui peuvent nous être étrangers. Vous aimez [Jekyll](http://jekyllrb.com/) ? Installer [Ruby](http://jekyllrb.com/). Vous trouvez [AsciiDoc](http://www.methods.co.nz/asciidoc/index.html) sympa ? Installer [Python](http://www.python.org/). Vous aimez l'emploi ? Installer [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ;)

Je pense que la gestion des erreurs de configuration de [Grunt](http://gruntjs.com/) ou de ses plugins est perfectible. Durant le workshop, il m'est souvent arrivé d'écrire une configuration non autorisée sans qu'il n'y ait aucune erreur remontée. Par exemple, lorsque vous utiliser les _multi task_, il faut absolument déclarer au moins un contexte d'éxécution sinon la tâche déclarée ne sera pas exécutée même si vous l'appelez explicitement. Heureusement les erreurs de syntaxes sont plutôt bien remontées.

[Grunt](http://gruntjs.com/) en tant qu'outil de _build_ se prête bien à l'intégration continue. Alors il ne faut pas hésiter !

C'était la première fois que je participais à une soirée du [Nantes JS](http://nantesjs.org/). Les organisateurs et les participants ont été très accueillants, vous aviez une bière dans vos mains en guise de _bonsoir_. Je vous recommande vivement ces soirées que ce soit pour la bière ou pour votre amour de javascript (oui il y en a qui aime) ;)

La prochaine soirée est prévue au mois de janvier, restez connecter via [@NantesJS](https://twitter.com/NantesJS).

{% img center /images/nantesjs/nov13/nantesjs-logo.png %}


