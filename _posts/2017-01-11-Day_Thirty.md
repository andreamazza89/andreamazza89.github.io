---
layout: post
title: 8th Light apprenticeship - Day Thirty
categories: 8thLight apprenticeship
---

### Pills
- Java class path and package conventions.
- Started working on a new application: contacts manager.

Today I have been looking into how Java source files and their dependencies are
compiled and run in Java. The task was to manually (from the command line) compile
my implementation of the roman numerals kata and run it. The program should be
able to take an input string (the roman numeral) and print out the result.
This was fairly simple but got me to research a few things.

Java source code is first compiled into a _class_ file. This is something that the
Java Virtual Machine (JVM) can run. One further step you can take is to create
a Java Archive (JAR); this is an archive that includes all that is necessary to run
your whole project in a different machine. An archive puts the whole application
in one file, which can be run on the JVM.

Now, when feeding a program to the JVM, you need to specify a class path and an
entry class. The entry class is the entry point of your program, the one button
that turns the whole thing ON. This class needs to a have a public static method,
called `main`, which will be invoked by the JVM. The JVM needs a directory to
look for all the assets it requires; this is the class path. So for instance, if
the root of your project is located at `~/Users/YourName/MyJavaProject/`
and your entry point is, say `HelloWord.class`, at
`.../MyJavaProject/src/main/java/com/YourDomain/AppName`, then you would enter the following command to
run your program: `java -cp ~/Users/YourName/MyJavaProject/src/main/java/ com.yourdomain.app_name.HelloWord`.
The last field in this command is the entry class, with its full package name.
The package name acts as a name space: to try and avoid duplicate class definitions
when many different libraries are used, it is conventional to use your (or your organisation's)
domain name to prefix the package name, followed by the actual name of the
package. There is this weird entanglement between the actual path in the filesystem
and the package name, which I guess enforces convention so that all projects are
structured in a predictable way.

Later on in the day I had a look at Gradle, one of the automation tools available
for Java. Gradle allows you to run tasks like `compile` or `test`, and define new
tasks as necessary. It also manages project dependencies.

Finally, I got started with the next application I am building, a contacts manager,
where you can store and retrieve contacts (wow!). This will initially be a command line
app, but it will eventually be on a GUI.

Ciao.
