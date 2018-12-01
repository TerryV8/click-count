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

Redis is a key-value store which allows data to be stored and accessed at lightning fast speeds.
Redis holds its database entirely in the memory, using the disk only for persistence.
Redis can replicate data to any number of slaves.

Following are certain advantages of Redis:
- Exceptionally fast: Redis is very fast and can perform about 110000 SETs per
second, about 81000 GETs per second.
- Supports rich data types: Redis natively supports most of the datatypes that
developers already know such as list, set, sorted set, and hashes. This makes it
easy to solve a variety of problems as we know which problem can be handled
better by which data type.
- Operations are atomic: All Redis operations are atomic, which ensures that if
two clients concurrently access, Redis server will receive the updated value.
- Multi-utility tool: Redis is a multi-utility tool and can be used in a number of use
cases such as caching, messaging-queues (Redis natively supports
Publish/Subscribe), any short-lived data in your


# Source Control Management (With Git)

[Read more ...](docs/source_control_management.md)


# Build Automation (With Maven)

[Read more ...](docs/build_automation.md)

# Continous Integration (With Jenkins)

[Read more ...](docs/continuous_integration.md)


# Containerization (With Docker)

[Read more ...](docs/containerization.md)

# Monitoring containers  (With Sonar/Kibana)

[Read more ...](docs/monitoring_containers.md)

# From a single host to multi-hosts (From Docker Compose to Kubernetes)

[Read more ...](docs/kubernetes.md)


# Replication: no failover  (With secure Kubernetes cluster)

[Read more ...](docs/replication.md)

# Scalability: increasing throughput (With Kubernetes pods)

[Read more ...](docs/scalability.md)
