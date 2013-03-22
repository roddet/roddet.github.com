---
layout: post
title: "Manipulez le bytecode"
date: 2012-06-21 22:45
comments: true
categories: bytecode java maven byteman
---

## Java et la philosophie du paquet cadeau
Nous le savons, avec Java, nous travaillons avec un langage compilé. Notre code écrit en Java est transformé en bytecode (stocké dans des fichiers .class) pour ensuite être exécuté par la JVM. En plus de cette notion de compilation, Java ajoute une philosophie de "packaging". Nous nous plaisons bien dans cette philosophie de packaging parce que nous développons avec de nombreuses ressources et le fait d'avoir un paquet cadeau qui représente le fruit de notre travail nous facile la tâche lorsque l'on veut l'offrir à un environnement d'exécution.
<table><tr>
  <td width="30%"><img src="{{root_url}}/images/manipbytecode/cadeau.png"/></td>
  <td width="70%">Cette philosophie du paquet cadeau a néanmoins un gros inconvénient, il n'est pas simple de le modifier même pour des ajustements minimes. Par exemple, votre équipe de test a qualifié votre livrable mais vous aurez bien voulu tracer l'exécution d'une méthode critique. Pour le faire, vous êtes traditionnellement obligé de recompiler, refaire le paquet cadeau et là vous hésitez ! Vous n'êtes jamais sûr à 100% que vous n'avez pas introduit de régression et vous savez qu'en cas de problème retirer votre code va vous obliger à refaire votre paquet cadeau pour la production.
<br/>Un autre exemple, vous avez un bug que vous ne reproduisez qu'en environnement de production, vous auriez aimé tracer l'exécution d'une méthode pour investiguer. Là aussi vous partez sur la constitution d'un nouveau cadeau avec le risque d'introduire encore plus de regression et le retour arrière ne sera pas aisé à mettre en oeuvre.</td>
</tr></table>
<br/>
## Notre façon de coder influencée par le packaging
Le fait de savoir qu'en production une modification du code est extremement difficile à faire passer, nous allons mettre un peu de tout et n'importe quoi dans notre application.
Nous allons notamment ajouter du code de debug comme :
```java
if(logger.isDebugEnabled()){
   logger.debug("Au cas où j'aurai un bug...");
}
```
Ou encore pour mesurer des temps d'exécution, on va ajouter du code en début et fin de méthode.
Ces parties de code qui n'ont rien avoir avec notre application, nous dépensons de l'énergie pour les écrire et surtout nous ne savons pas souvent s'ils sont pertinents. Nous essayons d'être le plus générique possible et à chaque fois nous nous faisons avoir... La trace n'est pas adapté à notre cas et nous devons modifier le code, refaire le paquet cadeau. Et pire, combien de NullPointerException nous avons dû corriger sur ces morceaux de code qui n'ont rien avoir avec le fonctionnement de notre application ?


## Java 5 et ses agents "secrets" pour nous aider
Depuis le JDK 1.5, nous avons la possibilité d'instrumenter un programme. J'entends ici, par instrumenter, la possibilité d'ajouter des instructions à nos classes déjà compilées (le bytecode) au runtime. Cette instrumentation peut être effectuée par le biais d'un agent que l'on fournit au lancement d'une application. Un agent se présente sous la forme d'un JAR et est fourni grâce au paramètre -javaagent:.
Nous progressons dans notre problématique, avec le mécanisme des agents nous pouvons séparer le code de l'application du code nous permettant de faire nos analyses. Nous ne sommes plus obligés de modifier le paquet cadeau généré, validé par nos équipes de test. C'est une avancée intéressante, car si notre code périphérique introduit des regressions, il suffit de redémarrer l'application sans l'agent pour supprimer la regression.

## Notre premier agent
Pour qu'un JAR puisse être utilisé comme un agent, il faut qu'il reste certaines conditions :
Avoir dans son fichier MANIFEST au moins l'attribut Premain-Class
L'attribut Premain-Class doit avoir comme valeur une classe existante qui contient au moins la méthode
``` java
public static void premain(String agentArgs, Instrumentation instr){
 
}
```
Pour créer un projet Java générant un JAR, je ne sais plus comment on fait sans Maven :-) L'exemple qui suit reste évidemment valable à condition de l'adapter à votre outil de génération de JAR (Eclipse, Netbeans, JDK, Ant, vos mains,...).

