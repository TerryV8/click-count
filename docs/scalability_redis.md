

### Using Kubernetes, we decided to go for a Redis Master-Slaves architecture

There is a master and there are replicas. The master pushes data to replicas. Replicas don’t talk between themselves.

Replicas are Read-Only and will tell you so if you try to do a SET on them (or any Write operation for what it’s worth).

![Redis replicated architecture](https://blog.octo.com/wp-content/uploads/2017/08/screen-shot-2017-08-11-at-14-35-11.png)


- Pros:
  - You always have a hot snapshot of your data
  - It’s a Disaster Recovery Plan (DRP) with Master/Stand-By
- Cons:
  - Write performance is bounded by the master
  - The replicas are not used to their full potential. They can not be use as write node, only read node. In order to achieve high write availability, you need manual operations (changing the master Redis manually and restarting the clients)
  

If you are a little to medium-sized organization and it’s your first Redis deployment, this may be the best trade-off for you.
