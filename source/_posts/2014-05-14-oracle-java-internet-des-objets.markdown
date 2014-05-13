---
layout: post
title: "Oracle, Java & Internet des Objets"
date: 2014-05-13 08:41
comments: true
categories: java
---

2014 c'est l'année d'_Internet Of Things_ ou l'Internet des Objets ou encore l'Internet des Choses.

Tous les médias le disent, regarder [ici](http://www.lefigaro.fr/secteur/high-tech/2014/01/06/01007-20140106ARTFIG00069-l-annee-2014-placee-sous-le-signe-de-l-internet-des-objets.php), [là](http://www.atlantico.fr/rdv/minute-tech/2014-sera-t-elle-annee-internet-choses-bertrand-duperrin-951986.html) ou encore [là](http://blog.econocom.com/blog/2014-lannee-de-linternet-des-objets-grand-public/).

C'est tellement l'année de l'Internet des Objets que :

* les conférences IT ont maintenant des tracks dédiés, [_Future Devoxx_](http://cfp.devoxx.fr/devoxxfr2014/talks/hack),
[Un après-midi (Démo NAO, Objets connectés, Démo ORA Smart Glasses) au BreizhCamp](http://cfp.devoxx.fr/devoxxfr2014/talks/hack), etc.
* des conférences dédiées fleurissent un peu partout dans le monde : [IoT Conference](http://blog.econocom.com/blog/2014-lannee-de-linternet-des-objets-grand-public/), [IoT Asia](http://www.internetofthingsasia.com/), [IoT Day](http://iotday.org/), etc.
* des objets connectés en tout genres apparaissent. Il en y a tellement que [Amazon a créé une rubrique dédiée "Wearable Technology"](http://www.amazon.com/b?node=9013937011).

[Oracle](http://www.oracle.com/index.html) l'a bien intégré et déploit beaucoup d'énergie sur ce sujet ces derniers temps comme nous allons le voir.

{% img center /images/logos/FY14Duke14.png %}

## Une gamme de produits Java Embedded
Il y a plusieurs produits pour faire du Java en environnement embarqué :

* [Oracle Java SE Embedded](http://www.oracle.com/technetwork/java/embedded/resources/se-embeddocs/index.html) destiné à des appareils de 32MB ou plus.
* [Oracle Java ME Embedded Client](http://www.oracle.com/us/technologies/java/embedded/mobile-edition/overview/index.html) pour des appareils de 8MB ou plus.
* [Oracle Java ME Embedded](http://www.oracle.com/technetwork/java/embedded/resources/me-embeddocs/index.html) pour des appareils de 128 KB ou plus.
* [Oracle Java Embedded Suite](http://www.oracle.com/technetwork/java/embedded/resources/java-embedded-suite/index.html), c'est du Oracle Java SE Embedded + Java DB + Glassfish optimisé + JAX-RS (Jersey)
* [Oracle Event Processing](http://docs.oracle.com/cd/E28280_01/doc.1111/e39318/toc.htm), un serveur qui permet de supporter des applications event-driven qui font beaucoup de streaming de données.
Il n'y a pas le mot _réactif_ mais ça ressemble dans l'idée ;)

## Des événements Oracle IoT
Des sessions en ligne sont régulièrement organisées :

* [Une session le 5 mai dernier](https://blogs.oracle.com/java/entry/free_webinar_on_java_micro)
* [Un "Virtual Developer Day"](https://blogs.oracle.com/java/entry/virtual_developer_day_java_2014) les 6 (Amérique), 14 (Europe/Afrique) et 21 mai (Asie).

En Afrique, des sessions Java Embedded ont été organisées notamment :

* En Tunisie, les 18 et 19 avril, pendant [JCertif Tunisie 2014](http://jcertif.com/tunisie/2014/05/09/jcertif-tunisie/)
* Au Congo-Brazzaville, en mars dernier, lors de la [Journée IoT Brazzaville](https://plus.google.com/events/c7fcpf5lb2dhclvbd4lesni8hng?cfem=1)
* D'autres sessions sont prévues en juillet en Côte d'Ivoire et au Cameroun lors des prochains événement JCertif.

Au Brésil, un [Hackaton](https://blogs.oracle.com/java/entry/internet_of_things_iot_hackathon) a été organisé.

Le 17 et 18 mail prochain, [Oracle](http://www.oracle.com/index.html) sera présent à l'événement [Make Faire](http://makerfaire.com/) et va présenter un [IoT Java Panel](https://blogs.oracle.com/java/entry/java_shows_off_at_maker).

## Un Challenge IoT
Un [IoT Developer Challenge](https://www.java.net/challenge) organisé par [Oracle](http://www.oracle.com/index.html) est en cours actuellement.
Les gagnants pourront aller à [Java One 2014 !](https://www.oracle.com/javaone/index.html).

Vous avez jusqu'au 30 mai pour participer.

## Un MOOC sur Java Embedded
Un [MOOC](http://en.wikipedia.org/wiki/Massive_open_online_course) sous le thème _Develop Java Embedded Applications Using a Raspberry Pi_ est donné par [Oracle](http://www.oracle.com/index.html) plusieurs fois dans l'année .

Il y en a eu un le 31 mars dernier, le prochain commence le 30 mai.

La formation dure 5 semaines et montre comment utiliser Java Embedded pour :

* Lire les différentes entrées de la Raspberry Pi
* Manipuler des LED via les interfaces GPIO
* Récupérer la température et la pression atmosphérique
* Stocker et gérer des données
* Envoyer des données à un client

Vous pouvez vous inscrire [ici](https://apex.oracle.com/pls/apex/f?p=44785:145:0::::P145_EVENT_ID,P145_PREV_PAGE:861,143).

## Allez-vous être un acteur de l'Internet des Objets ?
Si vous êtes développeur (quelque soit le langage/plateforme), l'Internet des Objets vous concerne directement que vous le vouliez ou non.

Le sujet étant jeune, il y a beaucoup d'opportunités, les technologies méritent d'être éprouvées.

[La communauté Javascript](http://postscapes.com/javascript-and-the-internet-of-things) est très active sur le sujet.

Pour les développeurs Java, une communité [IoT existe](https://community.java.net/community/iot). Le [Blog de Terrence Barr](https://terrencebarr.wordpress.com/tag/java-embedded/), est également une source d'informations intéressante.

Alors, ça vous tente du Java Embarqué ?