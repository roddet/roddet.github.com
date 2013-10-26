---
layout: post
title: "Devoxx France 2013 # Lab # Gatling Crash Course"
date: 2013-03-30 20:00
comments: true
categories: devoxxfr2013 gatling
---

Vous pouvez retrouvez la description de la session [sur le site de devoxx](http://www.devoxx.com/display/FR13/Gatling+Crash+Course).

{% img center http://blog.roddet.com/images/devoxx_gatling/slide.jpg %}

## Les animateurs


{% img center http://blog.roddet.com/images/devoxx_gatling/stephane_pierre.jpg %}

* Pierre Dal-pra (à gauche) [@pierre_dalpra](https://twitter.com/pierre_dalpra) / [Bio](http://www.devoxx.com/display/FR13/Pierre+Dal-pra)
* Stéphane Landelle (à droite) [@slandelle](https://twitter.com/slandelle) / [Bio](http://www.devoxx.com/display/FR13/Stephane+Landelle)


## Pourquoi j'ai choisi cette session ?
[Gatling](http://gatling-tool.org/) est un outil permettant de faire des tests de charge à base de scripts écrits en scala. Je l'avais découvert l'année dernière et j'avais écrit un article à ce sujet [ici](/2012/05/gatling-integration-maven-eclipse/). C'est donc naturellement que j'ai choisi d'être présent au Lab "Gatling Crash Course" pour expérimenter les nouveautés de l'outil.


## Démarrage de la session
10 min avant le début du lab, les organisateurs de Devoxx procède à un changement de salle car le lab Java EE 7 rencontre un franc succès.
Nous sommes donc emmener dans une salle de 24 places qui sera comble.

Les intervenants nous ont fournis une clé USB contenant :

* Une VM (Virtual Box)
* Les archives d'installation de Virtual Box pour Linux, Mac et Windows
* La version 1.4.6 de Gatling sortie la veille
* Une archive de PlayFramework 2.1.0
* Une archive gatling-cheat-sheet (une documentation technique de l'API Gatling)

Certains participants ont eu des difficultés d'installation de Virtual Box ou de configuration de la VM... Ils étaient majoritairement sous Windows, on conviendra qu'il s'agit d'une coincidence :)

## C'est quoi Gatling ?
La session a commencé par une présentation de Gatling par Stéphane Landelle.
Il définit "Gatling" comme un injecteur de charge.

## Les différentes familles d'injecteur ?
Il y a évidemment des solutions concurrentes dans ce créneau parmi lesquelles :

* [JMeter](http://jmeter.apache.org/)
* [The Grinder](http://grinder.sourceforge.net/)
* [Tsung](http://grinder.sourceforge.net/)
* [LoadUI](http://www.loadui.org/)
* [LoadRunner](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1175451#.UVcAhasjo08)
* [NeoLoad](http://www.neotys.fr/product/overview-neoload.html)

On peut distinguer 2 familles dans ces outils :

* ceux basés sur des interfaces graphiques : JMeter, LoadUI...
* ceux basés sur l'écriture des scripts : Tsung, The Grinder...

Gatling se positionne quand à lui dans la famille des injecteurs de charge basés sur l'écriture des scripts.

Les animateurs vont citer [Tsung](http://tsung.erlang-projects.org/) comme outil proche de Gatling dans sa philosophie. L'inconvénient de Tsung est l'utilisation du langage Erlang qui n'est pas très intuitif pour les développeurs venant du monde Java.


## Le problème de performance des certains concurrents

La plupart des solutions concurrentes nécessitent beaucoup de ressources pour exécuter des tests de charge. Il est arrivé à Stéphane par exemple d'avoir à monter un cluster d'injecteurs de taille comparable à celle de l'application en production.

Le manque de performance de certains de ces outils est dû au modèle : 
1 user = 1 thread (ce choix permet de gérer plus facilement la concurrence avec par exemple à l'utilisation des ThreadLocal).
Seulement 1 thread c'est couteux et l'orchestratreur doit jongler entre les threads pour accorder du CPU.

Certaines solutions s'appuient sur des I/0 bloquants. Cela implique que 
lorsqu'une requête est envoyée à un serveur, le thread attend la réponse sans faire autre chose. L'utilisation des ressources n'est ainsi pas optimisée.

## Scripts vs Interface graphique
Les animateurs définissent le travail de testeur de charge comme étant un "job de développeur". Et surtout, pour avoir une pertinence suffisante, ce développeur doit avoir participé au développement de l'application à tester.

Avec une interface graphique, le développeur est vite limité dans l'utilisation de l'outil.

Avoir la main sur le script permet d'avoir une meilleure maintenabilité des tests. En effet, les injecteurs à interface graphique propose généralement des exports de configuration (plusieurs MB en XML à stocker sur le disque) plus difficile à maintenir et à utiliser à plusieurs.

## Pourquoi Gatling est performant ?
Gatling doit sa performance à 2 volets :

* des requêtes non bloquantes grâce à [Async HTTP Client](https://github.com/AsyncHttpClient/async-http-client) utilisant le moteur [Netty](http://netty.io/)
* des traitements parallèles en se basant sur le modèle des acteurs et [Akka](http://akka.io/).

## Le DSL Gatling
Gatling expose un DSL riche et facile à prendre en main.

Vous pouvez consulter les éléments du DSL dans le [Gatling's Cheat Sheet](http://gatling-tool.org/cheat-sheet/).

## Le lab
La session a consister à effectuer un test de charge sur une des applications exemple contenu dans l'archive PlayFramework 2.1.0 : computer-database.

{% img center http://blog.roddet.com/images/devoxx_gatling/play2app.jpg %}

### Etape 1 : Démarrer l'application à tester

Se placer dans le répertoire "play-2.1.0/samples/scala/computer-database" et lancer la commande :
```
play
```
Puis démarrer l'application.

```
start -DapplyEvolutions.default=true
```

"-DapplyEvolutions.default=true" permet d'appliquer les modifications de base de données au démarrage de l'application.

A ce stade pour certains participants, play demandait à télécharger des packages. Il me semblait pourtant que l'archive play étant autoportante. Je n'ai pas eu le problème.

### Etape 2 : Enregistrement d'un scénario grâce au "recorder"
Pour éviter de partir de 0 lors de l'écriture d'un script, Gatling vient avec un recorder (enregistreur de scénario). Il est accessible via le lancement de l'exécutable "GATLING_HOME/bin/recorder.sh" (remplacer .sh par .bat pour Windows).

{% img center http://blog.roddet.com/images/devoxx_gatling/recorder.jpg %}

Nous avons configuré le recorder pour écarter les ressources statiques (.css, .js, .ico) pour ne tester que la partie dynamique de l'application.

Il faut configurer le proxy de votre navigateur avec les informations saisies dans la partie Network du recorder.

Le scénario enregistré :

* Recherche d'un ordinateur
* Consultation d'un ordinateur
* Navigation à travers les différentes pages de résultats
* Création d'un ordinateur

A savoir sur le recorder : il n'est pas possible de poursuivre un scénario enregistré précedemment. Chaque enregistrement génère un fichier complet.

Durant l'enregistrement, il est possible d'ajouter des "tags" qui vont se traduire en commentaire dans le script généré.

### Etape 3 : Configuration du WarmUp
Au démarrage de l'exécution d'un scénario, Gatling envoie une première requete (warmup) pour charger les classes, le moteur TCP etc...
On peut désactiver cette requête ou changer l'url. Dans le cadre de ce lab, nous avons changer l'URL en http://localhost:9000.

``` scala
  val httpConf = httpConfig
    .baseURL("http://localhost:9000")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8")
    .acceptCharsetHeader("ISO-8859-1,utf-8;q=0.7,*;q=0.3")
    .acceptEncodingHeader("gzip,deflate,sdch")
    .acceptLanguageHeader("en-US,en;q=0.8")
    .warmUp("http://localhost:9000")
```

Attention à ne pas supprimer les headers. Cela peut avoir une influence le réseau (taille des requêtes inférieure à la réalité) et le comportement de l'application peut être différent.

Gatling vérifie automatiquement pour chaque requête que le code de la réponse est compris entre  200 à 210.

### Etape 4 : Configuration d'une source de données
Il est important de faire varier ses données pour avoir des tests fiables. Gatling permet de configurer une source de données grâce à la notion de feeder.

Les fichiers de données sont des fichiers CSV à placer dans le répertoire "GATLING_HOME/user-files/data".

Exemple de fichier CSV
```
searchCriterion,searchComputerName
Macbook,MacBook Pro
eee, ASUS Eee PC 1005PE
```
La première ligne représente l'entête du fichier.

Créer un feeder :

``` scala
  val feeder = csv("search.csv")
```

Par défaut le feeder est une queue, le scénario consomme les données (lignes) les unes après les autres. Et si toute la pile est consommée et que le scénario souhaite une donnée supplémentaire, une erreur est lancée.

Il est possible de configurer le feeder pour la consommation des données soient :

* aléatoire
``` scala
val feeder = csv("search.csv").random
```

* circulaire (après la consommation de la dernière ligne, on reprend la première)
``` scala
val feeder = csv("search.csv").circular
```

Pour attacher un feeder à une exécution :
``` scala
exec(http("Home")
    .get("/"))
    .pause(1)
    .feed(feeder)

```
Il est possible d'appliquer une transformation au feeder pour formater les données.

Voici un exemple d'utilisation des valeurs contenues dans le feeder

``` scala

exec(http("Home")
  .get("/"))
  .pause(1)
  .feed(feeder)
  .exec(http("Search")
    .get("/computers")
    .queryParam("""f""", """${searchCriterion}""")
    .check(regex("""<a href="([^"]+)">${searchComputerName}</a>""").saveAs("url"))
  .pause(1)
  .exec(http("Select")
    .get("${url}").check(Status.is(200)))
  .pause(1)

```
${searchCriterion} représente la première colonne du fichier CSV.

${searchComputerName} représente la seconde colonne

La méthode .saveAs permet de sauvegarder dans une variable un résultat.

Le fichier CSV est chargée intégralement en mémoire au démarrage de l'exécution des tests.

### Etape 5 : Factorisation
Un script Gatling n'est pas autre chose qu'un programme Scala. On peut donc aisément créer des méthodes et factoriser les opérations que l'on fait plusieurs fois.

Exemple de factorisation : 
```
 def gotoPage(page: String) = exec(http("Page " + page)
    .get("/computers")
    .queryParam("""p""", page))
    .pause(1)
  
  val browse = exec(gotoPage("0"),gotoPage("1"),gotoPage("2"),gotoPage("3"))
```

Il est aussi possible d'utiliser des fonctions avancées comme "repeat" pour répéter des traitements.
``` scala
val browse = repeat(5, "i") {
    gotoPage("${i}")
  }
```

### Etape 6 : Activation de graphite
Pour activer le suivi de l'exécution avec Graphite, il faut modifier le fichier de configuration "GATLING_HOME/conf/gatling.conf"

gatling.conf

``` scala GATLING_HOME/conf/gatling.conf
gatling {
  data {
    writers = [console, file, graphite]
    #reader = file
  }
  graphite {
    host = "localhost"
    port = 2003
    rootPathPrefix = "gatling"
    bucketWidth = 100
  }
```

### Etape 7 : Le script complet

``` scala ComputersDatabase.scala
package devoxx.labs
import com.excilys.ebi.gatling.core.Predef._
import com.excilys.ebi.gatling.http.Predef._
import com.excilys.ebi.gatling.jdbc.Predef._
import com.excilys.ebi.gatling.http.Headers.Names._
import akka.util.duration._
import bootstrap._
import assertions._

object Search {
  
  val feeder = csv("search.csv").circular
  
  val search = exec(http("Home")
    .get("/"))
    .pause(1)
    .feed(feeder)
    .exec(http("Search")
      .get("/computers")
      .queryParam("""f""", """${searchCriterion}""")
      .check(regex("""<a href="([^"]+)">${searchComputerName}</a>""").saveAs("url")))
    .pause(1)
    .exec(http("Select")
      .get("${url}").check(status.is(200)))
    .pause(1)
}

object Browse {
  
  def gotoPage(page: String) = exec(http("Page " + page)
    .get("/computers")
    .queryParam("""p""", page))
    .pause(1)
    
  val browse = repeat(5, "i") {
    gotoPage("${i}")
  }
 
}

object Edit {

  val head = Map(
    "Cache-Control" -> """max-age=0""",
    "Content-Type" -> """application/x-www-form-urlencoded""",
    "Origin" -> """http://localhost:9000""")
    
  val random = new java.util.Random

    // Je reessai 2 fois en cas d'erreur
  val edit = tryMax(2){
    exec(http("New Computer") .get("/computers/new"))
    .pause(11)
    .exec(http("Post New Computer")
      .post("/computers")
      .headers(head)
      .param("""name""", """JCertif Mac"""+ random.nextInt(2))
      .param("""introduced""", """2013-03-26""")
      .param("""discontinued""", """""")
      .param("""company""", """13"""));
  }.exitHereIfFailed // l'utilisateur quitte le workflow du scenario
  
   
}

class ComputersDatabase extends Simulation {

  val httpConf = httpConfig
    .baseURL("http://localhost:9000")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8")
    .acceptCharsetHeader("ISO-8859-1,utf-8;q=0.7,*;q=0.3")
    .acceptEncodingHeader("gzip,deflate,sdch")
    .acceptLanguageHeader("en-US,en;q=0.8")
    .warmUp("http://localhost:9000")
    .userAgentHeader("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_3) AppleWebKit/537.31 (KHTML, like Gecko) Chrome/26.0.1410.43 Safari/537.31")

  val admins = scenario("Admins").exec(Search.search, Browse.browse, Edit.edit)
  val users = scenario("Users").exec(Search.search, Browse.browse)

  setUp(users.users(1000).ramp(20).protocolConfig(httpConf), admins.users(10).ramp(20).protocolConfig(httpConf))
}
```

### Etape 8 : Lancement du script
Il faut lancer l'exécutable "GATLING_HOME/bin/gatling.sh" (ou .bat) puis choisir le scénario à exécuter.

```
>./gatling.sh
GATLING_HOME is set to .../gatling-charts-highcharts-1.4.6
Choose a simulation number:
     [0] advanced.AdvancedExampleSimulation
     [1] basic.BasicExampleSimulation
     [2] devoxx.labs.ComputersDatabase
```
Faire le choix 2.

Le rapport est généré dans le répertoire "GATLING_HOME/results".

{% img center http://blog.roddet.com/images/devoxx_gatling/report.jpg %}


Exemple de rapport généré par Gatling : [ici](http://gatling-tool.org/sample/index.html)


## Intégration de Gatling avec d'autres outils
Gatling s'intègre avec les outils :

* Maven (possibilité d'utiliser un archetype ou de gérer le script dans un projet scala)
* [Un plugin pour Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Gatling+Plugin)
* [Graphite](http://graphite.wikidot.com/) pour un suivi en temps réel de l'exécution des tests.

## Trucs & Astuces pour des tests de performances avec Gatling

* Si l'application est accessible en HTTPS, il faut désenregistrer le certificat dans le navigateur pour éviter que celui-ci ne pense que Gatling est en train de hacker le site.
* Il faut rendre dynamique de la données pour éviter les caches, les optimisations de la JVM...
* Donner le recorder aux utilisateurs finaux pour enregistrer plus finement les temps de pause. Il est important de ne pas supprimer les temps de pause car vos utilisateurs ne sont pas des robots et font des pauses entre les actions.

## La roadmap à court terme
Dans 1 mois, nous devrions avoir la 1ère milestone de la version 2 de Gatling. 

Au menu :

* Support des Websockets
* Support de JDBC
* Possibilité de faire du clustering (surtout pour palier à l'éventuelle saturation de la carte réseau)
* Possibilité de générer des ramps en escalier
* Possibilité de choisir entre TCP & UDP pour envoyer les informations à Graphite

## Bilan
Ce lab était très instructif. On peut regretter que nous ne soyons pas aller au bout du lab avec la visualisation du déroulement des tests avec Graphite.