* Création du projet agent-simple
```
mvn archetype:generate -DgroupId=com.roddet -DartifactId=agent-simple -Dversion=1.0
```

* Créer une classe qui sera le point d'entrée de notre agent
```java
package com.roddet.agent.simple;
 
import java.lang.instrument.Instrumentation;
 
public class MySimpleAgent {
 
    public static void premain(String agentArgs, Instrumentation instr){
    }
}
```
Pour mettre à jour le fichier MANIFEST avec Maven on peut ajouter la configuration du plugin maven-jar-plugin comme suit :
``` xml
<build>
  <pluginmanagement>
    <plugins>
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-jar-plugin</artifactid>
        <version>2.4</version>
        <configuration>
          <archive>
            <manifestentries>
              <premain-class>com.roddet.agent.simple.MySimpleAgent</premain-class>
            </manifestentries>
          </archive>
        </configuration>
      </plugin>
    </plugins>
  </pluginmanagement>
</build>
```
Il ne reste plus qu'à écrire du code dans la méthode premain de la classe MySimpleAgent et lancer une application avec le paramètre -javaagent.

## Un exemple concret de l'utilisation d'un agent
Pour illustrer la modification du bytecode, nous allons considérer une application basique faite avec Swing qui affiche un champ de saisie et un bouton.

![/images/manipbytecode/swing-app.png](/images/manipbytecode/swing-app.png)

La classe principale de cette application est la suivante :
``` java
package com.roddet.swing.app;
 
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.IOException;
 
public class SwingFrame extends JFrame implements ActionListener {
 
    private JTextField swingText = new JTextField(10);
    private JButton swingBtn = new JButton("Click!");
 
    public SwingFrame() {
        init();
    }
 
    private void init() {
        this.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        swingBtn.addActionListener(this);
        this.setLayout(new GridLayout());
        this.add(swingText);
        this.add(swingBtn);
        this.pack();
    }
 
    @Override
    public void actionPerformed(ActionEvent actionEvent) {
 
    }
 
 
    public static void main(String[] args) throws IOException {
        JFrame app = new SwingFrame();
        app.setVisible(true);
    }
}
``` 
Lors du clic sur le bouton "Click!", la méthode actionPerformed est exécuté. Pour le moment, ce clic ne produit aucun résultat.
Nous créons un paquet "cadeau" de cette application qui s'appelle swing-app-1.0.jar.

Notre objectif avec cet exemple est de modifier le comportement du clic sur le bouton pour qu'il affiche du texte dans la zone de saisie sans modifier notre paquet.

