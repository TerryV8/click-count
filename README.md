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

- #### [Terraform, What is it?](docs/terraform_setup.md)
    - [What we plan to build from Infrastructure as Code](docs/terraform_build.md)
    - Presets:
      - [with Local setup & cloud infrastructure AWS credential setup: IAM](docs/terraform_iam.md)
      - [with Variables](docs/terraform_variables.md)
      
    - Network:
      - [with VPC](docs/terraform_vpc.md)
      - [with a Public subnet for the front](docs/terraform_public_subnet.md)
      - [with a Private subnet for the back](docs/terraform_private_subnet.md)
      - [with a Route table for routing and Internet gateway for internet](docs/terraform_routing.md)

    - Security:  
      - [with AWS Ssh key pair](docs/terraform_ssh_key_pair.md)
      - [with Security groups for:](docs/terraform_security_groups.md)
        - [the layer load balancer](docs/terraform_security_groups_load_balancer.md)
        - [the layer front](docs/terraform_security_groups_front.md)
        - [the layer back](docs/terraform_security_groups_back.md)
      
    - Servers:
      - [with 1 Load Balancer](docs/terraform_instance_load_balancer.md)
      - [with 3 EC2 for the front](docs/terraform_instance_public.md)
      - [with AWS_elasticache_cluster Redis Terraform module for the back](docs/terraform_instance_private.md)
      - [with a Nat instance](docs/terraform_nat_instance.md)
      - [with a Load Balancer and adding Application servers](docs/terraform_load_balancer.md)
      - [with S3 bucket](docs/terraform_s3.md)
      
    - DNS
      - [with DNS Route53](docs/terraform_dns.md)

- #### [Ansible](docs/ansible.md)
    - [Setup Docker playbook](docs/ansible_playbook.md)

