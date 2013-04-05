---
layout: post
title: "Devoxx France 2013 # Tools In Action - Live coding avec Yeoman & AngularJS"
date: 2013-04-05 23:09
comments: true
categories: devoxxfr2013 yeoman angularjs
---

Vous pouvez retrouvez la description de la session [sur le site de Devoxx France](http://www.devoxx.com/pages/viewpage.action?pageId=6817513).

{% img center http://blog.roddet.com/images/devoxxfr13/yeomanangular/slide.jpg %}

## Pourquoi j'ai choisi cette session ?
[Yeoman](http://yeoman.io/) bien qu'encore en version béta fait déjà beaucoup parlé de lui. Cette session va être, en ce qui me concerne, la 2nd fois qu'on aborde Yeoman dans la même journée (voir [la session Frontend Live Coding](http://blog.roddet.com/2013/04/devoxxfr13-university-frontend-live-coding/)).

Quand à [Angular JS](http://angularjs.org/), il fait tant parler de lui et j'étais curieux de voir son intégration avec Yeoman.


## Le présentateur
Matthieu Lux [@swiip](https://twitter.com/swiip) [http://swiip.github.io/](http://swiip.github.io/)

{% img center http://blog.roddet.com/images/devoxxfr13/yeomanangular/speaker.jpg %}

## Yeoman
La session commence par une présentation rapide de Yeoman.

En résumé, il s'agit d'un ensemble d'outils qui existaient déjà :

* Yo - générateur
* Bower - gestion des dépendances
* Grunt - preview, build, test...

## AngularJS
Il définit AngularJS comme un framework MVC apportant du "déclaratif" dans le modèle de programmation par opposition au modèle "impératif" du traditionnel Javascript ou de JQuery.

{% img center http://blog.roddet.com/images/devoxxfr13/yeomanangular/angularmvc.png %}

## Démo

Pré-requis: avoir installé NodeJS.

Il faut ensuite installer Grunt, Bower, Yo, express et monk.
```
npm install -g yo grunt-cli bower express monk
```
Récupérer les trois fichiers ["serveur"](https://github.com/Swiip/yeoman-angular/tree/master/server) et copier les dans un répertoire. Ou encore faire un clone du dépôt.

Lancer la partie serveur :
```
node app.js
```
### Etape 1 : Ajouter les dépendances JQuery et Bootstrap
Modifier le fichier component.json
```
{
  ...
  "dependencies": {
  	....
	"jquery" : "1.8.0",
	"bootstrap" : "2.3.1"
  }
  ...
}
```

Récupérer les dépendances
```
bower install
```

Ajouter dans le fichier index.html... Oui Bower ne le fait pas pour vous !
```
    <script src="components/jquery/jquery.js"></script>
    <script src="components/bootstrap/docs/assets/js/bootstrap.js"></script>

```

### Etape 2 : Installer le générateur Angular et Karma

```
npm install generator-angular generator-karma
```
Les générateurs sont installés dans le répertoire node_modules.

### Etape 3 : Générer un projet Angular

```
yo angular
```
Répondre Oui à toutes les questions.

### Etape 4 : Installer les dépendances
```
npm install && bower install 
```

### Etape 5 : Lancer l'application générée
```
grunt server
```

{% img center /images/devoxxfr13/yeomanangular/webapp_1.png %}


### Etape 6 : Modifier app/scripts/app.js

``` javascript
'use strict';

angular.module('clientApp', ['ngResource'])
  .factory('Frameworks', function($resource) {
    return $resource('http://localhost\\:1234/frameworks', {
      id: '@_id'
    })
  })
  .config(function ($routeProvider) {
    $routeProvider
      .when('/', {
        templateUrl: 'views/main.html',
        controller: 'MainCtrl'
      })
      .otherwise({
        redirectTo: '/'
      });
  });
```

### Etape 7 : app/scripts/controller/main.js

``` javascript
'use strict';

angular.module('clientApp')
  .controller('MainCtrl', function ($scope, Frameworks) {
    var modal = $('.modal');

    $scope.hello = 'Hello Devoxx !';

    $scope.frameworks = Frameworks.query();

    $scope.edit = function(framework) {
      $scope.framework = framework;
      modal.modal('show');
    }

    $scope.save = function() {
      if($scope.framework.$save) {
        $scope.framework.$save();
      } else {
        $scope.frameworks.push(Frameworks.save($scope.framework));
      }
      modal.modal('hide');
    }

    $scope.delete = function(framework, $event, $index) {
      framework.$delete();
      $event.stopPropagation();
      $scope.frameworks.splice($index, 1);
    }
  });
```

### Etape 8 : app/views/main.html

``` javascript
<div class="hero-unit">
  <h1>{{hello}}</h1>
</div>

<table class="table table-striped table-bordered">
    <tr><th>Name</th><th>URL</th></tr>
    <tr ng-repeat="framework in frameworks" ng-click="edit(framework)">
        <td>{{framework.name}}</td>
        <td>{{framework.url}}</td>
        <td><button class="btn btn-danger" ng-click="delete(framework, $event, $index)">Delete</button> </td>
    </tr>
</table>

<button class="btn" ng-click="edit({})">Add</button>

<div class="modal hide fade">
    <div class="modal-header"><h2>Edit</h2></div>
    <div class="modal-body">
        <label>Name</label>
        <input type="text" ng-model="framework.name">
        <label>URL</label>
        <input type="text" ng-model="framework.url">
    </div>
    <div class="modal-footer">
        <button class="btn" ng-click="save()">Save</button>
    </div>
</div>
```

## L'application développée

{% img center http://blog.roddet.com/images/devoxxfr13/yeomanangular/webapp_2.png %}

Le présentateur a durant la session appliqué des styles que vous pouvez retrouver [ici](https://github.com/Swiip/yeoman-angular/tree/master/repets/client-v8/app/styles)

## Les slides
Vous pouvez les télécharger [ici](https://github.com/Swiip/yeoman-angular/raw/master/slides/Devoxx%202013%20FR%20Yeoman%20%26%20AngularJS.ppt).

## Le code sur Github
[https://github.com/Swiip/yeoman-angular](https://github.com/Swiip/yeoman-angular)

## Bilan
En somme, je retiens qu'AngularJS permet de faire plus de HTML et moins de javascript. C'est un framework complet qui intègre de nombreux services et peut se mettre aisément en oeuvre avec Yeoman.