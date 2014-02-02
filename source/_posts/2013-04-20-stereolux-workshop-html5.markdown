---
layout: post
title: "GDG Nantes & Stereolux : Workshop HTML5"
date: 2013-04-20 19:17
comments: true
categories: html5 angularjs stereolux gdgnantes
---
Le 16/04 dernier, le [Stéréolux](http://www.stereolux.org/) et le [GDG Nantes](http://www.gdgnantes.com/) ont organisé un Workshop HTML5.

{% img center http://blog.roddet.com/images/stereolux/html5/2013_04_16_workshop_html5.jpg %}

## Une présentation du GDG Nantes & Stereolux

{% img left http://blog.roddet.com/images/stereolux/html5/presentation_stereolux.jpg %}

L'événement commence par une présentation du GDG Nantes et de Stereolux.

Le GDG Nantes (GDG = Google Developpers Group) est le groupe d'utilisateurs des technologies Google de Nantes. Il organise régulièrement des conférences, des concours et même des apéros :)

Le Stereolux organise des événements (ateliers, conférence, concert, ...) autour de la création numérique.

Les deux entités collaborent pour la première fois ensemble sur un événement. On sentait l'attention particulière des leaders de ces entités dans le choix des mots. En effet, si le GDG Nantes a l'habitude d'avoir un public de développeur, il avait en face un public mixte (développeur et créateurs numériques). Et l'inverse pour le Stereolux :)

## HTML5 pour les créatifs

[Jean-François Garreau](https://plus.google.com/114772056028520115043/about) nous a ensuite fait une présentation générale d'HTML5.
Il a enchainé les démonstrations d'applications web présentant des animations graphiques nous plongeant dans des univers variés ([Google Maze](http://www.chrome.com/maze/), [Plink](http://www.chromeexperiments.com/detail/plink-multiplayer-music-experience/), [Rome](http://www.ro.me/)...).

On retiendra qu'HTML5 n'est pas uniquement l'affaire des développeurs d'applications de gestion mais aussi celle des créateurs d'animations diffusées sur le web.

{% img center http://blog.roddet.com/images/stereolux/html5/html5_creatif.jpg %}

## 3 Workshops
Il fallait faire un choix entre les workshops suivants :

* Responsive design
* 3D sur le web (WebGL)
* Applications web dynamiques (avec AngularJS)

J'aurai voulu assister à tous les workshops mais un choix devait être fait...snif...
Mon choix s'est porté sur "AngularJS".

## Workshop : Applications web dynamiques avec AngularJS
Cette session était animée par [Antoine Richard](https://twitter.com/richard_antoine), un passionné de la dynamique côté navigateur. Il avait prévu nous faire développer et c'est ce qui s'est passé !

{% img center http://blog.roddet.com/images/stereolux/html5/angular-4.png %}

## Installation de NodeJS
Les programmes d'installation de [NodeJS](http://nodejs.org/) ont été distribués pour les plateformes Linux, Mac et Windows. J'avais déjà NodeJS installé sur ma machine, donc rien à faire pour moi à cette étape.

Vous pouvez récupérer la dernière version de NodeJS : [http://nodejs.org/](http://nodejs.org/).

## Angular en quelques mots

* Single Page Application
* Etendre HTML
* Simple et puissant
* Par Google

## Récupérer les sources
Les sources du Workshop ont été publiées sur Github : [https://github.com/antoine-richard/angular-movie-workshop-early2013](https://github.com/antoine-richard/angular-movie-workshop-early2013). Vous pouvez naviguer à travers les branches pour voir les changements entre les différentes étapes.

```
git clone https://github.com/antoine-richard/angular-movie-workshop
```

Vous allez récupérer 3 fichiers avec l'arborescence suivante : 
{% img center http://blog.roddet.com/images/stereolux/html5/angular-1.png %}

En affichant le fichier index.html dans un navigateur on obtient :

{% img center http://blog.roddet.com/images/stereolux/html5/angular-2.png %}

## Manipuler le DOM "sans" javascript
Cette partie a pour objectif d'illustrer la mise en place d'AngularJS dans une application et une modification du DOM sans écriture de code Javascript

### Inclure le script angular.js
``` html app/index.html
<script src="lib/angular.js"></script>
```

### Déclarer l'attribut ng-app
``` html app/index.html
<div id="container" class="intro" ng-app>
```

L'attribut ```ng-app``` permet de d'indiquer l'emplacement de l'application AngularJS. Il est possible d'avoir plusieurs applications dans une même page.

### Déclarer un modèle "name" lié au champ de saisie
``` html app/index.html
<input type="text" ng-model="name"/>
```

### Utilisation du modèle pour afficher les mots saisis

{% codeblock app/index.html %}
{% raw %} 
<p>Hello {{name}} !</p>
{% endraw %} 
{% endcodeblock %}

### Fichier app/index.html complet
{% codeblock app/index.html %}
{% raw %} 
<!doctype html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title>Movies App - AngularJS workshop at Stereolux</title>
	<link rel="stylesheet" href="css/movies.css"/>
	<script src="lib/angular.js"></script>
</head>
<body>
	
	<div id="container" class="intro" ng-app>

		<label>What's your name ?</label>
		<input type="text" ng-model="name"/>
		<p>Hello {{name}} !</p>

	</div>

	<p id="footer">AngularJS workshop at Stereolux</p>
  	
</body>
</html>
{% endraw %} 
{% endcodeblock %}

### Test

{% img center http://blog.roddet.com/images/stereolux/html5/angular-3.png %}

Ce que nous saisissons est automatiquement copié à la suite de "Hello".

Cet exemple simple permet de mettre en évidence la philosophie d'AngularJS. La manipulation du DOM que nous faisons habituellement en javascript est dans cet exemple effectuée de façon déclarative.

## Maintenant un peu de cinéma !
Après avoir écrit nos premières lignes d'AngularJS, nous passons au développement d'une application permettant de visualiser une liste de films, le détail de chaque film et des informations sur les acteurs principaux.

Ce développement va être fait en plusieurs étapes :

* [Step-0 : Utilisation d'un template](#step-0)
* [Step-1 : Externalisation du contrôleur + Appel AJAX](#step-1)
* [Step-2 : Filtre et Tri](#step-2)
* [Step-3 : Navigation entre différentes vues](#step-3)
* [Step-4 : Service AngularJS](#step-4)
* [Step-5 : Animation](#step-5)
* [Step-6 : Directives](#step-6)
* [Step-7 : Code complet](#complet)

<a name="step-0"></a>
## Step-0 : Utilisation d'un template

### Récupérer les sources
{% codeblock %}
{% raw %} 
git clone https://github.com/antoine-richard/angular-movie-workshop-early2013 -b step-0
{% endraw %} 
{% endcodeblock %}

{% img center http://blog.roddet.com/images/stereolux/html5/angular-5.png %}

Il y a 2 parties :

* app : l'application que nous allons développer
* server : un backend qui expose des services REST

### Lancer le serveur
Se positionner à la racine des sources récupérées et lancer la commande :

{% codeblock %}
{% raw %} 
node server/web-server.js
{% endraw %} 
{% endcodeblock %}

L'application initiale est accessible via l'adresse : http://localhost:8000/app/index.html

{% img center http://blog.roddet.com/images/stereolux/html5/angular-6.png %}

A cette étape, les données affichées sont statiques dans la page HTML :

``` html app/index.html
...
<div id="container">

	<div>

		<h1>Movies</h1>

		<ul>
			<li>
				<span>Reservoir Dogs</span>
				<a href="#">Actors</a>
				<a href="#">Sheet</a>
				<span class="year">1992</span>
			</li>
			<li>
				<span>Pulp Fiction</span>
				<a href="#">Actors</a>
				<a href="#">Sheet</a>
				<span class="year">1994</span>
			</li>
		</ul>

	</div>
</div>
...
```
L'objectif est maintenant d'apporter du dynamisme à cette page (injection des données via javascript).

### Suppression des données statiques

{% codeblock app/index.html %}
{% raw %} 
<div id="container">
	<div ng-view></div>
</div>
{% endraw %} 
{% endcodeblock %}

Les noeuds ```<h1>``` et ```<ul>``` sont supprimés.

L'attribut ```ng-view``` déclare l'emplacement de la vue AngularJS. Nativement, il ne peut y avoir qu'un seul attribut ```ng-view``` par application AngularJS.

### Créer un fragment HTML représentant la liste des films
Créer un fichier ```app/partials/movies.html``` représentant le template des informations supprimées précédemment.

{% codeblock app/partials/movies.html %}
{% raw %} 
<h1>Movies</h1>

<ul>
	<li ng-repeat="movie in movies">
		<span>{{movie.title}}</span>
		<a href="#">Actors</a>
		<a href="#">Sheet</a>
		<span class="year">{{movie.year}}</span>
	</li>
</ul>
{% endraw %} 
{% endcodeblock %}

On remarquera l'utilisation de ```ng-repeat``` qui est une directive fournie par AngularJS pour itérer sur un ensemble d'objets.
Dans notre cas, la boucle se fera sur la liste des films (variable ```movies```).

{% raw %}{{movie.title}}{% endraw %} permet d'afficher la propriété title de l'objet ```movie```.

### Modifier le contrôleur

``` javascript app/js/app.js
angular
	.module('moviesApp', [])
	.config(function($routeProvider) {
		$routeProvider.when('/movies', { controller: 'MoviesCtrl', templateUrl: 'partials/movies.html' });
	})
	.controller('MoviesCtrl', function($scope) {
		$scope.movies = [
			{ title: "Reservoir Dogs", year: 1992 },
			{ title: "Pulp Fiction", year: 1994 },
			{ title: "Jackie Brown", year: 1997 }
		];
	});
```

$routeProvider permet de faire le lien entre une URL, un contrôleur et un template.

$scope.movies permet d'ajouter une variable qui porte le nom ```movies``` et qui sera visible dans les vues. C'est sur cette variable qu'est appliqué la directive ```ng-repeat``` à la précédente étape.

### Test
http://localhost:8000/app/index.html#/movies

{% img center http://blog.roddet.com/images/stereolux/html5/angular-7.png %}

<a name="step-1"></a>
## Step-1 : Externalisation du contrôleur + Appel AJAX
L'objectif de cette étape est de mettre à jour la vue non plus via un tableau javascript "en dur" mais avec un appel serveur asynchrone. Nous allons profiter pour externaliser les contrôleurs de l'application.

### Inclusion du nouveau fichier à créer
```html app/index.html
<script src="js/controllers.js"></script>
```

### Suppression du contrôleur dans le fichier app/js/app.js
```javascript app/js/app.js
var app = angular.module('moviesApp', []);

app.config(function($routeProvider) {
 	$routeProvider.when('/movies', { controller: 'MoviesCtrl', templateUrl: 'partials/movies.html' });
});
```

### Création du fichier app/js/controllers.js
```javascript app/js/controllers.js
app.controller('MoviesCtrl', ['$http', '$scope', function($http, $scope) {
	$http.get('/server/data/movies.json/$').success(function(movies) {
		$scope.movies = movies;
	});
}]);
```
$http fourni par AngularJS permet de faire un appel HTTP asynchrone. 

A noter qu'il est possible d'utiliser $resource pour effectuer des appels REST. Son utilisation doit être préférée à $http lorsque le serveur à interroger est RESTful. $http est un service de plus bas niveau que $resource.


### Test

http://localhost:8000/app/index.html#/movies

{% img center http://blog.roddet.com/images/stereolux/html5/angular-9.png %}

<a name="step-2"></a>
## Step-2 : Filtre et Tri
Dans cette étape, l'écran principal va être enrichi pour permettre un filtre de données par titre et un tri suivant plusieurs critères.

### Ajout d'un filtre et un tri
{% codeblock app/partials/movies.html %}
{% raw %} 
<h1>Movies</h1>

<div class="filter-box">
	<div>
		<label>Filter</label>
		<input type="text" ng-model="query.title">
	</div>
	<div>
		<label>Sort by</label>
		<select ng-model="order" ng-init="order='year'">
			<option value="year">Year</option>
			<option value="title">Title</option>
		</select>
	</div>
</div>

<ul>
	<li ng-repeat="movie in movies | filter:query | orderBy:order">
		<span>{{movie.title}}</span>
		<a href="#">Actors</a>
		<a href="#">Sheet</a>
		<span class="year">{{movie.year}}</span>
	</li>
</ul>
{% endraw %} 
{% endcodeblock %}

__ng-model="query.title"__ permet de faire une correspondance entre la saisie de la partie filter et la valeur de la propriété ```title``` du filtre.

__movie in movies | filter:query__ le symbole ```|``` permet de définir une fonction à appliquer sur un ensemble de données, ici ```movies```. Le mot clé ```filter``` permet de filtrer des données suivant un ensemble de critères. Ici l'objet ```query``` est un conteneur qui contient l'ensemble de clé/valeur représentant le filtre à appliquer. Dans notre exemple, il contient ```title=XXX``` où ```XXX``` est la valeur saisie.

La combinaison des deux éléments précédents permet ainsi d'appliquer un filtre sur les films affichés en fonction du texte saisi.

__ng-model="order"__ déclare un modèle qui porte le nom ```order``` et qui contient la valeur sélectionnée dans la liste déroulante.

__ng-init="order='year'"__ déclare la valeur initiale (à l'affichage de la vue) du modèle ```order```. Cela permet d'appliquer un tri par défaut.

__| orderBy:order__ applique un tri suivant une propriété (valeur du modèle ```order```).

### Test
{% img center http://blog.roddet.com/images/stereolux/html5/angular-8.png %}

### Préparation de l'étape suivante
Ajoutons ensuite les 2 fichiers suivants pour préparer l'étape "Step-3".

* ```app/partials/actors.html``` : template d'affichage des acteurs d'un film
``` html app/partials/actors.html
<h2>
	Pulp Fiction
	<a href="#/movies">Home</a>
</h2>
<h3>
	Actors
	<a href="#/movies/44">Movie</a>
</h3>

<ul>
	<li>
		<span>Bruce Willis</span>
		<span class="role">Butch Coolidge</span>
	</li>
	<li>
		<span>Samuel L. Jackson</span>
		<span class="role">Jules Winnfield</span>
	</li>
	<li>
		<span>John Travolta</span>
		<span class="role">Vincent Vega</span>
	</li>
</ul>
```

* ```app/partials/movie.html``` : template d'affichage du détail d'un film.

``` html app/partials/movie.html
<h2>
	Pulp Fiction
	<a href="#/movies">Home</a>
</h2>
<h3>
	Info sheet
	<a href="#/movies/44/actors">Actors</a>
</h3>

<ul>
	<li>
		<span class="info">Year</span>
		<span>1994</span>
	</li>
	<li>
		<span class="info">Duration</span>
		<span>154 minutes</span>
	</li>
	<li>
		<span class="info">Budget</span>
		<span>$8.5 million</span>
	</li>
</ul>

<img src="/server/data/posters/pulp_fiction.jpg">
```
<a name="step-3"></a>
## Step-3 : Navigation entre différentes vues
Cette étape va permettre la mise en place de plusieurs vues accessibles via des URL spécifiques :

* ```/movies``` : la vue développée jusqu'ici
* ```/movies/:movieId``` (exemple /movies/4) : affichage du détail d'un film, le template à utiliser est ```app/partials/movie.html```
* ```/movies/:movieId/actors``` (exemple /movies/4/actors) : affichage des acteurs principaux d'un film, le template à utiliser est ```app/partials/actors.html```

### Configuration de la navigation  
```javascript app/js/app.js
var app = angular.module('moviesApp', []);

app.config(function($routeProvider) {
 	$routeProvider
 		.when('/movies', 					{ controller: 'MoviesCtrl',			templateUrl: 'partials/movies.html' })
 		.when('/movies/:movieId', 			{ controller: 'MovieDetailCtrl', 	        templateUrl: 'partials/movie.html'  })
 		.when('/movies/:movieId/actors',	{ controller: 'MovieActorsCtrl',	        templateUrl: 'partials/actors.html' })
 		.otherwise(						{ redirectTo: '/movies' });
});
```
### Mise à jour des contrôleurs
```
app.controller('MoviesCtrl', ['$http', '$scope', function($http, $scope) {
	$http.get('/server/data/movies.json/$')
	.success(function(movies) {
		$scope.movies = movies;
	});
}]);

app.controller('MovieDetailCtrl', ['$http', '$scope', '$routeParams', function($http, $scope, $routeParams) {
	$http.get('/server/data/movies.json/'+$routeParams.movieId+'/$') // Lightweight movie object
	.success(function(lightweightMovie) {
		$scope.movie = lightweightMovie;
	});
}]);

app.controller('MovieActorsCtrl', ['$http', '$scope', '$routeParams', function($http, $scope, $routeParams) {
	$http.get('/server/data/movies.json/'+$routeParams.movieId) // Heavyweight movie object
	.success(function(fullMovie) {
		$scope.movie = fullMovie;
	});
}]);
```

### Le template d'affichage des acteurs d'un film
{% codeblock app/partials/actors.html %}
{% raw %} 
<h2 class="light">
	{{movie.title}}
	<a href="#/movies">Home</a>
</h2>
<h3>
	Actors
	<a href="#/movies/{{movie.id}}">Movie</a>
</h3>

<ul>
	<li ng-repeat="actor in movie.actors">
		<span>{{actor.name}}</span>
		<span class="role">{{actor.role}}</span>
	</li>
</ul>
{% endraw %} 
{% endcodeblock %}

### Le template d'affichage du détail d'un film
{% codeblock app/partials/movie.html %}
{% raw %} 
<h2>
	{{movie.title}}
	<a href="#/movies">Home</a>
</h2>
<h3>
	Info sheet
	<a href="#/movies/{{movie.id}}/actors">Actors</a>
</h3>

<ul>
	<li>
		<span class="info">Year</span>
		<span>{{movie.year}}</span>
	</li>
	<li>
		<span class="info">Duration</span>
		<span>{{movie.duration}} minutes</span>
	</li>
	<li>
		<span class="info">Budget</span>
		<span>${{movie.budget}} million</span>
	</li>
</ul>

<img ng-src="{{movie.poster}}">
{% endraw %} 
{% endcodeblock %}

### Mise à jour du template d'affichage de tous les films
{% codeblock app/partials/movies.html %}
{% raw %} 
<h1>Movies</h1>

<div class="filter-box">
	<div>
		<label>Filter</label>
		<input type="text" ng-model="query.title">
	</div>
	<div>
		<label>Sort by</label>
		<select ng-model="order" ng-init="order='year'">
			<option value="year">Year</option>
			<option value="title">Title</option>
		</select>
	</div>
</div>

<ul>
	<li ng-repeat="movie in movies | filter:query | orderBy:order">
		<span>{{movie.title}}</span>
		<a href="#/movies/{{movie.id}}/actors">Actors</a>
		<a href="#/movies/{{movie.id}}">Sheet</a>
		<span class="year">{{movie.year}}</span>
	</li>
</ul>
{% endraw %} 
{% endcodeblock %}

### Test
{% img center http://blog.roddet.com/images/stereolux/html5/angular-10.png %}

{% img center http://blog.roddet.com/images/stereolux/html5/angular-11.png %}

{% img center http://blog.roddet.com/images/stereolux/html5/angular-12.png %}

<a name="step-4"></a>
## Step-4 : Mise en place d'un service AngularJS
AngularJS permet la mise en place de services réutilisables que les contrôleurs peuvent appeler. Cela permet d'avoir des contrôleurs ne contenant pas de logique complexe.

Nous allons maintenant mettre en place dans l'application une gestion de films favoris.

### Inclusion d'un nouveau fichier javascript pour nos services
``` html app/index.html
<script src="js/services.js"></script>
```

### Service de gestion de favoris
``` javascript app/js/services.js
app.factory('starService', function() {
	
	var starred = [];

	return {
		toggleStar: function(id) {
			starred[id] = !starred[id];
		},
		isStarred: function(id) {
			return starred[id];
		}
	}

});
```

La méthode ```factory``` permet de déclarer un service réutilisable avec un nom (ici ```starService```). Ce service est un singleton.

Il expose deux fonctions :

* ```toggleStar(id)``` pour ajouter en favori
* ```isStarred(id)``` qui détermine si un film est favori

### Utilisation du service depuis le contrôleur

```javascript app/js/controllers.js
app.controller('MovieDetailCtrl', ['$http', '$scope', '$routeParams', 'starService', function($http, $scope, $routeParams, starService) {
	$http.get('/server/data/movies.json/'+$routeParams.movieId+'/$') // Lightweight movie object
	.success(function(lightweightMovie) {
		$scope.movie = lightweightMovie;
		$scope.favorite = starService.isStarred(lightweightMovie.id) ? "filled" : "";
	});
	$scope.toggleStar = function(id) {
		starService.toggleStar(id);
		$scope.favorite = starService.isStarred(id) ? "filled" : "";
	}
}]);
```
Lors du retour de l'appel serveur, le service créé est utilisé pour savoir si le film récupéré a été mis en favori. Si le film est favori la variable ```favorite``` prend la valeur ```filled``` sinon elle vaudra "".

La fonction ```toggleStar``` mise dans l'objet __$scope__ lui permettra d'être utilisée par les templates.

### Mise à jour du template d'affichage du détail d'un film

{% codeblock app/partials/movie.html %}
{% raw %} 
<h2>
	{{movie.title}}
	<a href="#/movies">Home</a>
</h2>
<h3>
	Info sheet
	<a href="#/movies/{{movie.id}}/actors">Actors</a>
</h3>

<ul>
	<li>
		<span class="info">Year</span>
		<span>{{movie.year}}</span>
	</li>
	<li>
		<span class="info">Duration</span>
		<span>{{movie.duration}} minutes</span>
	</li>
	<li>
		<span class="info">Budget</span>
		<span>${{movie.budget}} million</span>
	</li>
	<li>
		<span class="info">Favorite</span>
		<span class="star {{favorite}}" ng-click="toggleStar(movie.id)"></span>
	</li>
</ul>

<img ng-src="{{movie.poster}}">

{% endraw %} 
{% endcodeblock %}

Deux choses à noter :

* {% raw %}{{favorite}}{% endraw %}  va prendre la valeur ```filled``` ou ```""``` suivant la logique définie dans le contrôleur
* ```ng-click``` permet de définir une action à exécuter lors du clic sur la zone favori. L'action consiste à appeler la fonction ```toggleStar``` définie précédemment avec l'identifiant du film.

### Test
{% img center http://blog.roddet.com/images/stereolux/html5/angular-13.png %}

<a name="step-5"></a>
## Step-5 : Animation
AngularJS intègre la possibilité d'appliquer des animations CSS. Cette fonctionnalité n'est accessible actuellement qu'en béta (version 1.1.4).

### Récupérer la version béta d'AngularJS et remplacer le fichier app/lib/angular.js
[AngularJS v1.1.4 béta](https://github.com/antoine-richard/angular-movie-workshop-early2013/raw/step-6/app/lib/angular.js)


### Ajouter le fichier ```app/css/animation.css```

```css app/css/animation.css
.custom-enter-setup, .custom-leave-setup, .custom-move-setup {
  -webkit-transition: .5s linear all;
  -moz-transition: .5s linear all;
  -o-transition: .5s linear all;
  transition: .5s linear all;
  position: relative;
}

.custom-enter-setup {
  left: -10px;
  opacity: 0;
}
.custom-enter-setup.custom-enter-start {
  left: 0;
  opacity: 1;
}

.custom-leave-setup {
  left: 0;
  opacity: 1;
}
.custom-leave-setup.custom-leave-start {
  left: -10px;
  opacity: 0;
}

.custom-move-setup {
  opacity: 0.5;
}
.custom-move-setup.custom-move-start {
  opacity: 1;
}
```

### Inclure le nouveau fichier CSS

```html app/index.html
...
<link rel="stylesheet" href="css/animation.css"/>
...
```

### Appliquer une animation à la liste des films

{% codeblock app/partials/movies.html %}
{% raw %} 
...
<ul>
	<li ng-repeat="movie in movies | filter:query | orderBy:order" ng-animate="'custom'">
		<span>{{movie.title}}</span>
		<a href="#/movies/{{movie.id}}/actors">Actors</a>
		<a href="#/movies/{{movie.id}}">Sheet</a>
		<span class="year">{{movie.year}}</span>
	</li>
</ul>
...
{% endraw %} 
{% endcodeblock %}

* On remarquera l'utilisation de la directive ```ng-animate``` pour appliquer une animation à un élément du DOM.

* ```custom``` représente le préfixe des noms des classes CSS qu'AngularJS va appliquer.

* Les suffixes ```enter-setup```, ```leave-setup```, ... que l'on voit dans le fichier ```app/css/animation.css``` sont des mots clés qu'AngularJS va utiliser pour appliquer les styles successifs dans l'ordre.

### Test
En rechargeant la page principale, on obtient une animation (changement d'opacité et translation de la gauche vers la droite).

<a name="step-6"></a>
## Step-6 : Directives
Nous avons jusqu'ici utiliser des directives fournies par AngularJS. Il est temps d'en créer une pour notre application.


### Création d'un fichier source de directives
```javascript app/js/directives.js 
angular.module('moviesApp.directives', [])
.directive('mvActor', function() {
    return {
    	restrict: 'E',
    	replace: true,
    	scope: {
    		name: '='
    	},
    	template: '<span>{{name}}</span>',
    	link: function(scope, element, attributes) {
    		if (scope.name === 'Samuel L. Jackson') {
    			scope.name += ' !';
    			element.addClass('samuel');
    		}
    	}
    };
});
```
Un nouveau module dont le nom est ```moviesApp.directives``` est créé. Il ne définit que la directive :

* porte le nom ```mvActor```
* est de type ```Element``` (restrict : E). Une directive peut être de type :
     * ```E``` : Element ```<mv-actor></mv-actor>```
     * ```A``` : Attribut  ```<div mv-actor="samuel" />```
     * ```C``` : Class  ```<div class="mv-actor:exp;" />```
     * ```M``` : Comment ```<!-- directive mv-actor exp -->```
* va remplacer l'élément du DOM sur lequel il est appliqué (```replace:true```)
* est paramétrable via une propriété ```name```
* va se baser sur le template défini pour l'attribut ```template``` pour générer l'affichage
* va exécuter la fonction définie pour la propriété ```link``` après la phase de "compilation" de la directive et avant de créer l'affichage de la vue. C'est via cette propriété que l'on peut injecter du dynamisme à une directive.

Cette directive applique un traitement particulier à "Samuel L. Jackson" :

* Ajout du ```!``` à la suite de son nom
* Application de la classe ```samuel``` qui permet d'afficher sa photo

### Inclure le nouveau fichier de script ```app/js/directives.js```
```html app/index.html
<script src="js/directives.js"></script>
```

### Déclaration du module directives comme dépendance de l'application

```javascript app/js/app.js
var app = angular.module('moviesApp', ['moviesApp.directives']);
...
```

### Utilisation de la directive

{% codeblock app/partials/actors.html %}
{% raw %}
<!--[if lte IE 8]>
    <script>
        document.createElement('mv-actor');
    </script>
<![endif]-->

<h2 class="light">
	{{movie.title}}
	<a href="#/movies">Home</a>
</h2>
<h3>
	Actors
	<a href="#/movies/{{movie.id}}">Movie</a>
</h3>

<ul>
	<li ng-repeat="actor in movie.actors">
		<mv-actor name="actor.name"></mv-actor>
		<span class="role">{{actor.role}}</span>
	</li>
</ul>
{% endraw %} 
{% endcodeblock %}

### Test

{% img center http://blog.roddet.com/images/stereolux/html5/angular-14.png %}

Voilà Samuel L. Jackson, un acteur au-dessus des autres, bien mis en valeur :)

<a name="complet"></a>
## Le code complet
{% codeblock %}
{% raw %} 
git clone https://github.com/antoine-richard/angular-movie-workshop-early2013 -b step-7
{% endraw %} 
{% endcodeblock %}
ou via l'interface web de Github [https://github.com/antoine-richard/angular-movie-workshop-early2013/tree/step-7](https://github.com/antoine-richard/angular-movie-workshop-early2013/tree/step-7)

## Le mot de la fin d'[Antoine Richard](https://twitter.com/richard_antoine)

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/rossioddet">rossioddet</a> merci :) préserve toi de manipuler le DOM hors des directives et ta maintenabilité sera bien gardée !</p>&mdash; Antoine RICHARD (@richard_antoine) <a href="https://twitter.com/richard_antoine/status/324261610392801280">April 16, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Liens utiles

* [Photos de l'événement](https://plus.google.com/events/c05p73b1ufl9n1d5uvgaepf3dhg)
* [Les sources Workshop AngularJS](https://github.com/antoine-richard/angular-movie-workshop-early2013)
* [Les sources Workshop CSS 3](https://github.com/jbodet/winteriscoming)
* [Les ressources Workshop WebGL](http://animateyourhtml5.appspot.com/)




