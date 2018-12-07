# SonarQube (formerly Sonar)

SonarQube (formerly Sonar) is an open source platform for continuous inspection of code quality. It provides a server component with a bug dashboard which allows to view and analyse reported problems in your source code.

Running SonarQube via Docker is as simple as the following command.
```console
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```
