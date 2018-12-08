# Homework: Click Count application

- ## Context - Client needs & Technical environment

The french Startup Click Paradise has developed a click counter application and in a Lean approach
it wants to deliver the evolutions continuously.
You have joined the team as a DevOps profile and your first task is to industrialize
the construction and delivery of the "Click Count" application.

The web application is developed in Java and uses Redis for storing data. The deployments
are always done first on a Staging environment and then, after validation, on the environment of
Production. ...

- ## Goal

Modify the code of the application and use whatever seems relevant to you to fill
the objectives of the company, namely to deliver quickly and automatically the evolutions in
Production.
The infrastructure of the Staging and Production environments must be configured automatically
in order to ensure the sustainability of the solution. 

[Read more ...](docs/enonce.md)

# My solution, below:
## Let's design the architecture
- #### [Redis Architecture (Back-end)](docs/redis_architecture.md)
- #### [Web-app (Front-end)](docs/web_app.md)

## Source Control Management
- #### [Git](docs/source_control_management.md)

## Build Automation
- #### [Maven Build](docs/build_automation.md)
- #### [JUnit Unit & Integration testing](docs/maven_unit_test.md)
- #### [SonarQube: Code quality](docs/code_quality.md)

## Continous Integration
- #### [Jenkins](docs/continuous_integration.md)

## Containerization
- #### [Docker, What is it?](docs/docker.md):
  - [for back-end](docs/docker_back_end.md)
  - [for front-end](docs/docker_back_end.md)
- #### [Docker Compose](docs/docker_compose.md)

## Monitoring containers
- #### [ELK/Kibana](docs/monitoring_containers.md)

## Secure Kubernetes Cluster
- #### [Kubernetes, What is it?:](docs/kubernetes.md) 
  - ##### From Docker Compose to Kubernetes: From a single host to multi-hosts: From Dev to Prod
  
- #### Installation
  - ##### [for Resilience: auto-healing containers, no failover ](docs/replication.md)

- #### Scalable: increasing throughput with: (docs/scalability.md)
  - ##### [Web-app pods](docs/scalability_web-app.md)
  - ##### [Redis pods](docs/scalability_redis.md)


## Automated Deployment
- #### Ansible
- #### AWS: Terraform
