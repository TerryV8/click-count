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

### Redis Architecture

[Read more ...](docs/redis_architecture.md)

### Kubernetes Architecture

[Read more ...](docs/kubernetes_architecture.md)


# Source Control Management (With Git)

[Read more ...](docs/source_control_management.md)


# Build Automation (With Maven)

[Read more ...](docs/build_automation.md)

# Continous Integration (With Jenkins)

[Read more ...](docs/continuous_integration.md)


# Containerization (With Docker)

### Docker [Read more ...](docs/containerization.md)

### Docker Compose [Read more ...](docs/docker_composer.md)


# Monitoring containers  (With Sonar/Kibana)

[Read more ...](docs/monitoring_containers.md)

# From a single host to multi-hosts (From Docker Compose to Kubernetes)

[Read more ...](docs/kubernetes.md)


# Replication: no failover  (With secure Kubernetes cluster)

[Read more ...](docs/replication.md)

# Scalability: increasing throughput (With Kubernetes pods)

[Read more ...](docs/scalability.md)
