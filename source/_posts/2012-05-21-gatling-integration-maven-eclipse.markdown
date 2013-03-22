---
layout: post
title: "Gatling : Intégration Maven & Eclipse"
date: 2012-05-21 22:15
comments: true
categories: gatling scala maven eclipse
---

![/images/gatling/logo.png](/images/gatling/logo.png)

[Gatling](http://gatling-tool.org/) permet d'effectuer des tests de charges de vos applications à base de scripts écrits en Scala.
Le nom Gatling fait référence à une mitrailleuse créée en 1861, plus de détail sur [Wikipedia](http://fr.wikipedia.org/wiki/Gatling). Vous l'avez compris l'objectif ici est de tirer à plein feux sur votre application.

L'approche utilisée peut se résumer en deux concepts : asynchrone à l'extrême et un DSL pour écrire les scripts.

## Asynchrone à l'extrême
La plupart des autres solutions sont construites sur le modèle 1 utilisateur = 1 thread. Lors d'une requête HTTP, le thread est mis en attente en attendant la réponse. Avec un grand nombre de threads (donc d'utilisateurs), le processeur passe beaucoup de temps à changer de contexte pour trouver le thread qui n'est pas en attente.

Le projet s'est construit autour du [modèle Actor](http://en.wikipedia.org/wiki/Actor_model) qui revient à la mode depuis que les fréquences des processeurs ont cessé d'augmenter et que l'on s'oriente plutôt vers la multiplication des coeurs.
Il permet de profiter pleinement la puissance du processeur.

L'unité de base du concept est un acteur et les acteurs communiquent entre eux par le biais de messages. Il faut donc voir un acteur comme une unité de traitement avec deux types d'interactions : une réception de message et un envoi de message. Gatling ne limite plus l'utilisation d'1 thread à un utilisateur. Après l'envoi d'une requête HTTP,  un message est posté et le thread est immédiatement disponible pour envoyer une autre. Un autre thread pourra traiter la réponse dans la pile des messages reçus. [Akka](http://akka.io/) est le framework open source utilisé pour implémenter ce pattern. Ce choix est de plus en plus adopté par des projets émergeants ([Play framework](http://www.playframework.com/), [Kandash](https://code.google.com/p/kandash/), ...).

Gatling effectue des appels HTTP non bloquant à l'aide du framework [Async Http Client](https://github.com/AsyncHttpClient/async-http-client) basé sur le client [Netty](http://netty.io/). Là encore on peut remarquer la similitude avec Play framework qui utilise également Netty, cette fois-ci côté serveur.
En combinant le modèle des acteurs et les appels Http non bloquants, Gatling met toutes les chances de son côté pour avoir de très bonnes performances et ça se voit !

## Un DSL pour écrire les scripts
L'objectif de Gatling est de sortir du modèle d'outil de réalisation de script à partir d'une interface graphique pour s'opposer à des outils comme [Apache JMeter](http://jmeter.apache.org/) et aller dans le sens des frameworks comme [The Grinder](http://grinder.sourceforge.net/). Le langage choisi pour l'écriture des scripts est Scala. Pas de panique, pas besoin de connaître ou de maîtriser ce langage, l'api fournie est "humainement" compréhensible, simple, concis.

En voici un exemple, je vous laisse juger par vous même :

``` scala  
scenario("Mon scénario à moi tout seul")  
     .exec(  
      http("requette_mapage1")  
      .get("/faces/mapage1.html")  
     )  
     .exec(  
      http("requette_mapage2")  
      .get("/faces/mapage2.html")  
      .queryParam("nom", """rossi""")  
      .check(status.is(304)  
     )  
```
Le fait d'utiliser un langage fortement typé comme Scala permet de détecter les erreurs de syntaxes à la compilation et d'utiliser toute la puissance de votre IDE pour organiser le code, mutualiser des bouts de script, etc...
Pour les développeurs Java, l'avantage de Scala est de pouvoir directement accéder aux API du JDK et aux différentes bibliothèques java une fois ajoutée au classpath.


## Installer Gatling
Pour une installation/utilisation classique, vous pouvez suivre les instructions de la page : [https://github.com/excilys/gatling/wiki/Getting-Started](https://github.com/excilys/gatling/wiki/Getting-Started). Il suffit de télécharger puis décompresser le livrable. Vous pourrez ainsi écrire et exécuter vos scripts en vous basant sur les exemples fournies.

Le lien suivant explique comment enregistrer un scénario et l'exécuter : [https://github.com/excilys/gatling/wiki/First-Steps-with-Gatling](https://github.com/excilys/gatling/wiki/First-Steps-with-Gatling).
Dans cet article, nous allons plutôt voir comment il s'intègre dans votre IDE (Eclipse) et dans le cycle de vie de vos projets (Maven). Il serait dommage de ne pas profiter de la puissance de votre IDE pour accélérer l'écriture et la maintenance de vos scripts.


## Installer vos outils de développement
Voici ce dont vous avez besoin, je mets en parenthèse les versions que j'ai utilisé :

* un JDK 6+ (1.6.0_31)
* [Eclipse](http://www.eclipse.org/downloads/) (Eclipse Classic 3.7.2)
* [Scala IDE Plugin](http://scala-ide.org/) ([http://download.scala-ide.org/releases-29/stable/site/](http://download.scala-ide.org/releases-29/stable/site/))
* [M2Eclipse](http://www.eclipse.org/m2e/) (Indigo Update Site > Collaboration > m2e)
* [M2E-Scala](http://alchim31.free.fr/m2e-scala/update-site)

J'ai dû repartir d'une version d'Eclipse "neuve" car le Scala IDE Plugin ne s'installait pas sur ma version d'Eclipse courante contenant très grand nombre de plugins installés.


## Créer le projet Maven

### Ajouter le catalog gatling 
Nous allons pour cela utiliser l'archetype "gatling-highcharts-maven-archetype". Cet archetype n'étant pas présent dans le repository maven central, il faut ajouter un nouveau catalog à eclipse.

Allez dans le menu :
Window > Preferences > Maven > Archetypes > Add Remote Catalog...
Puis saisissez l'url : [http://repository.excilys.com/content/groups/public/archetype-catalog.xml](http://repository.excilys.com/content/groups/public/archetype-catalog.xml)

![/images/gatling/eclipse_maven_catalog.png](/images/gatling/eclipse_maven_catalog.png)

### Créer un projet maven à partir de l'archetype gatling
File > New > Maven Project > Sélectionner l'archetype "gatling-highcharts-maven-archetype" (version 1.1.6 dans mon cas)

![/images/gatling/eclipse_maven_archetype.png](/images/gatling/eclipse_maven_archetype.png)


## Anatomie du projet généré

<table>
  <tr>
    <td width="30%"><img src="{{root_url}}/images/gatling/structure_mvn.png"/></td><td>&nbsp;&nbsp;</td>
    <td width="70%">On retrouve un projet Maven classique avec un pom.xml, les sources scala dans src/main/scala, les fichiers de configuration dans src/main/resources.<br/><br/>Engine.scala est la classe principale qui lance la mitrailleuse.<br/><br/>IDEPathHelper.scala contient les différents chemins d'accès aux ressources comme les classes compilées, les simulations, les fichiers de données, etc...<br/><br/>Recorder.scala est un utilitaire qui permet d'enregistrer un scénario avec par exemple votre navigateur et de générer les scripts correspondants.<br/><br/>FooSimulation.scala une simulation qui ne fait rien. Il peut être compléter pour créer son premier scénario.<br/><br/>
gatling.conf donne la possibilité de paramétrer gatling.</td>
</table>

## Générer une simulation avec le "Recorder"
Le "Recorder" permet de capturer toutes les requêtes HTTP en se positionnant en proxy entre votre machine et votre réseau. Le cas d'utilisation le plus courant est celui d'enregistrer une simulation à partir d'un navigateur web en y paramétrant un proxy.

Pour lancer l'outil "Recorder" :

* Sélectionner le fichier "Recorder.scala"
* Clic droit > Run As > Scala Application

![/images/gatling/gatling_run_recorder.png](/images/gatling/gatling_run_recorder.png)

L'écran suivant se lance :

![/images/gatling/recorder.png](/images/gatling/recorder.png)

La partie "Network/Listening Network" sert à renseigner les ports d'écoute qui seront à paramétrer dans l'outil utilisé pour réaliser un scénario (un navigateur web par exemple). Je ne parle pas intentionnellement directement d'un navigateur web parce que vous allez pouvoir utiliser le Recorder avec n'importe quel programme qui fait des requêtes HTTP, une compilation Maven par exemple.

Dans le cas présent, le proxy à paramétrer sera :
* Pour du HTTP locahost:8000
* Pour du HTTPS localhost:8001

La partie "Output" sert à paramétrer l'endroit où sera généré le script.

Commencer par paramétrer l'outil qui va vous aider avec le proxy configuré. Si votre outil possède un cache, comme un navigateur web, à vous de voir si vous voulez exécuter votre simulation dans ces conditions ou vider le cache.

Vous pouvez ensuite cliquer sur le bouton "Start".

__Attention petit bug déroutant sur Mac OSX Lion__ : l'affichage par défaut du Recorder ne permet pas de voir le bouton "Start". J'ai mis un certain temps à pouvoir trouver l'astuce pour l'afficher. Il consiste à simplement agrandir la fenêtre avec le bouton "+" avec comme conséquence la disparition de quelques rubriques de la partie "Output". Ce bug est déjà corrigé dans la version 1.2.0-SNAPSHOT en cours de développement.

Vous êtes désormais en mode écoute sur les ports configurés pour votre machine. Si vous faites par exemple la configuration du proxy au niveau de votre connexion Wifi, vous allez pouvoir voir toutes les requêtes et réponses qui transitent entre votre outil et votre réseau.

Exécutons un scénario simple :

1. Taper l'adresse "www.google.fr"
2. Lancer la recherche "JCertif 2012"

![/images/gatling/capture_recorder.png](/images/gatling/capture_recorder.png)

Le premier constat que vous allez voir est que vous n'avez pas seulement 2 requêtes mais beaucoup plus. Et oui c'est ça la vraie vie, google fait de multiple appels notamment pour la complétion lors de la saisie entre autres.
Quand on réalise des tests de charge, on se rend rapidement compte qu'un scénario pourtant fonctionnellement simple peut rapidement se complexifier.
Le Recorder permet d'ajouter des TAG, c'est particulièrement utile pour séparer les différentes phases de l'enregistrement. Concrètement, les TAG ajoutés vont se transformer en commentaire entre deux enchainements de requête dans le code Scala généré.

Pour arrêter l'enregistrement cliquer sur "Stop !".

De retour dans Eclipse, il faut rafraichir le projet pour voir apparaitre le fichier source généré.

![/images/gatling/fichier_genere.png](/images/gatling/fichier_genere.png)

Dans mon cas, il commence par "Simulation" suivi d'une indication sur la date/heure d'enregistrement.

Vous pouvez le parcourir vous verrez qu'il se lit aisément malgré la complexité généré. Le code généré est fidèle à tout ce qui s'est passé sans masquer les informations.

A cette étape, on se retrouve avec du code dans un IDE, on peut faire tout ce que l'on veut : refactoring, suppression des appels à des serveurs que l'on ne souhaite pas tester, suppression des pauses, ajout de requêtes, variabilisation de chaine de caractères, ...

Dans mon cas, je vais juste renommer la classe en "SimulationGoogleJCertif" en utilisant la fonction "Refactor" d'Eclipse. Pour les développeurs venant du monde Java, vous allez remarquer que le nom du fichier va être modifié mais pas le nom de la classe. Il ne s'agit pas d'un bug (je l'ai pensé au début, honte à moi...:-)), c'est autorisé en Scala ! Mais c'est pas grave, je modifie manuellement le nom de la classe. Il faut aussi savoir que le support de Scala dans Eclipse n'a pas autant de maturité que celui de Java. Vous allez probablement comme moi rencontrer quelques bugs, mais Eclipse nous a déjà habitué aux bugs et aux plantages, donc ça ne change pas fondamentalement nos habitudes...:-)

## Exécuter votre simulation depuis Eclipse
Pour exécuter votre simulation depuis Eclipse rien de plus simple, il faut lancer la classe Engine.scala de la même manière que le Recorder.

Le moteur Gatling se lance en mode interactif :

![/images/gatling/choix_simulation.png](/images/gatling/choix_simulation.png)

Il faut à présent suivre les instructions :

* Choisir la simulation : 1 dans mon cas
* Saisir un identifiant de cette exécution si besoin, taper sur la touche Entrée sinon
* Saisir une description si besoin, taper sur la touche Entrée sinon

C'est parti !

## Visualiser les résultats

Le résultat de la simulation est enregistré dans /target/gatling-results.

![/images/gatling/fichier_resultat.png](/images/gatling/fichier_resultat.png)

Le fichier le plus important est simulation.log, il contient les mesures des temps d'exécution. Il est aisément parsable pour générer des rapports.

Sinon vous avez un rapport sympa généré par gatling. Pour le visualiser, ouvrer le fichier "active_sessions.html" dans un navigateur web.

![/images/gatling/graphe1.png](/images/gatling/graphe1.png)
<br/>
![/images/gatling/graphe2.png](/images/gatling/graphe2.png)


## Gatling avec Maven en Intégration Continue
Nous avons pu jusqu'ici généré un projet Maven qui lors d'un mvn install compile le code Scala. Tant qu'à faire, il est plus intéressant de pouvoir les lancer automatiquement. Deux intérêts possibles :

* vous disposez d'un environnement pour faire vos tests de charges, vous pouvez alors simuler un grand nombre d'utilisateurs et détecter les regressions en terme de performance. C'est bien sûr le cas idéal.
* vous êtes dans la vrai vie, vous n'avez pas d'environnement dédié. Vous pouvez exécuter vos scripts au quotidien pour vous assurer qu'ils sont toujours valides. En effet, les modifications de votre application peuvent entrainer des changements d'url ou de paramètre, vous les détecterait en regardant les rapports.

Pour exécuter Gatling depuis Maven, il y a plusieurs options :

* Utiliser gatling-maven-plugin : le guide d'utilisation est là [https://github.com/excilys/gatling/wiki/Maven-plugin](https://github.com/excilys/gatling/wiki/Maven-plugin)
* Utiliser [exec-maven-plugin](http://mojo.codehaus.org/exec-maven-plugin/) pour exécuter la classe Engine juste après sa compilation. En effet, la compilation du code Scala va générer du bytecode que l'on sait exécuter simplement avec Java. Il  est surement possible d'utiliser directement le plugin [scala-maven-plugin](https://github.com/davidB/scala-maven-plugin), je n'ai pas testé.

Actuellement, je penche plutôt sur l'option [exec-maven-plugin](http://mojo.codehaus.org/exec-maven-plugin/) pour les raisons suivantes :

* Le modèle par défaut de gatling-maven-plugin préconise de mettre les sources dans src/main/resources/simulations. Ce qui ne me parait pas logique avec le reste de l'architecture de mon projet où je mets mes sources Scala dans src/main/scala. Il est possible de configurer le plugin pour modifier le répertoire des simulations. Cela n'a pas fonctionner du premier coup dans mon cas et je n'ai pas non plus insister (c'est peut-être une erreur de ma part, à suivre...)
* J'aime bien l'idée d'unifier le lancement de Gatling que ce soit via Eclipse ou via Maven. Cela permet par exemple de modifier la classe Engine pour ajouter des paramètres à Gatling, ou faire des traitements avant de lancer Gatling (récupérer des infos d'une base de données par exemple, etc...).

Avec cette option, il y a un petit soucis en l'état. En effet, on a vu tout à l'heure que Engine lançait Gatling en mode intéractif. Hors durant notre compilation Maven nous n'aurons personne pour faire les différents choix. Heureusement nous avons accès à tout le code et nous pouvons faire de petites modifications pour modifier ce comportement, c'est aussi un peu notre métier...:-) C'est du Scala certes, ça pique aux yeux de certains, mais on s'y habitue en attendant Java 8 !

Suivez les étapes suivantes pour la mise en oeuvre.

### Modifier le fichier IDEPathHelper.scala


``` scala
import scala.tools.nsc.io.File
import scala.tools.nsc.io.Path
import scala.collection.immutable.List

object IDEPathHelper {
  val gatlingConfUrl = getClass.getClassLoader.getResource("gatling.conf").getPath
  val projectRootDir = File(gatlingConfUrl).parents(2)
  val mavenSourcesDir = projectRootDir / "src" / "main" / "scala"
  val mavenResourcesDir = projectRootDir / "src" / "main" / "resources"
  val mavenTargetDir = projectRootDir / "target"
  val mavenBinariesDir = mavenTargetDir / "classes"
  val dataFolder = mavenResourcesDir / "data"
  val requestBodiesFolder = mavenResourcesDir / "request-bodies"
  val recorderOutputFolder = mavenSourcesDir
  val resultsFolder = mavenTargetDir / "gatling-results"
  
  val simulations:List[String] = List.fromString("com.roddet.gatling.SimulationGoogleJCertif",',')
}
```
Cette variable va nous permettre de paramétrer la liste des simulations que l'on souhaite lancer.

### Modifier le fichier Engine.scala

```scala
import com.excilys.ebi.gatling.app.{ Options, Gatling }
import com.excilys.ebi.gatling.core.util.PathHelper.path2string

object Engine extends App {

  new Gatling(Options(
    dataFolder = Some(IDEPathHelper.dataFolder),
    resultsFolder = Some(IDEPathHelper.resultsFolder),
    requestBodiesFolder = Some(IDEPathHelper.requestBodiesFolder),
    simulationBinariesFolder = Some(IDEPathHelper.mavenBinariesDir),
    simulations = Some(IDEPathHelper.simulations))).start
}
```

### Configurer le fichier pom.xml

``` xml
<build>
   ....
    <plugins>
      <plugin>
        <groupId>net.alchim31.maven</groupId>
        <artifactId>scala-maven-plugin</artifactId>
   ....
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.1</version>
        <configuration>
          <mainClass>Engine</mainClass>
        </configuration>
        <executions>
          <execution>
            <phase>integration-test</phase>
            <goals>
              <goal>java</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```
Ca y est que vous lanciez Engine depuis l'IDE ou via Maven, vous avez les simulations définies dans IDEPathHelper.scala qui sont exécutées sans utilisation du mode intéractif.

## Petit Bilan
Gatling est simple à utiliser. Il s'intègre aisément dans le cycle de vie de vos projets existants et dans vos IDE préférés. Le fait d'utiliser du code pour les scripts permet de d'éliminer à la compilation les problèmes de syntaxe, d'avoir la main sur la personnalisation des scripts et l'organisation du code.

L'autre avantage énorme est qu'il consomme peu de ressources (CPU, RAM), on peut simuler un très grand nombre d'utilisateurs même avec un pc pas très performant.
Voici quelques limites (il en faut bien...) :

* Gatling ne supporte pour le moment que le protocole HTTP/HTTPS
* Scala n'est pas Java surtout dans Eclipse. Le plugin Scala-Ide n'est pas aussi mature que le support de Java.
* On peut reprocher au projet sa jeunesse mais je pense qu'en continuant sur cette lancée il y a des chances que l'on en entende de plus en plus parlé de ce projet.
* Il n'y a pas de consolidation des mesures avec d'éventuelles données côtés serveurs. il faut tout faire à la main pour relier des événements serveurs aux événements clients.
* gatling-maven-plugin ne propose pas pour le moment beaucoup d'options (uniquement l'exécution des scripts). Il serait intéressant par exemple d'avoir un paramétrage qui fait échouer le build maven si le script par en erreur pour l'intégration continue.

Vous l'aurez vu, rien de bien méchant.
Vous pouvez retrouver des liens vers des blogs qui en parlent : [https://github.com/excilys/gatling/wiki/Links](https://github.com/excilys/gatling/wiki/Links).

Les sources de l'exemple sont disponibles dans github : [https://github.com/roddet/gatling-sample](https://github.com/roddet/gatling-sample)