Pour lancer l'application, il suffit d'exécuter la commande :
```
java -jar swing-app-1.0.jar
```
Pour exécuter l'application avec notre agent :
```
java -javaagent:agent-simple-1.0.jar -jar swing-app-1.0.jar
```
Pour le moment, il ne se passe rien (pas de comportement spécifique défini dans l'agent).
Pour nous assurer que le code de notre agent est bien exécuté, nous ajoutons le traditionnel System.out.println dans notre agent comme ceci :

``` java
public class MySimpleAgent {
    public static void premain(String agentArgs, Instrumentation instr){
      System.out.println("I'm your secret agent");
    }
}
```
A l'exécution, la chaine de caractère s'affiche, rien de bien intéressant pour le moment. 

La modification du bytecode d'une application passe par le mécanisme de "transformer" que l'on peut ajouter à notre objet instr de type java.lang.instrument.Instrumentation. Un transformer est une classe qui implémente l'interface java.lang.instrument.ClassFileTransformer.

Créons alors notre "transformer" MySimpleTransformer avec un affichage dans la console des classes sur lesquels on peut intervenir :
``` java
package com.roddet.agent.simple;
 
import java.lang.instrument.ClassFileTransformer;
import java.lang.instrument.IllegalClassFormatException;
import java.security.ProtectionDomain;
 
public class MySimpleTransformer implements ClassFileTransformer {
    @Override
    public byte[] transform(ClassLoader classLoader, String className, Class aClass,
                                 ProtectionDomain protectionDomain, byte[] bytes)
                                                  throws IllegalClassFormatException {
      System.out.println("I can modify class : " + className);
      return bytes;
    }
}
```
Et appliquons notre transformer à l'objet instr
``` java
public class MySimpleAgent {
    public static void premain(String agentArgs, Instrumentation instr) {
        System.out.println("I'm your secret agent");
        instr.addTransformer(new MySimpleTransformer());
    }
}
```
A l'exécution, on obtient :
```
   I'm your secret agent
   I can modify class : com/roddet/swing/app/SwingFrame
   I can modify class : javax/swing/JFrame
   I can modify class : javax/swing/WindowConstants
   I can modify class : javax/accessibility/Accessible
   I can modify class : javax/swing/RootPaneContainer
   I can modify class : javax/swing/TransferHandler$HasGetTransferHandler
   I can modify class : java/awt/Frame
   ..... (la liste est longue)
```
On remarque que l'on peut modifier non seulement notre classe mais aussi les classes du JDK chargées.
Pour modifier le bytecode d'une classe, il faut modifier le tableau byte[] retourné par la méthode transform au moment où elle est exécutée pour notre classe.

C'est là où ça se complique ...:-) Qu'est-ce que l'on va mettre dans ce tableau byte[] ? Comment va t-on construire un tableau de byte conforme au spécification du bytecode Java (bon magic number, version de java, bytecode, ...) ?
Heureusement plusieurs techniques/outils nous permettent de nous simplifier la tâche ou de tricher un peu.
On va commencer par la triche :-)
Nous pouvons modifier le code SwingFrame en complétant la méthode actionPerformed :
```java
public void actionPerformed(ActionEvent actionEvent) {
  this.swingText.setText(new Date().toString());
}
```
Nous pouvons récupérer le fichier SwingFrame.class correspondant et le mettre dans le répertoire de ressources (src/main/resources) du projet maven agent-simple. Ce fichier sera alors dans le classpath d'exécution de notre agent.

Nous pouvons modifier notre transformer comme suit pour qu'il remplace le bytecode de la classe SwingFrame par celui que l'on vient de générer : 
``` java
public class MySimpleTransformer implements ClassFileTransformer {
 
    @Override
    public byte[] transform(ClassLoader classLoader, String s, Class aClass,
                            ProtectionDomain protectionDomain, byte[] bytes)
                            throws IllegalClassFormatException {
        if (s.equals("com/roddet/swing/app/SwingFrame")) {
            try {
                bytes = getSwingFrameModifiedByteCode();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return bytes;
    }
 
    private byte[] getSwingFrameModifiedByteCode()
                                   throws IOException, URISyntaxException {
        InputStream fileInputStream = Thread.currentThread().getContextClassLoader()
                                         .getResourceAsStream("SwingFrame.class");
        ByteArrayOutputStream output = new ByteArrayOutputStream();
        byte[] buffer = new byte[1024 * 4];
        int n = 0;
        while (-1 != (n = fileInputStream.read(buffer))) {
            output.write(buffer, 0, n);
        }
        fileInputStream.close();
        return output.toByteArray();
    }
}
```
En exécutant sans agent (java -jar swing-app-1.0.jar), un clic sur le bouton "Click" ne produit rien comme résultat.

Avec une exécution avec agent 
```
java -javaagent:agent-simple-1.0.jar -jar swing-app-1.0.jar
```
, la date/heure courante s'affiche après un clic sur le bouton.

![/images/manipbytecode/swing-app-with-agent.png]

Le JDK 5 nous offre ainsi une flexibilité sur notre application en production. Nous pouvons modifier un ensemble de classes et les remplacer directement en production en redémarrant l'application avec un agent. En cas de regression, il suffit de retirer l'agent.


## Java 6 va encore plus loin
Depuis Java 6, il est possible d'instrumenter l'exécution d'une application après son lancement. Le package com.sun.tools.attach a été ajouté et fournit une API permettant d'ajouter, de supprimer un agent. Pour montrer l'utilisation de cet API, nous allons créer un programme permettant de connecter un agent à un programme en cours d'exécution.

