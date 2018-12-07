# Click Count application

[![Build Status](https://travis-ci.org/xebia-france/click-count.svg)](https://travis-ci.org/xebia-france/click-count)

# Homework
## Context - Client needs & Technical environment

The french Startup Click Paradise has developed a click counter application and in a Lean approach
it wants to deliver the evolutions continuously.
You have joined the team as a DevOps profile and your first task is to industrialize
the construction and delivery of the "Click Count" application.

The web application is developed in Java and uses Redis for storing data. The deployments
are always done first on a Staging environment and then, after validation, on the environment of
Production. ...

## Goal

Modify the code of the application and use whatever seems relevant to you to fill
the objectives of the company, namely to deliver quickly and automatically the evolutions in
Production.
The infrastructure of the Staging and Production environments must be configured automatically
in order to ensure the sustainability of the solution. 

[Read more ...](docs/enonce.md)


# Technical choice
- #### [Web-app (Front-end)](docs/web_app.md)

- #### [Redis Architecture (Back-end)](docs/redis_architecture.md)

- #### [Kubernetes Architecture (Back-end & Front-end)](docs/kubernetes_architecture.md)


# Source Control Management

- #### [Git](docs/source_control_management.md)


# Build Automation

- #### Maven [Build](docs/build_automation.md) & [Unit & Integration testing](docs/maven_unit_test.md)

- #### [SonarQube (Code quality)](docs/code_quality.md)

# Continous Integration

- #### [Jenkins](docs/continuous_integration.md)


# Containerization

- #### [Docker, What is it?](docs/docker.md):
  - [for back-end](docs/docker_back_end.md)
  - [for front-end](docs/docker_back_end.md)

- #### [Docker Compose](docs/docker_compose.md)


# Monitoring containers
- #### [ELK/Kibana](docs/monitoring_containers.md)

# Secure Kubernetes Cluster
- #### [From Docker Compose to Kubernetes (From a single host to multi-hosts)](docs/kubernetes.md)

- #### [Replication: no failover](docs/replication.md)

- #### [Scalability: increasing throughput with pods](docs/scalability.md)
