# Maven CLI and Packaging Projects

## Learning Goals

- Use the Maven commands for cleaning, compiling, and packaging a project.

## Introduction

Now that we’ve set up our POM file and the basic directory structure, we can
compile and package the project into a JAR file. But before we can do that, we
need to add code to our project.

## Create a Simple Program

We’ll create a simple program to demonstrate compilation and packaging in Maven.
We will be creating an `App.java` file in the `src/main/java/org/example`
directory.

```plaintext
└── ~/developer/example-app
		├── pom.xml
		├── src
		│   ├── main
		│   │   ├── java
		│   │   │   └── org
		│   │   │       └── example
		│   │   │           └── App.java
		│   │   └── resources
		│   ├── target
		│   └── test
		└── target
```

And here’s the content of the `App.java` file:

```java
package org.example;

import java.util.Scanner;

public class App
{
    public static void main( String[] args )
    {
        Scanner sc = new Scanner(System.in);
        System.out.println("What is your name?");
        String name = sc.nextLine();
        System.out.println(greet(name));
        sc.close();
    }

    public static String greet(String name) {
        return String.format("Hello, %s!", name);
    }
}
```

## Cleaning the Target Directory

Before we compile and package the project, we want to remove the `target`
directory. As a reminder, the `target` directory is where the compiled classes
are stored.

Run the `mvn clean` command to completely remove the `target` directory.

## Compiling the Project

We can use the `mvn compile` command to tell Maven to compile our project. This
will create a `target` directory which will have a `classes` directory
containing the compiled class.

## Making a JAR File (Packaging)

We can run the `mvn package` command to build a JAR file. But before we can
successfully run that command, we need to do a couple of things:

1. Add the `maven-jar-plugin` which will create the JAR file.
2. Add configuration to tell Java which .class file to run in the JAR file. In
   our case, it will be the `App` class.

Let’s modify our `pom.xml` file to add the plugin and the config.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>example-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>example-app</name>
  <url>http://www.example.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
  </properties>

  <dependencies>

  </dependencies>

  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.10.1</version>
      </plugin>
			<!-- START NEW SECTION -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.2.2</version>
        <configuration>
          <archive>
            <manifest>
              <mainClass>org.example.App</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
			<!-- END NEW SECTION -->
    </plugins>
  </build>
</project>
```

We have added the following to our POM file:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-jar-plugin</artifactId>
  <version>3.2.2</version>
  <configuration>
    <archive>
      <manifest>
        <mainClass>org.example.App</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
```

The groupId, artifactId, and version tags have been added to add the
`maven-jar-plugin`. The configuration tag includes instructions on which class
to run when the JAR is opened by the user.

We can finally run the `mvn package` command to generate the JAR file in the
`target` folder. Navigate to the `target` directory in your terminal or command
line and run the following command to start the app:

```bash
java -jar example-app-1.0-SNAPSHOT.jar
```

When you run this command, the `App.class` gets executed.

## Conclusion

We’ve learned how to clean the target folder, compile a project, and package
projects into JAR files. In the later lessons, we’ll learn how to work with
external libraries or dependencies.
