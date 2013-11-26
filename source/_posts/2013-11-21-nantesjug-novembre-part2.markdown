---
layout: post
title: "Soirée JUG Nantes : Du SQL au NoSQL en moins d'une heure (Partie 2)"
date: 2013-11-21 09:30
comments: true
categories: jugnantes jugnantesnov
---
Vous pouvez retrouvez la première partie de cet article [ici](http://blog.roddet.com/2013/11/nantesjug-novembre-part1/).

Le 04 novembre dernier, [Tugdual Grall](https://twitter.com/tgrall) et [David Pilato](https://twitter.com/dadoonet) ont offert au public nantais une preview d'une session qu'ils allaient donner quelques jours plus tard à [Devoxx Belgique](http://www.devoxx.be/dv13-david-pilato.html?presId=3281). Ils vont montrer, sur la base d'une application exemple, une migration du SQL au monde du NoSQL.

{% img center /images/nantesjug/nov13/nantesjug-nov13-david-pilato-tugdual-grall.jpg %}

## Pourquoi passer du SQL au NoSQL ?
[Tugdual](https://twitter.com/tgrall) et [David](https://twitter.com/dadoonet) vont commencer la session par présenter _LA_ raison qui pourrait vous convaincre de migrer d'une base SQL à une base NoSQL : la scalabilité horizontale.

Scalabilité horizontale sont les mots à la mode pour désigner la caractéristique d'un système capable de supporter une grande charge. Pour augmenter la puissance d'un système, sa stratégie consiste à multiplier le nombre de machines de petites puissances. Elle se positionne en opposition à la scalabilité verticale qui prône l'augmentation des capacités d'une machine pour répondre à des besoins de charge croissants.

La scalabilité horizontale s'impose de plus en plus comme _LA_ solution pour relever le défi des systèmes performants à fort trafic.

Peut-on effectuer la scalabilité horizontale avec une base de données relationnelle ?

La réponse est oui. Ca s'appelle mettre en place un _cluster de base de données_. Tug et David vont demander aux participants combien avaient déjà configuré un _cluster_ de bases de données relationnelles ? Je n'ai vu qu'une seule main levée de là où j'étais assis. Cette opération requiert des compétences assez pointues pour obtenir un système fonctionnel.

Et le NoSQL dans tout ça ?

Tug et David vont promettre que cette opération qui nécessitait la présence d'un administrateur de bases de données très expérimenté va être accessible aux développeurs. Cette promesse sera accompagnée d'une démonstration :

* du passage d'un modèle dit _legacy_ à un modèle moderne _REST_
* du passage d'un modèle SQL à un modèle NoSQL
* de distribution de données sur plusieurs _noeuds_
* de la mise en place d'une recherche _full text_ sur des données distribuées
* de visualisation des données distribuées suivant des axes configurables


L'intégralité des sources de la démonstration est accessible dans le dépôt Github [sql2nosql](https://github.com/dadoonet/sql2nosql/). La version des sources correspondante à une étape est accessible via les branches du dépôt.

A partir de ce dépôt, je vais effectuer toutes les étapes sur ma machine.

Allons y !

## Récupération des sources
Je vais commencer par récupérer le contenu de la branche _01-legacy/start_. 

Pour ceux qui voudraient faire autant et qui ne sont pas familier de Github, voici quelques alternatives :

* Télécharger le Zip des sources de la branche [ici](https://github.com/dadoonet/sql2nosql/archive/01-legacy/start.zip).

* Installer [Git](http://git-scm.com/) et exécuter les commandes 

``` sh
git clone https://github.com/dadoonet/sql2nosql.git
git checkout 01-legacy/start
```

* Installer le client officiel correspondant à votre OS : [Windows](http://windows.github.com/), [Mac](http://mac.github.com/). Pour linux ? Vous connaissez la chanson, si vous êtes sous linux c'est que vous ne jurez que par le _terminal_, faites comme d'habitude utiliser la ligne de commande ;) Il existe des clients graphiques non officiels mais je n'ai rien à vous recommander, j'ai pris l'habitude de la ligne de commande pour Git même si j'utilise un client graphique quand il s'agit de SVN. Il n'y a pas de raison particulière, question d'habitude.

* Installer [Subversion](http://subversion.apache.org/) et exécuter la commande suivante pour récupérer l'intégralité des sources. Vous pouvez aussi passer par des clients graphiques comme [TortoiseSVN](http://tortoisesvn.net/). Les sources se trouveront dans le répertoire _sql2nosql/branches/01-legacy_.

``` sh
svn co https://github.com/dadoonet/sql2nosql
```

* Utiliser votre IDE préféré [Eclipse](http://eclipse.org/), [Netbeans](https://netbeans.org/), [Intellij](http://www.jetbrains.com/idea/)...

## Exécution de l'application
L'architecture de l'application repose sur [Maven](http://maven.apache.org/). J'ai utilisé la version 3.1.1 pour écrire cet article.

Pour exécuter l'application :

* Si vous êtes sous linux ou Mac, lancer le script `run.sh`

* Si vous êtes sous Windows, lancer les commandes :

``` sh
mvn clean install
cd demo-webapp
mvn jetty:run
```

Si l'application ne parvient pas à récupérer le plugin Maven pour Jetty.
Créer/compléter la configuration du fichier `~/.m2/settings.xml` dans la section `pluginGroups` comme ceci :

``` xml
<settings>
  <pluginGroups>
    <pluginGroup>org.eclipse.jetty</pluginGroup>
  </pluginGroups>
</settings>
```

Une fois l'application démarrée, vous pouvez accéder à la page d'accueil via l'adresse : `http://localhost:8080`. Elle affiche une liste de personnes.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-1.png %}

En cliquant sur une personne, vous avez accès aux détails de la personne sélectionnée.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-2.png %}

## L'application _legacy_

Cette application se veut _legacy_. Elle est pilotée par 2 Servlet:

* [HomeServlet](https://github.com/dadoonet/sql2nosql/blob/01-legacy/start/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/HomeServlet.java), gestionnaire de la page d'accueil.
* [PersonServlet](https://github.com/dadoonet/sql2nosql/blob/01-legacy/start/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonServlet.java), pilote de la page de détail d'une personne.

Le mapping URL/servlet configurés dans le fichier [web.xml](https://github.com/dadoonet/sql2nosql/blob/01-legacy/start/demo-webapp/src/main/webapp/WEB-INF/web.xml).

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-3.png %}

Cette application génère l'intégralité de ses pages côté serveur. Les nouvelles générations d'applications Web encouragent :

* un Web avec de plus en plus d'intelligence côté navigateur. 

* un Web qui pense "application web" avec la même phylosophie qu'une application mobile. Une application qui s'installe dans le navigateur.

* un Web qui sépare les univers _frontend_ et _backend_. Il n'est plus question de devoir faire un seul choix technologique pour couvrir l'intégralité de votre besoin. Vous choisissez le meilleur outil pour réaliser votre _frontend_ et vous faites autant pour votre _backend_. Les deux univers communiquent via HTTP, un protocole particulièrement utile pour faire du web ;)

En phase avec ces principes, la prochaine étape va consister transformer l'existant en application _backend_ exposant des services HTTP/REST.

## _RESTification_ de l'application _legacy_
Pour passer à une architecture REST, commençons par quelque chose de facile : se débarrasser des _Servlet_ de l'application. Supprimer :

* Les classes _PersonServlet_ et _HomeServlet_.
* Les fichiers suivants qui ne servent plus à rien `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/ApplicationInitializer.java` et `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonService.java`. 


L'objectif à présent avec être de réaliser des services REST. Voici les services à réaliser :

```
GET 		/api/1/person/	=> Récupérer toutes les personnes

GET 		/api/1/person/{id}	=> Récupérer une personne avec l'identifiant {id}.

PUT 		/api/1/person/	=> Créer une nouvelle personne

PUT 		/api/1/person/{id}	=> Mettre à jour la personne ayant l'identifiant {id}

DELETE	/api/1/person/{id}	=> Supprimer la personne ayant l'identifiant {id}

POST 		/api/1/person/_search => Récupérer des personnes suivant un critière

POST 		/api/1/person/_init => Initialiser la base de données avec des données exemples
```

Les puristes du REST pourront ne pas être d'accord avec cette API (l'utilisation de POST pour effectuer une recherche par critères ou encore l'utilisation de PUT au lieu de POST pour créer une nouvelle personne) mais ce n'est pas le sujet, nous allons nous concentrer sur le NoSQL.

L'application aura l'architecture suivante : 

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-4.png %}

La _RESTification_ sera réalisée avec [Spring MVC](http://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/mvc.html).

J'ai mis à jour les fichiers :

* `pom.xml` à la racine du projet comme [ceci](https://raw.github.com/dadoonet/sql2nosql/02-restify/begin/pom.xml)

* `demo-webapp/pom.xml` comme [ceci](https://raw.github.com/dadoonet/sql2nosql/02-restify/begin/demo-webapp/pom.xml)

* `demo-webapp/src/main/webapp/WEB-INF/web.xml` pour qu'il ressemble à celui là : [web.xml](https://raw.github.com/dadoonet/sql2nosql/02-restify/end/demo-webapp/src/main/webapp/WEB-INF/web.xml).

J'ai créé le fichier `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonService.java` avec le contenu suivant [PersonService.java](https://github.com/dadoonet/sql2nosql/blob/02-restify/end/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonService.java).

Redémarrage de l'application comme précédemment.

Test du service de récupération de toutes les personnes en accédant à la page suivante via un navigateur moderne : `http://localhost:8080/api/1/person/`. J'obtiens :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-4a.png %}

Le résultat `[]` représente un tableau vide.

Pour initialiser la base de données, il faut utiliser le service 

```
POST 		/api/1/person/_init
```

Pour effectuer, une requête avec le verbe HTTP POST, vous avez plusieurs possibilités. Le plus simple en environnement Unix est d'utiliser la commande `curl` comme ceci :

```
curl -XPOST http://localhost:8080/api/1/person/_init
```

Sinon vous pouvez installer des extensions pour vos navigateurs comme par exemple [Simple REST Client](https://chrome.google.com/webstore/detail/simple-rest-client/fhjcajmcbmldlhcimfajhfbgofnpcjmb) pour Chrome ou encore [RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/) pour Firefox.

Une fois les données initialisées, la requête `http://localhost:8080/api/1/person/` donne le résultat suivant :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-4b.png %}

Il s'agit un flux JSON représentant 100 personnes.

Il est possible de tester les autres services développés avec ce même principe avec `curl` ou encore le super outil que vous avez installé.

## Couchbase en action
Maintenant que la partie Web du _backend_ fonctionne, nous allons commencer à quitter le monde SQL en faisant en sorte d'utiliser une base NoSQL. L'heureux élu sera [Couchbase](http://www.couchbase.com/).

[Couchbase](http://www.couchbase.com/) est une base NoSQL orienté document. Il stocke les données sous la forme "clé-valeur". La clé est une chaine de caractères et la valeur est un document JSON sans aucun schéma pré-défini.

L'architecture de l'application va être modifiée comme ceci :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5.png %}

J'ai téléchargé la dernière version (2.2.0) [Couchbase Community Edition](http://www.couchbase.com/download).
Je décompresse l'archive, l'installe puis lance [Couchbase](http://www.couchbase.com/).

J'accède à la console d'administration : `http://127.0.0.1:8091/index.html` :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5a.png %}

Et là, sans aucune autre documentation, je me laisse guider en cliquant sur `SETUP`. 

A la page suivante, j'ai laissé les choix par défaut m'invitant à créer un nouveau cluster. J'ai modifié le paramètre `Per Server RAM Quota` avec la valeur 512 MB au lieu des 3 GB par défaut sur ma machine.

Je laissé les paramètres par défaut et saisis un identifiant/mot de passe administrateur.

Et là, j'ai l'écran suivant :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5b.png %}

J'ai du rouge. En tant que développeur Java, je suis éduqué pour y voir des erreurs. Je me lance alors dans une tentative de décryptage des couleurs de l'interface d'administration :

* Pourquoi la phrase _Total Allocated (512 MB)_ est en rouge ? L'indicateur de progression est vert, la phrase _Unused 512 MB_ est en vert. Je conclue qu'il s'agit probablement d'un rouge marquant la criticité d'une ressource et non d'une erreur.

* Les mots _Usable Free Space (O B)_ en rouge m'inquiète un peu plus. Là encore, j'essai de me rassurer en me disant que [Couchbase](http://www.couchbase.com/) doit probablement réserver de l'espace disque progressivement et comme je n'ai encore aucune donnée, aucun espace disque n'a encore été réservé.

* La 3ème indication en rouge est _Servers Down : 1_. J'ai souvent de l'imagination pour trouver les bons côtés des choses mais là, je n'ai aucune inspiration qui me vient. Je dois avoir un problème !

Alors je clique sur ce message _Servers Down : 1_.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5c.png %}

Je décide de laisser en l'état et de compléter l'application pour voir si le client Couchbase a des difficultés à se connecter avec ce _Server Down : 1_ ;)

Pour communiquer avec Couchbase depuis l'application, nous allons utiliser la bibliothèque [couchbase-client](http://files.couchbase.com/maven2/couchbase/couchbase-client/) accessible via le repository Maven de Couchbase [http://files.couchbase.com/maven2/](http://files.couchbase.com/maven2/). Il faudrait donc modifier le fichier `demo-webapp/pom.xml` comme [ceci](https://raw.github.com/dadoonet/sql2nosql/03-couchbase-persistence/begin/demo-webapp/pom.xml) pour  ajouter la dépendance vers la librairie [couchbase-client](http://files.couchbase.com/maven2/couchbase/couchbase-client/).

Ce client est simple d'utilisation, voici un exemple d'utilisation :

``` java
// Création de la liste des différents noeuds Couchbase
List<URI> nodes = new ArrayList<URI>();
nodes.add(URI.create("http://127.0.0.1:8091/pools"));

// Création du client
CouchbaseClient client = new CouchbaseClient(nodes, "default", "");

// Récupération d'une donnée dont la clé est "MA_CLE"
String person = (String)client.get("MA_CLE");
```

Créer le fichier `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/ConnectionManager.java` avec [ce contenu](https://raw.github.com/dadoonet/sql2nosql/03-couchbase-persistence/end/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/ConnectionManager.java).

Créer le fichier `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/KeyUtil.java` avec [ce contenu](https://raw.github.com/dadoonet/sql2nosql/03-couchbase-persistence/end/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/KeyUtil.java)

Créer le fichier `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/ViewUtil.java` avec [ce contenu](https://raw.github.com/dadoonet/sql2nosql/03-couchbase-persistence/end/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/util/ViewUtil.java)

Compléter le fichier `demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonService.java` [ce contenu](https://raw.github.com/dadoonet/sql2nosql/03-couchbase-persistence/end/demo-webapp/src/main/java/fr/pilato/demo/sql2nosql/webapp/PersonService.java).

Redémarrer l'application.

J'obtiens une jolie stacktrace. Ah voilà quelque chose qui me parle !

```
Caused by: org.springframework.beans.BeanInstantiationException: Could not instantiate bean class [fr.pilato.demo.sql2nosql.webapp.PersonService]: Constructor threw exception; nested exception is com.couchbase.client.vbucket.config.ConfigParsingException: Number of buckets must be a power of two, > 0 and <= 65536
	at org.springframework.beans.BeanUtils.instantiateClass(BeanUtils.java:163)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:87)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateBean(AbstractAutowireCapableBeanFactory.java:1004)
	... 54 more
Caused by: com.couchbase.client.vbucket.config.ConfigParsingException: Number of buckets must be a power of two, > 0 and <= 65536
	at com.couchbase.client.vbucket.config.DefaultConfigFactory.parseEpJSON(DefaultConfigFactory.java:135)
```

Euh finalement, ça ne me parle pas tant que ça ;)

Je porte mon attention sur le message d'erreur _Number of buckets must be a power of two, > 0 and <= 65536_. Après plusieurs investigations dans différents forums [là](http://www.couchbase.com/forums/thread/number-buckets-must-be-power-two-0-and-0), [là](http://www.couchbase.com/forums/thread/number-buckets-must-be-power-two), ou encore [là](http://www.couchbase.com/issues/browse/MB-8332). Je croise même un message de [Tugdual](tugdual_grall) dans ces fils de discussion ;)

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5d.png %}

Après avoir cliqué sur tous les menus et tous les boutons de l'interface d'administration, je ne parviens toujours pas à avancer. J'ai réussi au passage à me débarrasser du message d'avertissement _Fail Over Warning : At least two servers are required to provide replication!_ en supprimant et recréant un _bucket_. Je comprends que j'ai eu ce message d'erreur car lors de l'initialisation de Couchbase, la case à cocher _Replicate_ est cochée par défaut. Par contre impossible de me débarrasser de l'erreur _Server Down : 1_.

J'ai essayé une version plus récente du client. Je remarque au passage que le client couchbase-client est disponible en fait dans le Repo Maven Central via la dépendance :

``` xml
<dependency>
    <groupId>com.couchbase.client</groupId>
    <artifactId>couchbase-client</artifactId>
    <version>1.2.2</version>
</dependency>
```
Quoiqu'il en soit, cela ne résoud pas le problème !

Je décide alors de recommencer l'installation. Je n'ai pas trouvé un moyen de revenir à l'état initial via l'interface d'administration. J'arrête le serveur, je supprime tous les fichiers générés par Couchbase puis je démarre [Couchbase](http://www.couchbase.com/). Cette fois-ci je choisis 1024 MB pour la RAM, je désactive la réplication et je sélectionne un échantillon de données (_beer_). Et là, c'est magique plus de _Server Down : 1_ !

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5e.png %}

Je redémarre l'application, plus d'exception !

Rechargement des données

``` sh
curl -XPOST http://localhost:8080/api/1/person/_init
```

Les données sont accessibles via l'url `http://localhost:8080/api/1/person/`.

Dans l'interface d'administration, à l'onglet _View_, il y a une vue _by_name_ qui permet de visualiser quelques données. Oui les données ont bien été insérées dans [Couchbase](http://www.couchbase.com/).

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-5f.png %}

## Les écrans avec AngularJS et Twitter Bootstrap
Nous avons jusqu'à présent un backend qui renvoie des données au format JSON. Cette étape va consister à recréer les vues que nous avions au départ.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-6.png %}

Pour ne pas trop alourdir cet article, je ne vais pas faire un cours sur AngularJS qui n'est pas le sujet principal de cette session ;)

Vous pouvez directement retrouver les sources dans la branche `04-angular/end` ou bien les télécharger directement [ici](https://github.com/dadoonet/sql2nosql/archive/04-angular/end.zip).

Redémarrer l'application.

Les écrans sont de retour.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-6a.png %}

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-6b.png %}

Il n'est pour le moment possible que d'effectuer des recherches par nom. 

## La recherche _full text_
La recherche _full text_ va être implémentée avec [Elasticsearch](http://www.elasticsearch.org/). L'idée principale est d'avoir les données répliquées de [Couchbase](http://www.couchbase.com/) à [Elasticsearch](http://www.elasticsearch.org/) qui va les indéxer. L'application _front end_, pour rechercher les données, va directement intéroger [Elasticsearch](http://www.elasticsearch.org/).

Nous aurons à la fin de cette étape, l'architecture suivante :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7.png %}

Je télécharge [Elasticsearch v0.90.2](https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-0.90.2.zip) et décompresser l'archive dans le répertoire de votre choix.

J'installe le plugin Couchbase pour [Elasticsearch](http://www.elasticsearch.org/) (version 1.1.0) via l'exécutable `bin/plugin(.bat)`.

```
bin/plugin -install transport-couchbase -url http://packages.couchbase.com.s3.amazonaws.com/releases/elastic-search-adapter/1.1.0/elasticsearch-transport-couchbase-1.1.0.zip
```

Dans le fichier `config/elasticsearch.yml`, je renseigne les paramètres :

```
couchbase.username: Administrator
couchbase.password: Administrator
couchbase.maxConcurrentRequests: 256
```

Je démarre [Elasticsearch](http://www.elasticsearch.org/).

```
bin/elasticsearch -f
```

Je crée un template [Elasticsearch](http://www.elasticsearch.org/).

```
curl -XPUT http://localhost:9200/_template/couchbase -d '
{
    "template" : "*",
    "order" : 10,
    "mappings" : {
        "couchbaseCheckpoint" : {
            "_source" : {
                "includes" : ["doc.*"]
            },
            "dynamic_templates": [
                {
                    "store_no_index": {
                        "match": "*",
                        "mapping": {
                            "store" : "no",
                            "index" : "no",
                            "include_in_all" : false
                        }
                    }
                }
            ]
        },
        "_default_" : {
            "properties" : {
                "meta" : {
                    "type" : "object",
                    "enabled" : false
                }
            }
        }
    }
}
'
```

Je crée un index pour person.

```
curl -XPUT http://localhost:9200/person
```

Je reviens à l'interface d'administration Couchbase : `http://127.0.0.1:8091/index.html`.

Je clique sur l'onglet XDCR.

Je crée un cluster de référence avec le bouton `Create Cluster Reference`.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7a.png %}

Je crée une réplication avec le bouton `Create Replication`.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7b.png %}

Et là dans mon cas, ça n'a pas fonctionné.

Après quelques recherches, je suis tombé sur [cette fil de discussion](https://github.com/couchbaselabs/elasticsearch-transport-couchbase/issues/3) qui conseille l'utilisation de la version 1.2.0 du plugin pour ma version de [Couchbase](http://www.couchbase.com/).

Je désinstalle de l'ancienne version du plugin.
```
bin/plugin -remove transport-couchbase
```
J'installe la version 1.2.0 du plugin.
```
bin/plugin -install transport-couchbase -url http://packages.couchbase.com.s3.amazonaws.com/releases/elastic-search-adapter/1.2.0/elasticsearch-transport-couchbase-1.2.0.zip
```

Je réessaie de créer la réplication et j'ai toujours la même erreur. Côté [Elasticsearch](http://www.elasticsearch.org/), je peux lire l'exception suivante dans les logs :

```
013-11-23 02:30:02,779][WARN ][org.eclipse.jetty.servlet.ServletHandler] Error for /pools/default/buckets
java.lang.NoSuchMethodError: org.elasticsearch.cluster.metadata.MetaData.getIndices()Ljava/util/Map;
	at org.elasticsearch.transport.couchbase.capi.ElasticSearchCouchbaseBehavior.getBucketsInPool(ElasticSearchCouchbaseBehavior.java:82)
```

Je comprends que ma version d'[Elasticsearch](http://www.elasticsearch.org/) n'est pas compatible avec la nouvelle version du plugin.

J'ai trouvé le tableau de compatibilité suivant qui rend clair tous mes problèmes d'incompatibilité accessible [ici](https://github.com/couchbaselabs/elasticsearch-transport-couchbase) :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7c.png %}

[Tugdual](tugdual_grall) et [David](david_pilato) ont travaillé sur la ligne Plugin=1.1.0, Couchbase=2.0, ElasticSearch=0.90.2.

Vu que j'ai une base [Couchbase](http://www.couchbase.com/) qui fonctionne et que comme tout développeur j'aime bien les dernières versions, je vais télécharger [ElasticSearch 0.90.5](https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-0.90.5.zip) et recommencer l'installation.

J'obtiens toujours la même erreur lors de la création de la réplication même avec les dernières versions. Par un geste de désespoir je supprime et recrée le cluster de référence. Et là, la création de la réplication se fait sans problème !

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7d.png %}

Mais... Il y a un petit message que je n'ai pas envie de voir _Last 10 errors_. Je clique sur ce message de couleur bleu.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7e.png %}

Là je ne sais pas trop quoi penser ;) La réplication ne se passe visiblement pas bien. 

Je décide de faire une courte pause, de télécharger la version 2.0.0 de Couchbase et de revenir à la version 0.90.2 d'[Elasticsearch](http://www.elasticsearch.org/).

Je démarre la version 2.0.0 de [Couchbase](http://www.couchbase.com/), elle rentre en conflit avec ma version 2.2.0 installée précédemment car elles partagent le même répertoire de travail. Je déplace ce répertoire de travail et là plus de problème.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7f.png %}

Je réinjecte les données :

```
curl -XPOST http://localhost:8080/api/1/person/_init

```

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7g.png %}

Les données sont bien répliquées, on est passé de 1001 à 2001 comme prévu.

Le client AngularJS sera modifié pour interroger directement [Elasticsearch](http://www.elasticsearch.org/) pour la fonctionnalité de recherche. [Elasticsearch](http://www.elasticsearch.org/) expose une API REST pour rechercher des données. Pour rechercher les personnes ayant 'Alix' dans leur nom ou prénom, il suffit d'accéder à l'adresse : `http://127.0.0.1:9200/person/_search?q=alix`.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-7h.png %}

## Vos tableaux de bord les doigts dans le nez avec Kibana
[Kibana](http://www.elasticsearch.org/overview/kibana/) est une application web qui permet de visualiser les données indexées dans [Elasticsearch](http://www.elasticsearch.org/) suivant des critères.

A l'issue de cette étape, l'architecture de l'application va ressembler à ceci :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-8.png %}

J'installe le plugin Kibana pour [Elasticsearch](http://www.elasticsearch.org/).

```
bin/plugin -install elasticsearch/kibana
```

En lançant [Kibana](http://www.elasticsearch.org/overview/kibana/), `http://localhost:9200/_plugin/kibana/`, j'obtiens ceci :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-8a.png %}

Je choisis de ne pas tenter l'aventure de la mise à jour. Je clique sur le lien `src` et j'accède à une page d'introduction.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-8b.png %}

Je clique sur `Sample Dashboard`.

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-8c.png %}

Il est possible de filtrer, visualiser les données suivant plusieurs axes d'analyse. Si vous souhaitez jouer avec [Kibana](http://www.elasticsearch.org/overview/kibana/), il y a une démo en ligne accessible à l'adresse [http://demo.kibana.org/#/dashboard](http://demo.kibana.org/#/dashboard) :

{% img center /images/nantesjug/nov13/nantesjug-nov13-demo-8d.png %}

## Les slides de Devoxx Belgique 2013

<iframe src="http://www.slideshare.net/slideshow/embed_code/23026423" width="800" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

## Et pour conclure ?
Comme vous l'avez surement constaté, cette soirée du JUG Nantes a été riche en contenu.

Dans le monde des bases de données NoSQL orientées document, [Couchbase](http://www.couchbase.com/) a un concurrent direct [MongoDB](http://www.mongodb.org/). Un participant va poser la question de savoir quelles étaient les différences de fond entre ces deux bases de données. [Tugdual](https://twitter.com/tgrall), évangéliste [Couchbase](http://www.couchbase.com/), va donner les éléments de réponse suivants :

* [Couchbase](http://www.couchbase.com/) est conçu pour faciliter la distribution des données, la création de nombreux clusters de données via une interface d'administration. Il est donc plus indiqué pour des systèmes nécessitant de traitement de grand volumes de données distribuées sur plusieurs machine. Créer un cluster avec [MongoDB](http://www.mongodb.org/) ne serait pas une tâche aussi simple que dans [Couchbase](http://www.couchbase.com/).

* [MongoDB](http://www.mongodb.org/) est plus riche et plus facile à requêter que [Couchbase](http://www.couchbase.com/). Il dispose d'une API très puissante pour extraire des données. C'est pour cette raison qu'il est conseillé de coupler [Couchbase](http://www.couchbase.com/) à [Elasticsearch](http://www.elasticsearch.org/) pour disposer d'une plus grande puissance d'extraction/analyse de données.

Au regard de la complémentarité des deux technologies ([Couchbase](http://www.couchbase.com/) & [Elasticsearch](http://www.elasticsearch.org/)), on peut se demander _A quand le rachat de l'un par l'autre ?_. [David](https://twitter.com/dadoonet) va affirmer, hors séance, que ce n'était pas à sa connaissance à l'ordre du jour. Il y voit en [Elasticsearch](http://www.elasticsearch.org/) un produit complet au point que certains clients n'hésiteraient pas à l'utiliser directement en tant que base de données.

Bien évidemment, si vous souhaitez mettre en oeuvre ces outils, prenez des versions compatibles entre elles et n'hésitez pas à recommencer quand vous êtes dans une impasse ;)

Je ne sais pas pour vous mais je suis impatient d'être à la prochaine soirée du JUG Nantes : [Grails dans les tranchés + Java 8](http://nantesjug.org/#/events/2013_12_04).
