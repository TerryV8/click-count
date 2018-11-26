# Docker

Why do we need Docker?
The short list of benefits includes:

- faster development process
- handy application encapsulation
- the same behaviour on local machine / dev / staging / production servers
- easy and clear monitoring
- easy to scale

## Faster development process
There is no need to install 3rd-party apps like PostgreSQL, Redis, or Elasticsearch on the system — you can run them in containers.

Docker also gives you the ability to run different versions of same application simultaneously. For example, say you need to do some manual data migration from an older version of Postgres to a newer version. You can have such a situation in microservice architecture when you want to create a new microservice with a new version of the 3rd-party software.

It could be quite complex to keep two different versions of the same app on one host OS. In this case, Docker containers could be a perfect solution — you receive isolated environments for your applications and 3rd-parties.
