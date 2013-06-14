---
layout: post
title: "Gatling 2 : La nouvelle API d'injection"
date: 2013-06-12 20:56
comments: true
categories: gatling
---

Lorsque nous écrivons une simulation Gatling, nous avons généralement 2 parties :

* La définition du scénario : orchectration des différentes requêtes
* L'injection de charges : nombre d'utilisateurs, définition de rampe, etc...

La seconde partie a été refondue, les mots clés du DSL : users, delay et ramp ont été supprimés et un nouveau DSL a été défini.

Pour illustrer l'utilisation de ce nouveau DSL, nous allons utiliser un scénario qui consiste à rechercher successivement sur Twitter les mots clés : jcertif, gatling, nantes, scala.

Définir un tel scénario consiste à écrire les lignes suivantes :

``` scala
val httpConf = httpConfig
    .baseURL("http://search.twitter.com")
    .acceptHeader("*/*")
    .acceptCharsetHeader("ISO-8859-1,utf-8;q=0.7,*;q=0.3")
    .acceptEncodingHeader("gzip,deflate,sdch")
    .acceptLanguageHeader("en-US,en;q=0.8")
    .connection("keep-alive")
    .userAgentHeader("Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.22 (KHTML, like Gecko) Ubuntu/12.10 Chromium/25.0.1364.172 Chrome/25.0.1364.172 Safari/537.22")

val scn = scenario("Recherches sur Twitter")
			.exec(http("Recherche JCertif").get("/search.json?q=jcertif"))
			.exec(http("Recherche Gatling").get("/search.json?q=gatling"))
			.exec(http("Recherche Nantes").get("/search.json?q=nantes"))
			.exec(http("Recherche Scala").get("/search.json?q=scala"))
```

La première partie définie l'URL de base "http://search.twitter.com" à utiliser et des informations que nous allons envoyer à chaque requête.

La seconde définie le scénario que nous allons exécuter.

Voyons à présent plusieurs cas d'injection et leurs mises en oeuvre.

## Cas 1 : 100 utilisateurs lancés une seule fois en même temps

### Le code de l'injection
``` scala
setUp(scn.inject(
	atOnce(100 users)
	).protocolConfig(httpConf))
```

### Graphe du nombre d'utilisateurs actifs pendant le test
{% img center /images/gatling2/injectionapi/cas1_active_users.png %}

Les 100 utilisateurs sont lancés en même temps.

### Le code complet
[Case1_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case1_Simulation.scala)

## Cas 2 : 100 utilisateurs simultanés puis 10 secondes d'attente puis 50 utilisateurs simultanés

### Le code de l'injection
``` scala
setUp(scn.inject(
	atOnce(100 users),
	nothingFor(10 minutes),
    atOnce(50 users)
	).protocolConfig(httpConf))
```

### Graphe du nombre d'utilisateurs actifs pendant le test
{% img center /images/gatling2/injectionapi/cas2_active_users.png %}

### Le code complet
[Case2_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case2_Simulation.scala)


## Cas 3 : 50 utilisateurs lancés en 5 minutes

### Le code de l'injection
``` scala
setUp(scn.inject(
	ramp(100 users) over (40 seconds)
	).protocolConfig(httpConf))
```

### Graphe du nombre d'utilisateurs actifs pendant le test
{% img center /images/gatling2/injectionapi/cas3_active_users.png %}

La répartition des utilisateurs dans le temps est faite de façon linéaire : 1 nouvel utilisateur toutes les 6 secondes dans notre cas.

### Le code complet
[Case3_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case3_Simulation.scala)


## Cas 4 : 2 requêtes/seconde pendant 2 heures

### Le code de l'injection
``` scala
setUp(scn.inject(
	constantRate(2 usersPerSec) during (2 minutes)
	).protocolConfig(httpConf))
```

### Résultat

{% img center /images/gatling2/injectionapi/cas4_active_users.png %}

### Le code complet
[Case4_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case4_Simulation.scala)



## Cas 5 : Passer de 2 utilisateurs/seconde à 10 utilisateurs/seconde en 5 minutes

### Le code de l'injection
``` scala
setUp(scn.inject(
	rampRate(2 usersPerSec) to(10 usersPerSec) during(5 minutes)
	).protocolConfig(httpConf))
```

### Résultat

{% img center /images/gatling2/injectionapi/cas5_active_users.png %}


### Le code complet
[Case5_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case5_Simulation.scala)



## Cas 6 : Pour 100 utilisateurs au total, lancer 10 utilisateurs en 20 secondes et espacer deux lancements avec une pause de 15 secondes

### Le code de l'injection
``` scala
setUp(scn.inject(
	split(100 users).into(ramp(10 users) over (20 seconds)).separatedBy(15 seconds)
	).protocolConfig(httpConf))
```

### Résultat

{% img center /images/gatling2/injectionapi/cas6_active_users.png %}

### Le code complet
[Case6_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case6_Simulation.scala)


## Cas 7 : 2 scénarios (recherche twitter et recherche facebook) lancés en simultanné

### Les scenarios

``` scala
  val scnSearchTwitter = scenario("Recherches sur Twitter")
    .exec(http("Recherche Twitter JCertif").get("http://search.twitter.com/search.json?q=jcertif"))
    .exec(http("Recherche Twitter Gatling").get("http://search.twitter.com/search.json?q=gatling"))
    .exec(http("Recherche Twitter Nantes").get("http://search.twitter.com/search.json?q=nantes"))
    .exec(http("Recherche Twitter Scala").get("http://search.twitter.com/search.json?q=scala"))

  val scnSearchFacebook = scenario("Recherches sur Facebook")
    .exec(http("Recherche Facebook JCertif").get("https://graph.facebook.com/search?q=jcertif&type=post"))
    .exec(http("Recherche Facebook Gatling").get("https://graph.facebook.com/search?q=gatling&type=post"))
    .exec(http("Recherche Facebook Nantes").get("https://graph.facebook.com/search?q=nantes&type=post"))
    .exec(http("Recherche Facebook Scala").get("https://graph.facebook.com/search?q=scala&type=post"))
```

### Le code de l'injection

``` scala
 setUp(
    scnSearchTwitter.inject(atOnce(40 users)).protocolConfig(httpConf),
    scnSearchFacebook.inject(atOnce(40 users)) protocolConfig (httpConf))
```

### Résultat

{% img center /images/gatling2/injectionapi/cas7_active_users.png %}

### Le code complet
[Case7_Simulation.scala](https://github.com/roddet/gatling2-les-nouveautes/blob/master/src/test/scala/com/roddet/blog/gatling2/injectapi/Case7_Simulation.scala)

## Mon avis
Vous l'avez peut-être constater, dès fois le DSL est plus facile à lire que ma retranscription en français :)

## Note
J'ai écris les scénarios en utilisant l'API Twitter version 1 qui doit être bientôt désactivé (si ce n'est pas déjà fait :)).

## Liens utiles

* [Télécharger Gatling 2](http://gatling-tool.org/)
* [Toutes les nouveautés Gatling 2](https://github.com/excilys/gatling/wiki/Gatling-2)
* [Le code de toutes les simulations](https://github.com/roddet/gatling2-les-nouveautes)
* [Les résultats complets des différents cas de tests](https://github.com/roddet/gatling2-les-nouveautes/tree/master/results/injectapi)