Créons un projet Maven agent-live-installer
```
mvn archetype:generate -DgroupId=com.roddet -DartifactId=agent-live-installer -Dversion=1.0
```
Créons une classe principale qui fait le lien entre un agent et le pid d'une application en cours d'exécution.
``` java
package com.roddet.agent;
 
import com.sun.tools.attach.VirtualMachine;
 
public class MyAgentLiveInstaller {
 
    public static void main(String[] args) throws Exception {
        VirtualMachine vm = null;
        try {
            // 2 parameters for this execution
            if(args.length < 2) {
                throw new RuntimeException("Two parameters is mandatory : "
                                                      + "pid and agent JAR path");
            }
 
            String pid = args[0];
            String agent = args[1];
 
            System.out.println("Attaching agent " + agent
                                    + " to application with pid " + pid);
 
            // Attach VM to running application with his pid
            vm = VirtualMachine.attach(pid);
 
            // load agent into target VM
            vm.loadAgent(agent);
 
            System.out.println("Agent loaded + " + vm.getAgentProperties());
 
        } finally {
            if(vm != null) {
                vm.detach();
            }
        }
 
    }
}
```
Pour qu'un agent puisse être chargé dynamiquement, deux conditions :
la classe principale de l'agent doit avoir une méthode agentmain comme ci-dessous :
``` java

public class MySimpleAgent {
 
    public static void premain(String agentArgs, Instrumentation instr) {
        System.out.println("I'm your secret agent");
        instr.addTransformer(new MySimpleTransformer());
    }
 
    public static void agentmain(String agentArgs, Instrumentation instr) throws
                                                       UnmodifiableClassException {
        System.out.println("I'm your dynamic secret agent");
        instr.addTransformer(new MySimpleTransformer(), true);
        instr.retransformClasses(getModifiedClasses(instr));
    }
 
    private static Class[] getModifiedClasses(Instrumentation instr) {
        List<Class<?>> modifiableClasses = new ArrayList<Class<?>> ();
        for(Class aClass : instr.getAllLoadedClasses()) {
            if(instr.isModifiableClass(aClass)){
                modifiableClasses.add(aClass);
            }
        }
        Class[] classesTab = new Class[modifiableClasses.size()];
        int i = 0;
        for (Class aClass : modifiableClasses){
            classesTab[i] = aClass;
            i++;
        }
        return classesTab;
    }
}
```
le fichier MANIFEST doit avoir l'attribut Agent-Class. Nous pouvons l'ajouter à la configuration Maven. Nous ajoutons également les attributs Can-Redefine-Classes et Can-Retransform-Classes pour donner les droits de modifications à l'agent.

``` xml
<build>
  <pluginmanagement>
    <plugins>
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-jar-plugin</artifactid>
        <version>2.4</version>
        <configuration>
          <archive>
            <manifestentries>
              <premain-class>com.roddet.agent.simple.MySimpleAgent</premain-class>
              <agent-class>com.roddet.agent.simple.MySimpleAgent</agent-class>
              <can-retransform-classes>true</can-retransform-classes>
              <can-redefine-classes>true</can-redefine-classes>
            </manifestentries>
          </archive>
       </configuration>
      </plugin>
    </plugins>
  </pluginmanagement>
</build>
```
On peut exécuter l'application swing seule :
```
java -jar swing-app-1.0.jar
```
Pour avoir la liste des processus java tournant sur une machine, on peut utiliser la commande :
```
jps

   804
   1913 Jps
   1907 swing-app-1.0.jar
```
A ce stade si nous cliquons sur le bouton "Click!", il ne se passe rien.

Ensuite nous pouvons utiliser le programme agent-live-installer pour "attacher" un agent à un processus en cours d'exécution.
```
java -jar agent-live-installer-1.0.jar 1907 agent-simple-1.0.jar
```
Et là, sans redémarrer l'application swing-app, un clic sur bouton affiche la date/heure.
Il est également possible aussi de créer un programme qui détache un agent d'un processus.

Avec Java 6, nous devenons tout puissant. Nous pouvons modifier du code "en live" et retirer lorsque nous le souhaitons. La modification après le lancement de l'application a néanmoins des limites, on ne peut pas par exemple modifier la structure d'une classe.

## Quoi pour modifier le bytecode ?
La procédure présentée précédemment permet d'adresser plusieurs cas mais elle n'est pas très pratique notamment pour instrumenter des classes dont nous n'avons pas les sources. Heureusement plusieurs outils peuvent nous aider à modifier le bytecode. Ils sont nombreux, voici les 3 plus populaires :

