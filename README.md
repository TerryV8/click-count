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
  - [for back-end](docs/docker_back-end.md)
  - [for front-end](docs/docker_front-end.md)
- #### [Link network Docker connection between back-end & front-end](docs/docker_networking.md)  
- #### [Docker Compose](docs/docker_compose.md)


## Secure Kubernetes Cluster
- #### [Kubernetes, What is it?:](docs/kubernetes.md) 
  - From Docker Compose to Kubernetes: From a single host to multi-hosts: From Dev to Prod
  
- #### Installation
  - [for Resilience: auto-healing containers, no failover ](docs/replication.md)

- #### Scalable: increasing throughput:
  - in front end:
    - [with Web-app pods ](docs/scalability_web-app.md)
    - [with HAProxy as a load balancer to distribute the incoming traffic ](docs/load_balancer_web-app.md)
  - [network link between web-app, front-end & redis, back-end](docs/link_web-app_to_redis.md)
  - in back end, Master-Slaves Redis architecture:
    - [with a Master pod](docs/scalability_redis.md)
    - [with Slave pods](docs/scalability_redis_slaves.md)

## Automated Deployment

- #### Terraform
    - [What is it?](docs/terraform_setup.md)
    - [with Local setup and IAM, DNS setups on AWS, cloud infrastructure](docs/terraform_iam_dns.md)
    - [with Variables](docs/terraform_variables.md)
    - [with VPC](docs/terraform_vpc.md)
    - [with an Internet Gateway, Public subnet](docs/terraform_public_subnet.md)
    - [with Security groups](docs/terraform_security_groups.md)
    - [with Ssh key pair](docs/terraform_ssh_key_pair.md)
    - [with a Nat instance](docs/terraform_nat_instance.md)
    - [with a Private subnet and routes](docs/terraform_private_subnet_and_routes.md)
    - [with a Load Balancer and adding Application servers](docs/terraform_load_balancer.md)
    - [with S3 bucket](docs/terraform_s3.md)
  
- #### [Ansible](docs/ansible.md)
