---
layout: post
title: "Soirée JUG Nantes : Java 8 -> Lambdas, Streams et Collectors (Partie 2)"
date: 2013-12-26 09:00
comments: true
categories: jugnantes jugnantesdec13
---

La première partie de cet article est accessible [ici](/blog/).

La deuxième partie de cette soirée du [JUG Nantes](http://nantesjug.org/) est consacrée à une présentation de Java 8 animée par [José Paumard](https://twitter.com/JosePaumard).

Avant de rentrer dans le coeur de la présentation de [José](https://twitter.com/JosePaumard), faisons un tour de ce que réserve Java 8.

## Java 8 -> quoi de neuf ?
Java 8 est une évolution majeure du langage Java.

La liste complète des nouveautés peut être consulté [ici](http://openjdk.java.net/projects/jdk8/features).

Elles peuvent être classées suivant plusieurs catégories pour les plus significatives : 

* **Nouveaux Projets** -> les nouvelles fonctionnalités assez structurantes pour être considérées comme des projets à part.
* **Machine Virtuelle** -> des modifications du fonctionnement de la JVM et des travaux d'amélioration des performances.
* **Core** -> nouveautés non structurantes du langage (ajout d'API, d'annotations, etc.).

Voici quelques nouveautés qui ont attirées mon attention.

## Java 8 -> Nouveaux Projets
Deux projets :

* Le [projet Lambda](http://openjdk.java.net/projects/lambda/) qui ajoute les _closures_ au langage Java. Il s'agit certainement de la fonctionnalité qui a le plus d'impact sur le langage. C'est le sujet principal de cette soirée.
* Le [projet Nashorn](http://openjdk.java.net/projects/nashorn/) qui offre un moteur d'exécution Javascript sur la JVM. En prenant un raccourci, c'est du [NodeJS](http://nodejs.org/) mais sur la JVM.

## Java 8 -> Machine Virtuelle
L'évolution la plus remarquable est [la suppression de la _Permanent Generation_](http://openjdk.java.net/jeps/122). Cette zone était utilisée par la JVM pour stocker les définitions des classes, méthodes, etc. Elle était bien distincte de la _Heap_ qui contient les instances des objets créées.

Par défaut, la _Permanent Generation_ a la taille maximale de 64MB. Il est possible de modifier cette valeur avec le paramètre _-XX:MaxPermSize_.

Si vous avez le plaisir de travailler sur application _usine_ (nombreuses classes, nombreuses librairies), habituelle du milieu professionnel, vous avez peut-être déjà rencontré une erreur qui ressemble à cela :

```
java.lang.OutOfMemoryError: PermGen space
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:620)
```

Avec Java 8, dites adieu à cette erreur et à le paramétrage lié à la _Permanent Generation_.

Cette évolution de la machine virtuelle est un des résultats de la convergence entre la machine virtuelle propriétaire d'Oracle [JRockit](http://www.oracle.com/technetwork/middleware/jrockit/overview/index-101826.html) et celle de la communauté : HotSpot. 
En effet, [JRockit](http://www.oracle.com/technetwork/middleware/jrockit/overview/index-101826.html) est distribué avec le serveur d'application propriétaire d'Oracle [WebLogic](http://www.oracle.com/technetwork/middleware/weblogic/overview/index.html) et il a la particularité de ne pas avoir de _Permanent Generation_. Un bon signe pour la communauté ?

## Java 8 -> Core
Quelques améliorations sont apportées au coeur du JDK.

### Une application JavaFX devient exécutable ([JEP 153](http://openjdk.java.net/jeps/153))
Avant Java 8, pour rendre une application JavaFX exécutable depuis un JAR, il fallait faire deux choses : 

* Créer une classe avec la méthode _public static void main(String[] args)_ qui contiendra du code d'initialisation de l'application JavaFX.
* Référencer cette classe dans l'attribut _Main-Class_ du _Manifest_ du JAR. 

Avec Java 8, il est possible de créer un JAR exécutable JavaFX sans passer par une classe avec une méthode _main_.


### Des annotations sur les types Java ([JEP 104](http://openjdk.java.net/jeps/104))
Java 8 permettra d'écrire ce genre de chose :

```java
new @Interned MaClasse();

// ou encore
String str = (@NotNull String) element;

// ou encore 
public class MaClasse<T> implements @ReadOnly List<@ReadOnly T>

// ou encore
public void monitorTemperature() throws @Critical TemperatureException {...}
```

La documentation officielle en version béta : [ici](http://docs.oracle.com/javase/tutorial/java/annotations/index.html). 

### Répéter des annotations ([JEP 120](http://openjdk.java.net/jeps/120))
Avec Java 8, il sera possible d'écrire :

```java
@Alert(role="Manager")
@Alert(role="Administrator")
public class UnauthorizedAccessException extends SecurityException { ... }
```

Plus de détails, [ici](http://docs.oracle.com/javase/tutorial/java/annotations/repeating.html).

### Accéder au nom des paramètres au runtime ([JEP 118](http://openjdk.java.net/jeps/118))
L'idée ici est de stocker dans le _bytecode_ les noms des paramètres de méthodes ainsi que leurs types. Cette fonctionnalité pourrait apporter un vrai plus aux développeurs de librairies.

Par exemple, on pourrait peut-être un jour rendre optionnel l'annotation _@PathParam_ de JAX-RS ;)

### Une nouvelle API pour les dates et les heures ([JEP 150](http://openjdk.java.net/jeps/150))
Java 8 vient avec une nouvelle API pour les dates/heures qui se veut plus _clean_, _fluent_, _immutable_ et _extensible_.

Un aperçu d'utilisation :
```java
LocalDate aujourdhui = LocalDate.now();
LocalDate dans2ansMoins4jours = LocalDate.now().plusYears(2).minusDays(4);
```

Documentation officielle en version béta : [ici](http://docs.oracle.com/javase/tutorial/datetime/index.html).


### Trie en parallèle des tableaux ([JEP 103](http://openjdk.java.net/jeps/103))
Des méthodes sont ajoutées à la classe _java.util.Arrays_ :
```java
Arrays.parallelSort(...);
```

## Revenons au JUG !
Les nantais ont répondu présent pour découvrir Java 8 et pour [José](https://twitter.com/JosePaumard), c'est la session qui clôture son tour de Bretagne... Ou plutôt de l'ouest de la France pour les plus susceptibles ;)

[José](https://twitter.com/JosePaumard) va faire un zoom sur les expressions _Lambda_ et ses impacts sur le JDK 8.

{% img center /images/nantesjug/dec13/nantesjug-jose-paumard.jpg %}

## Une introduction aux expressions Lambda
[José](https://twitter.com/JosePaumard) va illustrer les expressions _Lambda_ à l'aide du pattern _Map - Filter - Reduce_. Il va montrer une implémentation en Java 7 et une équivalence en Java 8 avec les expressions _Lambda_.

Illustration -> Approche impérative (avant Java 8) vs Approche fonctionnel (SQL)

{% img center /images/nantesjug/dec13/nantesjug-prog-imp-vs-fonctionnel-by-jose-paumard.gif %}

Illustration -> Passage d'un code Java 7 à un code Java 8 avec une expression _Lambda_.

{% img center /images/nantesjug/dec13/nantesjug-lambdas-by-jose-paumard.gif %}


## Syntaxes d'une expression _Lambda_

```java
mapper = (Person person) -> person.getAge();

mapper = person -> person.getAge(); // sans préciser le type

mapper = Person::getAge; // méthode non statique

sum = Integer::max; // méthode statique

mapper = (Person person) -> {
	System.out.println("Mapping " + person);
	return person.getAge();
}

reducer = (int i1, int i2) -> {
	return i1 + i2;
}

reducer = (int i1, int i2) -> i1 + i2;

reducer = (i1, i2) -> i1 + i2; // sans préciser les types

```

## La notion d'interface fonctionnelle
Une expression _Lambda_ peut être définie comme une _instance_ d'une interface dite fonctionnelle. Une interface fonctionnelle est une interface avec une seule méthode abstraite.

L'annotation _@FunctionalInterface_ peut être utilisée pour désigner une interface fonctionnelle. Elle est optionnelle et comme l'annotation _@Override_, elle ne sert qu'à apporter une sécurité supplémentaire à la compilation. Une interface annotée avec _@FunctionalInterface_ ne compilera pas si elle possède plusieurs méthodes sans implémentation par défaut.

Un exemple d'utilisation dans le JDK 8.

```java
package java.util.function;

@FunctionalInterface
public interface IntSupplier {
    int getAsInt();
}
```

## Le nouveau package _java.util.functions_
Il contient des interfaces fonctionnelles usuelles utilisées pour représenter des expressions _Lambda_ en entrée et sortie de méthode.

Les interfaces _Supplier_, _Consumer_, _BiConsumer_, _Function_, _BiFunction_, _Predicate_, _BiPredicate_ seront expliquées pendant la présentation.

## Des méthodes implémentées dans les interfaces
Avec Java 8, il est possible d'exprimer une implémentation par défaut à une méthode d'une interface. C'est l'option qui a été choisie pour permettre d'ajouter des méthodes à des interfaces _historiques_ du JDK sans modifier toutes les classes des implémentations.

Le mot clé `default` est utilisé pour définir une implémentation par défaut.

Exemple avec l'interface `Collection<E>` :

```java
public interface Collection<E> {
	
	...

	default Stream<E> stream() {
		return ...;
	}
 }
```

Java 8 autorise aussi les méthodes statiques dans les interfaces.

```java
public interface MonInterface {
	
	public static void main(String[] args) {
		System.out.println("Désormais, le Hello World en Java se fera avec une interface");
	}
}
```

## L'API _Stream_
Un _Stream_ sera défini comme une interface paramétrée construite à partir d'une source (une collection, un tableau, une source I/O) qui permet d'appliquer une expression _Lambda_ sur ses éléments.

Comment construire un _Stream_ ?
```java
// A partir d'une collection
Collection<String> collection = ...;
Stream<String> stream1 = collection.stream();

// A partir d'un tableau
Stream<String> stream2 = Arrays.stream(new String[]{"un", "deux", "trois"});

// A partir d'une factory de Stream 
// Stream.of, Stream.empty, Stream.generate, Stream.iterate, etc.
Stream<String> stream3 = Stream.of("un", "deux", "trois");

// A partir de quelques méthodes de classes du JDK mises à jour
IntStream stream4 = random.ints();
```

Un exemple d'utilisation ?

```
int sum = persons.stream()
	.map(p -> p.getAge())
	.filter(a -> a > 20)
	.reduce(0, (a1, a2) -> a1 + a2);
```

Deux types d'opérations sont applicables à un _Stream_ : une opération dite _intermédiaire_ et une dite _terminale_. Seule une opération dite _terminale_ déclenche le traitement modélisé. Dans l'exemple précédent, _map_ et _filter_ sont des opérations intermédiaires et _reduce_ une opération terminale.

Attention, un _Stream_ ne peut être traité qu'une seule fois. Une fois une opération _terminale_ exécutée, il est nécessaire de créer un nouveau _Stream_.

Illustration -> Un _Stream_ est _lazy_ (seule l'opération terminale déclenche le traitement) !

{% img center /images/nantesjug/dec13/nantesjug-stream-lazy-by-jose-paumard.gif %}

## _Stream_ parallèle
Il permet d'exécuter du code modélisé dans un _Stream_ en parallèle.

Comment construire un _Stream_ parallèle ?

```java
// Appeler parallelStream() au lieu de stream()
Stream<String> s = strings.parallelStream();

// Appeler parallel() sur un stream existant
Stream<String> s = strings.stream().parallel();
```

Attention, un _Stream_ parallèle ne signifie pas toujours plus performant. Le parallélisme entraine des opérations supplémentaires et ses performances sont dépendantes de la nature des opérations à exécuter. Il n'y a donc pas de recette magique de performance, la règle _mesurer pour optimiser_ s'applique toujours. 

## Les _Optionals_
Certaines méthodes des classes du JDK 8 vont renvoyer des instances de la famille _Optional_ : _Optional<T>_, _OptionalInt_, _OptionalLong_, _OptionalDouble_.

Cette structure est utilisée pour modéliser une possible absence de résultat d'un traitement.

Que peut-on faire avec un _Optional_ dans les mains ?

Un exemple avec `OptionalInt` :
```java
OptionalInt opt = ...;

// Tester s'il contient une valeur et récupérer la valeur
if(opt.isPresent()) {
	int valeur1 = opt.get();
} else {
	...
}

// Lire la valeur ou lancer une exception NoSuchElementException s'il n'y a pas de valeur
int valeur2 = opt.getAsInt();

// Lire la valeur ou lancer une exception particulière s'il n'y a pas de valeur
int valeur3 = opt.orElseThrow(exceptionSupplier);

// Lire la valeur si elle existe sinon retourner une valeur par défaut
int valeur4 = opt.orElse(12);
```

## Les réductions & La classe Collectors
L'API _Stream_ donne accès à plusieurs réductions : _reduce()_, _count()_, _min()_, _max()_, _findFirst()_, etc.

La méthode _collect_ applicable à un _Stream_ permet d'appliquer des réductions complexes à partir d'un _Collector_.
Sans rentrer dans les détails de la définition d'un _Collector_, la classe _Collectors_ fournit un ensemble d'instance de _Collector_ qui facilitent le travail du développeur.

Quelques exemples d'utilisation de la classe _Collectors_.

```java
// Transformer un Stream en List
List<Person> liste1 = persons.stream().collect(Collectors.toList());

// Transformer un Stream en Set
Set<String> liste2 = persons.stream().collect(Collectors.toSet());

// Transformer un Stream en TreeSet
TreeSet<String> liste3 = persons.stream().collect(Collectors.toSet(TreeSet::new));

// Concaténer les noms d'une liste de personnes
String names1 = persons.stream().map(Person::getName).collect(Collectors.joining());

// Concaténer les noms séparés par une virgule d'une liste de personnes
String names2 = persons.stream().map(Person::getName).collect(Collectors.joining(","));

// Compter le nombre de personnes
int nbPersons = persons.stream().collect(Collectors.counting());

// Moyenne des ages des personnes
double moyenneAge = persons.stream().collect(Collectors.averagingDouble(Person::getAge));

// Regroupement des personnes par age
Map<Integer, List<Person>> map = persons.stream().collect(Collectors.groupingBy(Person::getAge));

// Regroupement des personnes par age en utilisant un Set
Map<Integer, Set<Person>> map = persons.stream().collect(Collectors.groupingBy(Person::getAge,Collectors.toSet()));

// Répartir les données en 2 ensembles : true -> liste des personnes age > 20 et false -> le reste
Map<Boolean, List<Person>> map = persons.stream().collect(Collectors.partitionningBy(p -> p.getAge() > 20));

```

## Nouvelle API pour construire des comparateurs
Avec Java 8, ça devient presqu'un plaisir de créer une instance de l'interface _Comparator_ :
```java
Comparator<Person> comp = Comparator.comparing(Person::getLastName)
									.thenComparing(Person::getFirstName)
									.thenComparing(Person::getAge);
```

## Pourquoi des expressions _Lambda_ ?
[José](https://twitter.com/JosePaumard) va expliquer que les expressions _Lambda_ n'ont pas été introduites dans Java 8 parce que c'est à la mode ou parce que le code écrit est plus compact.

Les expressions _Lambda_ apporte la possibilité d'appliquer de nouveaux patterns qui permettent de paralléliser simplement et de façon plus sûr des traitements.

## Les slides
Vous y trouverez des détails que je n'ai pas abordé pour rester synthétique :

* Des méthodes implémentées dans des interfaces entrainent la possibilité d'avoir un héritage multiple conflictuelle. Quelles sont les règles du compilateur ?
* Des explications sur quelques classes du package _java.util.functions_
* Les différents états d'un _Stream_ et leurs conséquences
* La problématique des valeurs par défaut dans les réductions _max_ et _min_
* Une utilisation plus avancée de la classe _Collectors_
* Des choses qui ne marchent pas avec le traitement parallèle

Alors, installer vous confortablement et prenez votre temps, il y a 330 slides ;)

<iframe src="http://www.slideshare.net/slideshow/embed_code/29010060" width="800" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

## Qu'est-ce que j'ai pensé de tout ça ?
Dans l'ensemble, j'ai trouvé cette présentation claire, progressive et surtout riche en contenu. Elle présente bien les expressions _Lambda_ et ses impacts sur la future version du JDK.

### _Lambda_ Java 8, une révolution ?

La réponse est probablement oui. C'est la première fois :

* qu'il est possible de déclarer des paramètres sans préciser leurs types (paramètres d'entrée d'une expression _Lambda_).
* qu'une "méthode" (particulière certes) peut être passée en paramètre d'une autre.
* que le langage fait une place aussi importante aux concepts de la programmation fonctionnelle.
* qu'il y aura autant de changement dans le vocabulaire d'un développeur Java. Il parlera désormais avec des mots comme _map_, _filter_, _reduce_, _Supplier_, _Function_, _BiFunction_, _Consumer_, _BiConsumer_, etc. Y aura t-il une race d'intégriste qui va naitre dans la communauté Java comme les _Scalafistes_ de _Scala_ ? ;)

### _Lambda_ Java 8 et le debug ?

Je trouve que le langage Java a une qualité formidable : une maintenance _possible_ sur une grosse volumétrie de code. Même sur des applications dites _legacy_ qui ont été développées de la pire des manières, j'ai toujours pu lancer l'application en debug, faire du pas à pas partout même dans les classes fournies par les librairies. Le côté impératif de la programmation fait clairement apparaître chaque étape du programme.

Avec Java 8, les applications vont de plus en plus ressembler à des enchainements de méthodes avec des expressions _Lambda_ en paramètres. Ce code séduisant va probablement poser des difficultés au debuggage. Va t-il falloir mettre les expressions _Lambda_ systématiquement sur plusieurs lignes pour permettre de faire du pas à pas ? Comment, pendant un debuggage, simuler l'exécution d'une opération sur un _Stream_ sans avoir à en créer un nouveau ? Aurons-nous toujours cette maintenance qui peut être pénible mais toujours possible avec une application qui vieillit avec des expressions _Lambda_ ? Peut-être tout simplement qu'il y aura moins de besoin d'utiliser un debugger ? A suivre ;)

### Fini les boucles _for_ pour les listes ?
Les boucles _for_ servent souvent à itérer sur des listes d'éléments afin de les transformer (_map_), les filtrer (_filter_) et à faire des calculs (_reduce_). Les développeurs vont-ils privilégier l'utilisation des expressions _Lambda_ ?

### _Lambda_ Java 8 vs les langages alternatifs ?
Les langages alternatifs (Scala, Groovy, etc.) de la JVM n'ont pas grand chose à envier aux expressions _Lambda_ de Java 8, ils vont déjà beaucoup plus loin depuis plusieurs années. Ils vont cependant probablement continuer à jalouser la base énorme d'utilisateurs qui reste fidèle à Java _Standard_ ;)

{% blockquote James Gosling, 1997 %}
Java is a blue collar language. It's not PhD thesis material but a language for a job.
{% endblockquote %}

### La date de sortie prévue de Java 8 est le 18/03/2014. Alors, impatient d'avoir Java 8 dans vos entreprises ?
