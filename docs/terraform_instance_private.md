# Define the Redis architecture in AWS

Replication: Redis (cluster mode disabled) vs. Redis (cluster mode enabled)

Beginning with Redis version 3.2, you have the ability to create one of two distinct types of Redis clusters (API/CLI: replication groups). A Redis (cluster mode disabled) cluster always has a single shard (API/CLI: node group) with up to 5 read replica nodes. A Redis (cluster mode enabled) cluster has up to 90 shards with 1 to 5 read replica nodes in each.



![cluster_mode_disabled](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-NodeGroups.png)


Redis (cluster mode disabled) and Redis (cluster mode enabled) clusters

The following table summarizes important differences between Redis (cluster mode disabled) and Redis (cluster mode enabled) clusters.

Comparing Redis (cluster mode disabled) and Redis (cluster mode enabled) Clusters

Feature |	Redis (cluster mode disabled)|	Redis (cluster mode enabled)
--- | --- | ---
Modifiable |	Yes. Supports adding and deleting replica nodes, and scaling up node type.	| Limited. For more information, see Upgrading Engine Versions and Scaling Redis (cluster mode enabled) Clusters.
Data Partitioning	| No |	Yes
Shards |	1	| 1 to 90
Read replicas	| 0 to 5. Important. If you have no replicas and the node fails, you experience total data loss. | 0 to 5 per shard. Important. If you have no replicas and a node fails, you experience loss of all data in that shard.
Multi-AZ with Automatic Failover |	Yes, with at least 1 replica. Optional. On by default. | Yes. Required.
Snapshots (Backups)	| Yes, creating a single .rdb file.	| Yes, creating a unique .rdb file for each shard.
Restore	| Yes, using a single .rdb file from a Redis (cluster mode disabled) cluster. |	Yes, using .rdb files from either a Redis (cluster mode disabled) or a Redis (cluster mode enabled) cluster.



# AWS_elasticache_cluster Redis Terraform module for the back

Redis Cluster Mode Enabled.

To create two shards with a primary and a single read replica each:

Edit instance_private.tf:
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
    engine = "redis"

    cluster_mode  {
        replicas_per_node_group = 1
        num_node_groups = 2
    }
}
```


The following arguments are used:
- replication_group_id – (Required) The replication group identifier. This parameter is stored as a lowercase string.
- replication_group_description – (Required) A user-created description for the replication group.
- node_type - (Required) The compute and memory capacity of the nodes in the node group.
- automatic_failover_enabled - (Optional) Specifies whether a read-only replica will be automatically promoted to read/write primary if the existing primary fails. If true, Multi-AZ is enabled for this replication group. If false, Multi-AZ is disabled for this replication group. Must be enabled for Redis (cluster mode enabled) replication groups. Defaults to false.

- availability_zones - (Optional) A list of EC2 availability zones in which the replication group's cache clusters will be created. The order of the availability zones in the list is not important.
- engine - (Optional) The name of the cache engine to be used for the clusters in this replication group. e.g. redis
- parameter_group_name - (Optional) The name of the parameter group to associate with this replication group. If this argument is omitted, the default cache parameter group for the specified engine is used.
- port – (Optional) The port number on which each of the cache nodes will accept connections. For Memcache the default is 11211, and for Redis the default port is 6379.
- subnet_group_name - (Optional) The name of the cache subnet group to be used for the replication group.
- security_group_names - (Optional) A list of cache security group names to associate with this replication group.
- security_group_ids - (Optional) One or more Amazon VPC security groups associated with this replication group. Use this parameter only when you are creating a replication group in an Amazon Virtual Private Cloud
- cluster_mode - (Optional) Create a native redis cluster. automatic_failover_enabled must be set to true. Cluster Mode documented below. Only 1 cluster_mode block is allowed.


When cluster_mode is set. We use those arguments:
- replicas_per_node_group - (Required) Specify the number of replica nodes in each node group. Valid values are 0 to 5. Changing this number will force a new resource.
- num_node_groups - (Required) Specify the number of node groups (shards) for this Redis replication group. Changing this number will trigger an online resizing operation before other settings modifications.
