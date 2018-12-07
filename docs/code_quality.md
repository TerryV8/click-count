# SonarQube (formerly Sonar)

SonarQube (formerly Sonar) is an open source platform for continuous inspection of code quality. It provides a server component with a bug dashboard which allows to view and analyse reported problems in your source code.

Running SonarQube via Docker is as simple as the following command.
```console
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

You can now login your local Sonar server on http://localhost:9000/ with the admin user and the admin password.
Genera the yourkey token (eg. token_sonar: 6590b072344f241ddf649121349593e15f09a0d1)
Click on: Create your first project.

This will allow you to create an access token which you batch job can use to update the project.

### Analyse a Maven project

To analyse a Maven project use the following command:
```console
mvn sonar:sonar   -Dsonar.host.url=http://localhost:9000   -Dsonar.login=yourkey
```

Create HTML reports
Your can generate HTML reports in the preview analysis mode.
```console
mvn sonar:sonar -Dsonar.analysis.mode=preview -Dsonar.issuesReport.html.enable=true
```
eg.
```console
mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=6590b072344f241ddf649121349593e15f09a0d1
```


Thanks to Sonar we build a clean quality code.
In particular, we removed commented lines after checking on Sonar.
Actually, I made some tests of the REST API of the web App, before establishing the connection to the Redis Database.

![code_quality_sonar](/images/code_quality_sonar.png)


