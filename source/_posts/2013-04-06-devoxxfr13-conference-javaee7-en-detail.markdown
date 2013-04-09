---
layout: post
title: "Devoxx France 2013 # Conférence # Java EE 7 plus en détail"
date: 2013-04-06 14:14
comments: true
categories: devoxxfr2013 java websocket json batch
---

Vous pouvez retrouvez la description de la session [sur le site de Devoxx France](http://www.devoxx.com/display/FR13/Java+EE+7+en+detail).

{% img center http://blog.roddet.com/images/devoxxfr13/javaee7detail/affiche.jpg %}

## Pourquoi j'ai choisi cette session ?
En tant que développeur Java EE depuis plusieurs années, je souhaitais avoir les dernières nouvelles sur la version 7 à venir.

## Le présentateur
David Delabassee [@delabassee](https://twitter.com/delabassee) ([Bio](http://www.devoxx.com/display/FR13/David+Delabassee))


## Le succès de Java EE 6
La session commence par un rappel du succès de Java EE 6 :

* Plus de 50 millions de téléchargements de composants Java EE 6
* Choix N°1 pour le développement en entreprise
* La version la plus rapide de Java EE

## Le périmètre de Java EE 7
Java EE 7 a deux objectifs :

* Améliorer la productivité des développements Java EE
* Apporter un support de HTML 5 (WebSockets, JSON, Formulaires)

Voici un vue d'ensemble de Java EE 7 :

{% img center http://blog.roddet.com/images/devoxxfr13/javaee7detail/overview.jpg %}

En vert les nouveaux modules :

* JSR 236 : Concurrency Utilities
* JSR 352 : Batch Applications
* JSR 353 : Java API for JSON
* JSR 356 : Java API for WebSocket

En orange les modules qui subissent les évolutions majeures :

* JAX-RS 2.0
* EL 3.0
* JMS 2.0

En gris les modules mis à jour.

## Java API for JSON
Cette nouvelle API permettra de parser et générer du JSON. Lors de sa 1ère version, le binding JSON vers objet Java ne sera pas présent. Il est prévu dans une version ultérieure.

Ce nouveau composant offrira une API en Streaming permettant de traiter de gros documents JSON.

## Java API for Websocket
Elle est composée de 2 parties : client & serveur.

### Exemple de partie serveur
``` java
import javax.websocket.*;

@ServerEndPoint("/hello")
public class HelloBean {
	
	@OnMessage
	public String sayHello(String name){
		return "Hello " + name;
	}
}
```

### Exemple de partie cliente
``` java
@ClientEndPoint
public class HelloClient {
	@OnMessage
	public void message(String message, Session session) {
		// traitement de messages provenant du serveur
	}
}
```
Pour lancer la connexion du client au serveur
```java
WebSocketContainer c = ContainerProvider.getWebSocketContainer();
c.connectToServer(HelloClient.class, "hello")
```

## Batch Applications
Il s'agit ici d'apporter un standard dans la création des batchs en Java.

Ce composant apporte les concepts suivants :

* Job : Le processus du batch dans sa globalité
* Step : un traitement indépendant d'un batch
* JobOperator : Gestion de l'exécution du batch
* JobRepository : Métadonnées des jobs

Il y a 2 types de step :

* "Chunked" un traitement standard utilisant le pattern "reader-processor-writer"
* "Batchlet" un traitement spécifique utilisant notre propre pattern

Un job est paramétrable via un fichier XML. Exemple :
```xml
<job id="myJob">
	<step id="init">
		<chunk reader ref="MyReader" processor ref="MyProcessor" writer ref="MyWriter"/>
		<next on="initialized" to="process" />
		<fail on="initError"/>
	</step>
	<step id="process">
		<batchlet ref="ProcessAndEmail" />
		<end on="success"/>
		<fail on="*" exit-status="FAILURE"/>
	</step>
</job>
```
Il est possible de mettre en place des intercepteurs à différents niveaux (JobListener, StepListener, ChunkListener, etc...).

Pour lancer un Job :
```java
try {
	JobOperator jop = BatchRuntime.getJobOperator();
	long jobId = jop.start("myJob"); // META-INF/batch.xml
} catch(JobStartException e) {
	
}

```
Il est possible de lancer les traitements d'un job en parallèle.

## Concurrency Utilities for Java EE
L'objectif de cette JSR est d'offrir la possibilité d'exécuter des traitements concurrents dans un container Java EE sans compromettre son intégrité.
Elle sera cohérente avec l'API existante en Java SE (java.util.concurrent.*) en fournissant une version "managed" de java.util.concurrent.ExecutorService : ManagedExecutorService (récupérable via JNDI).

## JAX-RS 2.0

### La partie cliente entre dans la standard. 
Exemple :
```java
Client client = ClientFactory.newClient();

String name = client.target(".../orders/{orderId}/customer")
					.resolveTemplate("orderId", "10")
					.queryParam("shipped", "true")
					.request()
					.get(String.class)
```

### Des requêtes asynchrones possibles pour la partie cliente
```java
Client client = ClientFactory.newClient();

Future<String> future = client.target(".../orders/{orderId}/customer")
					.resolveTemplate("orderId", "10")
					.queryParam("shipped", "true")
					.request()
					.async()
					.get(
						new InvocationCallBack<String>(){
							public void completed(String result){

							}
							public void failed(InvocationException e){

							}
						}
					)
```


### Possibilité de créer des intercepteurs
```java
public class MyInterceptor implements ReaderInterceptor {
	@Override
	Object aroundReadFrom(ReaderInterceptorContext ctx){
		// Traitement de l'intercepteur
	}
}
```

## JMS 2.0
Ca faisait longtemps que JMS n'avait pas évolué. 

L'objectif de Java EE 7 sur ce point :

* Simplifier l'API
* Offrir la possibilité d'injecter des ressources
* Les objets Connection, Session, etc... sont AutoCloseable (plus besoin de fermer explicitement ces ressources)

### Injection de ressources par annotations
```java
@Resource(lookup = "jms/connFactory")
ConnectionFactory cf;

@Resource(lookup = "jms/inboundQueue")
Destination dest;
```

### Création de JMSContext
C'est une combinaison de la Connection et la Session.

```java
try(JMSContext context = cf.createContext();){
	//
}

```

### On peut envoyer un String directement sans passer par un objet spécifique
```java
try(JMSContext context = cf.createContext();){
	context.createProducer().send(dest,"Hello");
}
```

### JMSContext peut être injecté
```java
@Inject
JMSContext context;
```

## Bean Validation 1.1

### Validation des paramètres d'une méthode
```java
public void myMethod(
	@NotNull String name, 
	@NotNull @Max("10")){
	//
}

```

### Validation du résultat d'une méthode
```java
@Future
public Date getProchainDevoxxFrance(){
	//
}
```

## Et bien sûr les autres nouveautés :

* JSF 2.2 : support HTML5, Composant d'upload de fichier, ...
* Servlet 3.1 : IO Non bloquant, ...
* JPA 2.1 : Génération de schéma, ...
* JTA 1.2 : déclaration de transaction en dehors des EJB, etc...
* CDI 1.1 : Ordonnancement des intercepteurs, ...
* EJB 3.2 : Asynchronisme, ...
* EL 3.0 : Support des lambdas, Collection, ...
* JavaMail 1.5

## Petite réorganisation des profils Java EE

### Web Profile

* Servlet, JSF, JSP
* WebSocket, JSON-P
* JAX-RS
* EL, Beans Validation
* EJB (Lite), CDI, JTA, JPA, ...


### Plateforme complète

* Web Profile
* Concurrency Utilities, Batch API
* JMS, EJB, JAX-WS, JAXB, JavaMail, ...

## Liens utiles

* [Java EE 7](http://glassfish.org/javaee7)
* [The Aquarium](https://blogs.oracle.com/theaquarium/)
* [Adopt-A-JSR](http://glassfish.java.net/adoptajsr/)
* [FishCat](http://glassfish.java.net/fishcat/)

## RoadMap à court terme
Les spécifications seront validées à la fin du mois d'Avril. La première version de GlassFish stable sortira quelques jours après.
JCache n'est pas dans le périmètre de Java EE 7.

## Bilan
Session interressante qui présente en 1h les grandes nouveautés attendues pour Java EE 7. Il ne reste plus qu'à approfondir chaque sujet et tester :) 

