---
layout: post
title: "BreizhCamp 2013 # Android BootStrap"
date: 2013-06-14 15:00
comments: true
categories: breizhcamp13 android
---

{% img center /images/breizhcamp13/android_bootstrap/android-bootstrap.jpg %}

## Animateurs

* [Jean-François Garreau](https://plus.google.com/114772056028520115043) SQLI Nantes

## Les supports
Les slides c'est par [ici](http://blog.binomed.fr/binomed_docs/Prezs/AndroidBootstrap/bootstrap.html).

Les sources de l'application BreizhCamp c'est par [là](https://github.com/binomed/DevFestCodeLab/tree/breizhcamp).

## Déroulement
Jean-François a commencé par nous expliquer l'importance des librairies. Nous avons en tant que développeur, dès fois, la facheuse tendance de "ré-inventer la roue" à chaque projet. Cela est particulière le cas sur la plateforme Android dont le SDK nous fournit déjà pas mal de composants prêts à l'emploi. Il faut le savoir, les librairies existent et certaines ont particulièrement été éprouvées.

Tout n'est pas parfait dans le monde des librairies Android. Intégrer des librairies dans notre application peut nous emmener des complications en terme de gestion de version, de conflits, du poids total, de l'environnement de développement, ...

Jean-François va ensuite nous présenter l'utilisation de plusieurs librairies (ActionBarSherlock, ACRA, Compatibility package, ViewPager, ...) à travers la réalisation d'une application pour le BreizhCamp (visualisation des sessions, des présentateurs, etc...).

Il va terminer par nous présenter les bonnes pratiques d'intégration des librairies dans un projet Android et nous conseille de suivre de prêt [Android BootStrap](http://www.androidbootstrap.com/) et [Android KickStartR](http://androidkickstartr.com/)

## Ce que j'en retiens

* Il existe des librairies Android pratiques à utiliser
* L'utilisation d'une librairie peut être problématique suivant qu'elle :
	* soit distribué en APK ou en JAR
	* soit disponible dans un dépôt Maven ou non
	* s'utilise par héritage ou par composition. L'héritage multiple étant interdit en Java, une librairie peut nous bloquer l'usage d'une autre.
* Qu'il y a 2 projets à surveiller : [Android BootStrap](http://www.androidbootstrap.com/) et [Android KickStartR](http://androidkickstartr.com/)