* [ASM](http://asm.ow2.org/) : propose une API de relativement bas niveau pour modifier nos classes. Il est à la base de nombreux frameworks comme Oracle TopLink, Cobertura, Groovy, JRuby, ... Une liste plus exhaustive est disponible ici.
* [AspectJ](http://www.eclipse.org/aspectj/doc/released/progguide/index.html) : pionnier de la programmation orientée aspect, il permet de définir des comportements transverses de manière agréable (java ou xml) à une application. Il fournit un agent auquel on adjoint une configuration.
* [Byteman](http://www.jboss.org/byteman)  : produit open source développé par JBoss qui permet de modifier le bytecode au démarrage d'une application ou sur une application en cours d'exécution. L'instrumentation d'une application se fait par le biais de règles très facile à lire et à mettre en oeuvre.

## Quelques exemples avec Byteman
Pour installer Byteman, télécharger puis décompresser le binaire ici.
Idéalement ajouter le répertoire [BYTEMAN_INSTALL]/bin dans le "Path" de votre système.

Règle 1 : tracer le clic sur le bouton "Click!"
Créer le fichier trace-click-swing-app.btm avec le contenu :
```
RULE trace click
CLASS com.roddet.swing.app.SwingFrame
METHOD actionPerformed(java.awt.event.ActionEvent)
IF TRUE
DO System.out.println("Click!");
ENDRULE
```
Lancer l'application swing-app : 
```
java -jar swing-app-1.0.jar
```

![/images/manipbytecode/swing-app-without-agent.png](/images/manipbytecode/swing-app-without-agent.png)

Un clic sur le bouton ne produit aucun résultat.

Utiliser la commande jps pour récupérer le pid du processus puis installer l'agent byteman avec la script bminstall.sh :
```
bminstall.sh 2276
```
Puis appliquer la règle trace-click-swing-app.btm :
```
bmsubmit.sh trace-click-swing-app.btm
```
Et là, quand on clique sur le bouton "Click!", la trace "Click!" apparait dans la console.

Règle 2 : mettre à jour le champ de saisie après un clic
Créer le fichier addText-swing-app.btm avec le contenu :
```
RULE addText on click
CLASS com.roddet.swing.app.SwingFrame
METHOD actionPerformed(java.awt.event.ActionEvent)
IF TRUE
DO $0.swingText.setText("Hello, it's magic!!")
ENDRULE
```
Le mot clé $0 est l'équivalent de "this".

L'exécution de bminstall.sh n'est nécessaire qu'un seule fois, on directement ajouter un deuxième agent.

Appliquer la règle addText-swing-app.btm :
```
bmsubmit.sh addText-swing-app.btm
```
Lors du clic sur le bouton, le message "Hello, it's magic!!" s'affiche.

![/images/manipbytecode/byteman-swing-app.png](/images/manipbytecode/byteman-swing-app.png)

[Byteman](http://www.jboss.org/byteman) permet d'aller loin dans l'écriture des règles notamment avec :
l'utilisation d'expression régulière pour définir un ensemble de classe
le choix plus précis de l'endroit à instrumenter grâce aux mots clés AT ENTRY, AT EXIT, AT LINE, AFTER READ, etc...
de retirer "à chaud" un agent chargé
de manipuler les paramètres d'entrées de méthode
une intégration avec JUnit pour nous aider à simuler des erreurs inattendues (exception due à une coupure réseau, ...)

## Qu'est-ce qu'on fait maintenant ?
Nous avons vu qu'avec Java 5, nous étions capable de séparer le code de l'application aux problématiques transverses comme le logging. Avec Java 6, on peut effectuer des modifications de bytecode à chaud. Cette fonctionnalité est de plus en plus utilisée par des applications tierces, par exemple avec l'apparition de l'onglet "profiler" dans jvisualvm :

![/images/manipbytecode/jvisualvm-profiler.png](/images/manipbytecode/jvisualvm-profiler.png)

N'hésitons pas à :

* exploiter les possibilités de notre JVM
utiliser les outils de programmation orienté aspect comme Spring, AspectJ, etc..
* nous servir des outils comme [Byteman](http://www.jboss.org/byteman) pour des analyses sans devoir défaire notre paquet cadeau.

Le code utilisé est disponible là : [https://github.com/roddet/manip-bytecode](https://github.com/roddet/manip-bytecode).