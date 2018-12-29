
# AWS_elasticache_cluster Redis Terraform module for the back

Redis Cluster Mode Enabled
To create two shards with a primary and a single read replica each:

```console
Redis Cluster Mode Enabled
To create two shards with a primary and a single read replica each:
resource “aws_elasticache_replication_group” “redis_rg”  {
    replication_group_id = “rg1”
    replication_group_description = “replication_group for Redis which is configured with 2 shards with  a primary and a single read replica each”
    node_type = “cache.t2.small”
    port = 6379
    parameter_group_name = “default.redis3.2.cluster.on”
    automatic_failover_enabled = true

    cluster_mode  {
        replicas_per_node_group = 1
        num_node_groups = 2
    }
}
```
