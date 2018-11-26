# Build Automation

Build Automation est l'automatisation des taches necessaires pour processer et preparer le code source au deploiement en production. C'est un important composant de l'integration continue.

Ceci inclus:
- la compilation
- la gestion des dependences. S'il y a des parties tiers ou des librairies qui ont besoin d'etre present pour compiler, ou tester le code. Ils vont etre inclus dans le meme package.
- l'execution des tests d'automatisations. Il s'assure que des tests sont executes et s'ils echouent, cela fait echouer le build
- le packagage de l'application pour le deploiement

J'ai utilise Maven comme outil d'automatisation de build.

Installer Gradle:
```bash
cd ~/
wget -O ~/gradle-4.7-bin.zip https://services.gradle.org/distributions/gradles-4.7-bin.zip
sudo yum -y install unzip java-1.8.0-openjdk
sudo mkdir /opt/gradle
sudo unzip -f /opt/gradle ~/gradle-4.7-bin.zip
sudo vi /etc/profile.d/gradle.sh
```

Mettre le texte dans gradle.sh:
```bash
export PATH=$PATH:/opt/gradle/gradle-4.7/bin
```

Puis mettre les permissions sur gradle.sh:
```bash
sudo chmod 755 /etc/profile.d/gradle.sh
```

Puis, apres s'etre deconnecte et reconnecte:
```bash
gradle --version
```

Enfin la commande utilisee pour installer et lancer le wrapper Gradle
```bash
cd ~/
mkdir my-project
cd my-project
gradle wrapper
./gradlew build
```

Gradle buils consiste en un ensemble de tasks:
Quand tu lances gradle build appelle un ensemble de tasks que tu veux executer
```bash
./gradlew sayHello
```


This is a simple train schedule app written using nodejs. It is intended to be used as a sample application for a series of hands-on learning activities.

Running the app
It is not necessary to run this app locally in order to complete the learning activities, but if you wish to do so you will need a local installation of npm. Begin by installing the npm dependencies with:

npm install
Then, you can run the app with:

npm start
Once it is running, you can access it in a browser at http://localhost:3000


## Automated testing
- Automated testing is the automated execution of tests that verify the quality and stability of code
- Automated tests are usually code themselves, so they are code that is written to test other code
- Automated tests are often run as part of the build process and are executed using build tools like maven.

There are multiple types of automated tests:
- Unit Tests focus on testing small pieces of code in isolation. Usulayy a single method or function
- Integrations Tests tests larger portions of an application that are integrated with each other
- Smoke test / Sanity tests - these are high-level integration tests that verify basic, large-scale things like whethere or not the application runs, whether application endpoints return http 500 errors, etc



Index page
GET / 200 23.23.ms
  renders successfully
  
Trains API
GET /trains 200 4.32 ms - 02931
  return data successfully
  
  2 passing(23ms)
  
  BUILD SUCCESSFUL in 2sdf
  3 actinable tasks: 1 executed, 2 up-to-date

What Maven does ?
Compilation of Source Code
Running Tests (unit tests and functional tests)
Packaging the results into JAR’s,WAR’s,RPM’s,etc..
Upload the packages to remote repo’s (Nexus,Artifactory)
