

### Using Kubernetes, we decided to go for a Redis Master-Slaves architecture

There is a master and there are replicas. The master pushes data to replicas. Replicas don’t talk between themselves.
Replicas are Read-Only.

![Redis replicated architecture](https://blog.octo.com/wp-content/uploads/2017/08/screen-shot-2017-08-11-at-14-35-11.png)


- Pros:
  - You always have a hot snapshot (permanent store) of your data
  - It’s a Disaster Recovery Plan (DRP) with Master - Slave Stand-By
- Cons:
  - Write performance is bounded by the master
  - The replicas are not used as write node, only read node. 
