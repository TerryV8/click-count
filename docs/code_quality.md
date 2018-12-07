# SonarQube (formerly Sonar)

SonarQube (formerly Sonar) is an open source platform for continuous inspection of code quality. It provides a server component with a bug dashboard which allows to view and analyse reported problems in your source code.

Running SonarQube via Docker is as simple as the following command.
```console
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

You can now login your local Sonar server on http://localhost:9000/ with the admin user and the admin password.

Click on: Create your first project.

This will allow you to create an access token which you batch job can use to update the project.

