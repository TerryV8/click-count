# Before you start

- Make sure you are using IntelliJ IDEA ULTIMATE Edition.
- Install the Java SE Development Kit (JDK), version 1.8 or later, see Download Oracle JDK.
- Download the GlassFish application server, version 3.0.1 or later, see Download GlassFish.
- Make sure a web browser is available on your computer.

# Configuring the GlassFish server in IntelliJ IDEA

Open the File / Settings / Build, execution, deployment / Application Servers
and click + an choose GlassFish 4.1.2
and specify the GlassFish Server installation folder in the GlassFish Home field (eg. /opt/glassfish4)

# Configuring the JDK

Open the File / Project structure / Project settings / Project / Project SDK
and select version 1.8
and specify the installation folder of the Java SE Development Kit (JDK) to use. 
(eg. /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64)


# Add Framework support

click right on "main"
and select Add Framework support
and click right on "Java EE"
and select "Java EE 7"
and select RESTFul Web Service / Download

# Exploring the project structure

- The folder src is for your Java source code.

- The folder web is for the web part of your application. At the moment this folder contains the deployment descriptor WEB-INF\web.xml.

- External Libraries include your JDK and the JAR files for working with the GlassFish Server.

# Examining the generated artifact configuration

Besides building a RESTful-specific project structure, IntelliJ IDEA has also configured an artifact for us.

The word artifact in IntelliJ IDEA may mean one of the following:

An artifact configuration, i.e. a specification of the output to be generated for a project;

An actual output generated according to such a specification (configuration).

Let's have a look at this configuration.
Open the Project Structure dialog by pressing ⌘; or choosing File | Project Structure on the main menu.

Under Project Settings, select Artifacts.
The available artifact configurations are shown in the central pane under icons general add svg and icons general remove svg. Currently there is only one configuration rest_glassfish_hello_world:war exploded, it is a decompressed web application archive (WAR), a directory structure that is ready for deployment onto a web server.

The artifact settings are shown in the right-hand pane of the dialog:
