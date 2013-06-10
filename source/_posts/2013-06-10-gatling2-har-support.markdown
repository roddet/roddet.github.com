---
layout: post
title: "Gatling 2 : Support du format HAR"
date: 2013-06-10 22:05
comments: true
categories: gatling2 gatling har
---
Gatling 2 introduit le support du format HAR ! Super...:\ 

## Oui mais en fait c'est quoi ce format HAR ?

{% blockquote Wikipédia http://fr.wikipedia.org/wiki/Format_HTTP_Archive %}
Le Format HTTP Archive (HAR) est un format ouvert destiné à l'export et l'échange de données collectées par des outils de monitoring HTTP.
...
Le Format HTTP Archive permet de sauvegarder et d"échanger le "détail de la chronologie de chargement d'une page WEB".
{% endblockquote %}

En résumé, il s'agit d'un fichier au format JSON (ou XML) qui contient les différents appels effectués vers un serveur web.

## Comment peut-on créer un fichier HAR ?

### Avec Chrome

* Lancer les outils de développement Chrome
* Se positionner à l'onglet Network
* Effectuer une navigation à travers plusieurs pages web ou recharger simplement la page courante.
* Effectuer un clic droit sur une des requêtes. 2 options nous intéressent :
	* *Copy All as HAR* : faire un export dans le presse-papier que nous pourrons coller dans un fichier
	* *Save as HAR with Content* : enregistrement dans un fichier du HAR

{% img center /images/gatling2/har/gatling2_har_chrome_export.png %}

### Avec Firefox

Vous pouvez installer les extensions suivantes :

* [Firebug](https://addons.mozilla.org/en-US/firefox/addon/firebug/) : outil pour développeur web
* [NetExport](https://getfirebug.com/releases/netexport/) : une extension de Firebug permettant d'effectuer un export de l'activité réseau au format HAR

{% img center /images/gatling2/har/gatling2_har_firefox_export.png %}

### Internet Explorer

Il est possible de faire un export HAR depuis IE9.

{% img center /images/gatling2/har/gatling2_har_ie_export.png %}

Petite surprise : c'est du XML et non du JSON...

## Que contient un fichier HAR ?

Vous pouvez télécharger un exemple de fichier HAR [JSON](/images/gatling2/har/www.ubuntu.com.har) ou [XML](/images/gatling2/har/www.ubuntu.com.xml).

On y trouve les différentes requêtes/réponses échangées entre le navigateur et un serveur web.


## Et Gatling 2 dans tout ça ?

Gatling permet de créer une simulation à partir d'un fichier HAR via le "recorder". Par contre, n'essayeZ pas d'importer le format XML généré par IE, ça ne fonctionne pas et je n'envoudrai pas le recorder pour cela...:)

Pour importer un fichier HAR :

* Lancer le recorder ([GATLING_HOME]/bin/recorder)
* Sélectionner le mode "HAR Converter"
* Saisir/sélectionner le fichier HAR à importer
* Remplir les autres informations (nom de la simulation, package, répertoire de sortie)
* Cliquer sur "start"

{% img center /images/gatling2/har/gatling2_har_recorder_import.png %}

Une simulation est alors créée dans le répertoire de sortie spécifié.

Il ne reste plus qu'à faire un peu de refactoring.

## Les avantages
Cette fonctionnalité facilite l'utilisation du recorder. En effet :

* Il n'est plus nécessaire de configurer un proxy Http
* L'enregistrement du scénario se fait avec des outils que nous avons l'habitude d'utiliser (Chrome pour moi, Firebug pour d'autres)
* Il est possible de récupérer des scénarios d'autres outils comme sélénium en les jouant dans un navigateur qui permet un export HAR

## Les inconvénients
Il y a tout de même quelques inconvénients par rapport au mode "HTTP Proxy" :

* On ne peut plus ajouter des "tags" qui permettent d'introduire des commentaires pour bien séparer les différentes étapes du scénario
* Le script généré peut comporter des erreurs de compilation (facile à résoudre). Le cas que j'ai systématiquement avec Gatling 2.0.0-M2 concerne les entêtes avec la propriété "If-None-Match" qui peut se présenter comme suit dans le fichier HAR :

``` javascript
{
    "name": "If-None-Match",
    "value": "\"38394ec8941af055543888383adadc2f\""
}
```
Sa traduction en script donne :
``` scala
val headers_4 = Map(
		...
		"If-None-Match" -> ""38394ec8941af055543888383adadc2f"")
```
Pour corriger l'erreur, on peut procéder comme suit :
``` scala
val headers_4 = Map(
		...
		"If-None-Match" -> """38394ec8941af055543888383adadc2f""")
```
Ce bug est déjà corrigé dans la version en cours de développement.

## Mon avis
La possibilité d'importer des fichiers HAR est une fonctionnalité intéressante et je compte désormais la privilégier dans mon utilisation du recorder.

## Liens utiles

* [La liste des outils supportant le format HAR](http://www.softwareishard.com/blog/har-adopters/)
* [Les spécifications HAR v1.2](http://www.softwareishard.com/blog/har-12-spec/)
* [Télécharger Gatling 2](http://gatling-tool.org/)
* [Toutes les nouveautés Gatling 2](https://github.com/excilys/gatling/wiki/Gatling-2)