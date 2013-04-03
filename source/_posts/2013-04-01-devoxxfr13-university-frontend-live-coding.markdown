---
layout: post
title: "Devoxx France 2013 # University # Frontend Live Coding"
date: 2013-04-04 13:35
comments: true
categories: devoxxfr2013 yeoman grunt sass requirejs handlebars backbonejs
---

Vous pouvez retrouvez la description de la session [sur le site de Devoxx France](http://www.devoxx.com/display/FR13/Frontend+Live+Coding+++Tour+d%27horizon+de+l%27outillage+et+des+technos+web+d%27aujourd%27hui).

{% img center http://blog.roddet.com/images/devoxxfr13/frontend/affiche.jpg %}

## L'animateur
Frédéric Camblor [@fcamblor](https://twitter.com/fcamblor)  ([Bio](http://www.devoxx.com/display/FR13/Frederic+Camblor))


## Pourquoi j'ai choisi cette session ?
Avec le boost que lui a apporté HTML 5, les technologies "pures web" ont gagnées en popularité. Côté Java EE (serveur), nous sommes bien outillés (ide, build, qualité etc...), j'étais curieux de découvrir le panorama d'outils qui allait être présenté côté frontend.

C'est dans une salle d'environ 200 personnes qu'à eu lieu cette séance dense (riche en contenu).
Cette session s'est déroulée en 2 parties, l'animateur va commencer par faire un tour de l'outillage web d'aujourd'hui puis va présenter quelques briques de developpement d'une application web.


## Yeoman
{% img left http://blog.roddet.com/images/devoxxfr13/frontend/yeoman-logo.jpg %}
[Yeoman](http://yeoman.io/) va être le premier à être présenté. Il se définit comme étant un ensemble d'outils et leurs bonnes pratiques associées.

Il est composé de 3 outils :

{% img center http://blog.roddet.com/images/devoxxfr13/frontend/yeoman-tools.jpg %}

* [Yo](https://github.com/yeoman/yo) : outil permettant de générer le squelette d'un projet concentrant les bonnes pratiques à appliquer à une application web.
* [Grunt](http://gruntjs.com/) : outil permettant de lancer les tâches (minification des fichiers javascript, packaging, etc...)
* [Bower](https://github.com/twitter/bower) : gestionnaire de package pour le web. Il va s'occuper de gérer les dépendances de l'application web. Il permet aussi de récupérer les dépendances transitives.


### Installer Yeoman
En pré-requis, il faut installer [Nodejs](http://nodejs.org/).
Pour installer Yeoman, il faut lancer l'installation des 3 outils cités précedemment via la commande :
```
npm install -g yo grunt-cli bower 
```
[NPM](https://npmjs.org/) est le gestionnaire de packages de Nodejs. La commande "npm install" permet de lancer l'installation d'un package. L'option "-g" signifie que l'installation sera globale, c'est à dire non spécifique au projet.

### Générer le squelette d'un projet
Pour générer une application web prête à l'emploi, il suffit de lancer la commande :
```
yo webapp
```
Et on obtient le résultat suivant en répondant "Y" à toutes les questions :
```
	     _-----_
	    |       |
	    |--(o)--|   .--------------------------.
	   `---------´  |    Welcome to Yeoman,    |
	    ( _´U`_ )   |   ladies and gentlemen!  |
	    /___A___\   '__________________________'
	     |  ~  |
	   __'.___.'__
	 ´   `  |° ´ Y `

	Out of the box I include HTML5 Boilerplate, jQuery and Modernizr.
	Would you like to include Twitter Bootstrap for Sass? (Y/n) Y
	Would you like to include RequireJS (for AMD support)? (Y/n) Y
	   create Gruntfile.js
	   create package.json
	   create .gitignore
	   create .gitattributes
	   create .bowerrc
	   create component.json
	   create .jshintrc
	   create .editorconfig
	   create app/favicon.ico
	   create app/404.html
	   create app/robots.txt
	   create app/.htaccess
	   create app/scripts/vendor/bootstrap.js
	   create app/styles/main.scss
	   create app/scripts/app.js
	   create app/index.html
	   create app/scripts/main.js
	   create app/scripts/hello.coffee
	   invoke   mocha:app
	   create     test/index.html
	   create     test/lib/chai.js
	   create     test/lib/expect.js
	   create     test/lib/mocha/mocha.css
	   create     test/lib/mocha/mocha.js
	   create     test/spec/test.js

	I'm all done. Just run npm install && bower install to install the required dependencies.
```

Un squelette de projet est ainsi généré.

### Récupérer toutes les packages (dépendances)
```
npm install && bower install
```
On obtient le téléchargement d'une partie d'internet.

```
.....
npm http GET https://registry.npmjs.org/timespan
npm http GET https://registry.npmjs.org/graceful-fs
npm http GET https://registry.npmjs.org/inherits
npm http GET https://registry.npmjs.org/winston
npm http GET https://registry.npmjs.org/cli/0.4.3
npm http GET https://registry.npmjs.org/minimatch
npm http GET https://registry.npmjs.org/qs/0.5.1
npm http GET https://registry.npmjs.org/formidable/1.0.11
npm http GET https://registry.npmjs.org/crc/0.2.0
npm http GET https://registry.npmjs.org/cookie/0.0.4
....

```
En tant que développeur Java EE, on est habitué :)

### Anatomie d'une application web
A ce stade on se retrouve avec 3 répertoires à la racine :

* /app : l'application web(root de l'application)
* /test : les tests
* /node_modules : les packages téléchargés par NPM principalement pour exécuter les tâches Grunt.

Et quelques fichiers de configurations : 

* .bowerrc : fichier de configuration de bower. Il contient :
``` javascript
{
    "directory": "app/components"
}
```
Le paramètre "directory" indique l'emplacement des packages téléchargés par bower.

* Gruntfile.js : fichier de configuration de Grunt. Il contient le paramétrage des différentes tâches.

* .jshintrc : paramétrage de [JSHint](http://www.jshint.com/), outil de vérification de la qualité du code.

* component.json : configuration bower du projet.
``` javascript
{
  "name": "test",
  "version": "0.0.0",
  "dependencies": {
    "sass-bootstrap": "~2.3.0",
    "requirejs": "~2.1.4",
    "modernizr": "~2.6.2",
    "jquery": "~1.9.1"
  },
  "devDependencies": {}
}
```
Ce fichier comporte les dépendances du projet. Attention Bower se charge de récupérer les packages mais ne pas modifier l'application pour utiliser la librairie.

* package.json : identité du projet et dépendances de build.
```
{
  "name": "test",
  "version": "0.0.0",
  "dependencies": {},
  "devDependencies": {
    "grunt": "~0.4.0",
    "grunt-contrib-copy": "~0.4.0",
    "grunt-contrib-concat": "~0.1.2",
    "grunt-contrib-coffee": "~0.4.0",
    "grunt-contrib-uglify": "~0.1.1",
    "grunt-contrib-compass": "~0.1.2",
    "grunt-contrib-jshint": "~0.1.1",
    "grunt-contrib-cssmin": "~0.4.1",
    "grunt-contrib-connect": "0.1.2",
    "grunt-contrib-clean": "0.4.0",
    "grunt-contrib-htmlmin": "0.1.1",
    "grunt-contrib-imagemin": "0.1.2",
    "grunt-contrib-livereload": "0.1.1",
    "grunt-bower-hooks": "~0.2.0",
    "grunt-usemin": "~0.1.9",
    "grunt-regarde": "~0.1.1",
    "grunt-requirejs": "~0.3.2",
    "grunt-mocha": "~0.2.2",
    "grunt-open": "~0.2.0",
    "matchdep": "~0.1.1"
  },
  "engines": {
    "node": ">=0.8.0"
  }
}
```

C'est un peu dommage de n'avoir pas fusionné le fichier package.json et component.json (même structure, contenu similaire). Surtout qu'il est possible de configurer bower pour qu'il utilise un autre fichier que "component.json" donc éventuellement "package.json". Nous allons peut-être l'avoir dans une futur version de Yeoman ou bien il y a une raison pour cette distinction :)


Le nommage des numéros de version obéit à des règles du [semantic versionning](http://semver.org/).
On peut remarquer l'utilisation du caractère "~" dans certains numéros de version. 
La version ~0.4.0 permet de récupérer la version 0.4.0 d'une librairie ou la version mineure la plus à jour (0.4.1 par exemple).

Ce qu'il faut également savoir avec NPM et Bower est qu'ils génèrent un dépôt par projet par défaut, contrairement par exemple à Maven.

## Saas
{% img left http://blog.roddet.com/images/devoxxfr13/frontend/sass.jpg %}
CSS permet d'appliquer des mises en forme à un document HTML. Malgré l'importance de ce language, CSS a d'énormes lacunes évidentes : impossible d'utiliser des variables, impossible d'effectuer des traitements, etc...

[Sass](http://sass-lang.com/) est une extension de CSS3 ayant l'objectif de combler les lacunes de CSS.
La syntaxe CSS est entièrement compatible avec Sass.
Le code Sass est compilé pour obtenir le CSS cible qui sera interprété par un navigateur.

Un fichier Sass est un fichier ayant l'extension .scss (nouveau) ou .sass (ancien).

La page d'accueil du site [http://sass-lang.com/](http://sass-lang.com/) présente un comparatif de code équivalent entre CSS et Sass qui permet de voir rapidement l'intérêt.

{% img center http://blog.roddet.com/images/devoxxfr13/frontend/sass_1.jpg %}

{% img center http://blog.roddet.com/images/devoxxfr13/frontend/sass_2.jpg %}

Le projet généré par Yo précédemment possède une tâche Grunt qui compile "en live" nos modifications, ce qui rend complètement transparente cette phase de compilation. 

## Démonstration bower
Bower vient avec des outils permettant de rechercher un package, d'obtenir des informations sur celui-ci, de récupérer une dépendance etc...
Voici une démonstration :

### Rechercher une librairie dans le dépôt central
```
bower search jquery
```
On obtient :
```
Search results:

  - jquery git://github.com/components/jquery.git
  - jquery-ui git://github.com/components/jqueryui
  - jquery.cookie git://github.com/carhartl/jquery-cookie.git
  - jquery-placeholder git://github.com/mathiasbynens/jquery-placeholder.git
  - jquery-waypoints git://github.com/imakewebthings/jquery-waypoints.git
  - jquery-pjax git://github.com/defunkt/jquery-pjax.git
  - jquery-file-upload git://github.com/blueimp/jQuery-File-Upload.git
  - jasmine-jquery git://github.com/velesin/jasmine-jquery
...
```
On remarque immédiatement que les librairies accessibles via Bower sont en fait des dépôts Git. A noter que ces dépôts sont différents de ceux hébergeant les sources des différents projets.

### Lister les différentes versions d'un package
```
bower info jquery-ui
```
La liste des tags au sens Git disponibles s'affichent.
```
jquery-ui 

  Versions:
    - 1.10.2
    - 1.10.1
    - 1.10.0
    - 1.9.2
    - 1.9.1
    - 1.9.0
    - 1.8.23
```


### Installer une bibliothèque
```
bower install --save backbone underscore
```
Bower clone le dépôt qui contient le livrable. 

### Ajouter une librairie à Bower
```
bower register foo git://...
```
Cette commande permet de déclarer sa librairie dans Bower.
Aucune règle particulière pour ajouter une librairie. Dès que celle-ci est ajoutée, elle accessible de tous. Il est cependant plus délicat de supprimer une librairie (il faut rentrer en contact avec les administrateurs).


## RequireJS
{% img left http://blog.roddet.com/images/devoxxfr13/frontend/requirejs.png %}
[RequireJS](http://requirejs.org/) perment de charger à la demande et de cloisonner des scripts Javascript. 

Elle est l'implémentation d'[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) la plus répandue.

Pour intégrer RequireJS dans un projet, il faut modifier le fichier HTML principal avec la balise script :
```
<script data-main="scripts/main" src="...requirejs.js"></script>
```
L'attribut "data-main" permet de désigner le script principal qui sera exécuté en premier.

RequireJS introduit la notion de module.

La fonction "define" permet de créer un module.

La fonction "require" permet de déclarer des dépendances.

Plus de détails [ici](http://requirejs.org/docs/api.html).


## Handlebars
{% img left http://blog.roddet.com/images/devoxxfr13/frontend/handlebars_logo.jpg %}
Handlebars est un moteur de templating côté client. 

Il permet de créer des fichiers HTML réutilisable.

Exemple de template :
{% img http://blog.roddet.com/images/devoxxfr13/frontend/handlebars_template.jpg %}


## Backbone.js
{% img left http://blog.roddet.com/images/devoxxfr13/frontend/backbone.jpg %}
[Backbone.js](http://backbonejs.org/) est une librairie qui permet de structurer une application avec une architecture MVC.

Exemple de contrôleur :
``` javascript
var MainRouterClass = Backbone.Router.extend({
        routes: {
            "!/hello": "sayHello",
            "!/listTechnos": "listTechnos"
        },
    
        initialize: function () {
            MainRouterClass.__super__.initialize.apply(this, arguments);
            // Starting urls handlings
            // See http://backbonejs.org/#Router
            Backbone.history.start();
        },
    
        sayHello: function(){
            console.log("hello has been called !");
            require(["views/HelloView"], function(HelloView){
                window.view = new HelloView({ el: $(".hero-unit") }).render();
            });
        },

        listTechnos: function(){
            require(["views/TechnoListingView"], function(TechnoListingView){
                window.view = new TechnoListingView({ el: $(".hero-unit") }).render();
            });
        }
    });
```
Exemple de modèle (il existe 2 catégories : les modèles et les collections.) :
``` javascript
var TechnoClass = Backbone.Model.extend({
        defaults: {
            type: 1
        },

        initialize: function(attributes, options){
            TechnoClass.__super__.initialize.call(this,attributes, options);
        }

        // Aliases
    });
```
Exemple de vue :
``` javascript
 var HelloViewClass = Backbone.View.extend({
        events: {
            "click #sayHelloBtn": "sayHello"
        },
    
        initialize: function(){
            HelloViewClass.__super__.initialize.apply(this, arguments);
        },
    
        render: function(){
            this.$el.html(viewTemplate({ who: "devoxxFr" }));
    
            return this;
        },

        sayHello: function(){
            console.log("Hello world ! "+new Date());
        }
    
    });
```

## Rivetsjs
{% img center http://blog.roddet.com/images/devoxxfr13/frontend/rivets.jpg %}
[Rivetsjs](http://rivetsjs.com/) est indépendant de Backbone.js.

Il permet de créer un binding unidirectionnel ou bi-directionnel entre un modèle (Backbone par exemple) et un élément du DOM, de formatter les données etc...

## Astuces Google Chrome

#### Arrêter automatiquement l'exécution lors d'une exception
{% img center http://blog.roddet.com/images/devoxxfr13/frontend/chrome1.png %}

#### Afficher un élément du DOM par id ("main" par exemple)
{% img center http://blog.roddet.com/images/devoxxfr13/frontend/chrome2.png %}

#### Rechercher un élément du DOM par id ("content" par exemple)

{% img center http://blog.roddet.com/images/devoxxfr13/frontend/chrome3.png %}
{% img center http://blog.roddet.com/images/devoxxfr13/frontend/chrome4.png %}


## Le code sur Github
[https://github.com/fcamblor/technostore/tree/devoxxfr13](https://github.com/fcamblor/technostore/tree/devoxxfr13)

## Quelques anglicismes de l'animateur
Nous le savons, nous sommes envahis de mots anglais dans notre métier. Nous passons notre temps à lire des ressources en anglais et cela s'est particulièrement senti auprès de l'animateur.
Voici un top 5 des expressions qui m'ont marqué (soit pour leurs fréquences, soit pour leurs originalités) :)

1. "out of the box" 
2. "baby step"
3. "bootstraper"
4. "draw back"
5. "overhead"

## Bilan
La session a été très intéressante. Beaucoup de concepts/outils ont été abordés et cela nous a donné un panorama de ce qui se fait actuellement côté frontend. 

Bravo à Frédéric qui a tout de même tenu 3h de livecoding !