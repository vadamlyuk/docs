# Monitoring the state of a cluster and hosts

Using diagnostic tools in the management console, you can track the status of a {{ mmg-name }} cluster and its individual hosts. These tools display diagnostic information in the form of charts.

Cluster metric values are collected and charts are displayed by [{{ monitoring-name }}](../../monitoring/concepts/index.md). To get started with {{ monitoring-name }} metrics, dashboards, or alerts, click **Open in Monitoring** in the top panel.

Chart update rate:

* Standard hosts and hosts with an increased RAM to vCPU ratio (`memory-optimized`): {{ graph-update }}.
* Hosts with a guaranteed vCPU share under 100% (`burstable`): {{ graph-update-burstable }}.

{% note info %}

The most appropriate multiple units (MB, GB, and more) are automatically used in charts.

{% endnote %}

## Monitoring cluster status {#cluster}

To view detailed information about the {{ mmg-name }} cluster status:

1. Go to the folder page and select **{{ mmg-name }}**.
1. Click on the name of the cluster and open the **Monitoring** tab.

The following charts open on the page:

* **Asserts total**: The number of [assertions](https://docs.mongodb.com/manual/reference/command/serverStatus/#mongodb-serverstatus-serverstatus.asserts) raised in the cluster.
* **Average operation time per host**: The average time of command execution by each host (in microseconds).
* **Average operations time on primary**: The average command execution time on primary replicas (in microseconds).
* **Average operations time on secondaries**: The average command execution time on secondary replicas (in microseconds).
* **CPU usage per host**: vCPU usage on each host (in thousandths).
* **CPU usage per host, top 5 hosts**: Top 5 hosts with the maximum load on the vCPU (%).
* **Configured oplog size per host**: The size of the oplog on each cluster host (in GB).
* **Connections per host**: The average number of connections to each host.
* **Data size on primary, top 5 collections**: The size of the five largest collections on the primary replica (in bytes).
* **Disk read per host, top 5 hosts**: Top 5 hosts with the maximum read load (bytes per second).
* **Disk space usage per host, top 5 hosts**: Top 5 hosts that take up the most disk space (two charts are displayed: in bytes and %).
* **Disk usage per host, top 5 hosts**: Top 5 hosts with the maximum load on the storage I/O subsystem (bytes per second).
* **Disk write per host, top 5 hosts**: Top 5 hosts with the maximum write load (kilobytes per second).
* **Documents affected on primary**: The average number of documents affected by queries on the primary replica.
* **Documents affected on secondaries**: The average number of documents affected by queries on all secondary replicas.
* **Documents affected per host**: The average number of documents affected by queries on each host.
* **Hosts available for read**: The number of hosts that accept read data queries.
* **Hosts available for write**: The number of hosts that accept write data queries.
* **Index size on primary, top 5 indexes**: The size of the five largest indexes on the primary replica (in bytes).
* **Memory usage per host**: The amount of RAM used by each host (in bytes).
* **Memory usage per host, top 5 hosts**: Top 5 hosts that use the most RAM (%).
* **Network data received per host, top 5 hosts**: Top 5 hosts with the highest read load on the network (kilobytes per second).
* **Network data sent per host, top 5 hosts**: Top 5 hosts with the highest write load on the network (kilobytes per second).
* **Network usage per host, top 5 hosts**: Top 5 hosts with the maximum total network load (kilobytes per second).
* **Open cursors total**: The number of open cursors in the cluster.
* **Page faults per host**: The number of errors that occur when working with memory pages on each host.
* **Queries on secondaries**: The average number of queries of each type on secondary replicas.
* **Queries on primary**: The average number of queries of each type on primary replicas.
* **Readers/writers active queue per host, top 5**: Top 5 hosts with the highest ratio of read to write queries enqueued.
* **Replicated queries**: The average number of replicated queries in the cluster.
* **Replication lag per host and write_concern wait**: Replication lag on each host and waiting time for [write concern](https://docs.mongodb.com/manual/reference/write-concern/) (in seconds).
* **Scan and order per host**: The average number of data scanning and sorting operations on each host.
* **Scanned / returned**: Shows the following ratios:
    * `scanned_docs / returned_docs`: Documents scanned to documents returned.
    * `scanned_keys / returned_docs`: Index keys scanned to documents returned.
* **TTL indexes total**: The total number of [TTL indexes](https://docs.mongodb.com/manual/core/index-ttl/).
* **Total operations count on cluster**: The total number of operations performed in the cluster.
* **Total operations time on cluster**: The total operation execution time in the cluster (in milliseconds).
* **WiredTiger cache pages evicted on primary**: The average number of memory pages evicted on the primary replica.
* **WiredTiger cache state on primary**: WiredTiger cache usage on the primary replica (in bytes).
* **WiredTiger checkpoint time on primary**: The average time it takes to create WiredTiger checkpoints on the primary replica (in milliseconds).
* **WiredTiger concurrent transactions on primary**: The average number of parallel transactions on the primary replica.
* **WiredTiger transactions state on primary**: The average number of transactions on each level on the primary replica.
* **Write conflicts per host**: The number of write conflicts on each host.

## Monitoring the state of hosts {#hosts}

To view detailed information about the status of individual {{ mmg-name }} hosts:

1. Go to the folder page and select **{{ mmg-name }}**.
1. Click the name of the desired cluster and select **Hosts** → **Monitoring**.
1. Select the host from the drop-down list. You'll see the host role (`PRIMARY` or `SECONDARY`) and type (`MONGOCFG`, `MONGOD`, `MONGOINFRA`, or `MONGOS`) next to the host name.

This page displays charts showing the load on an individual host in the cluster:

* **CPU**: The loading of processor cores. As the load goes up, the **Idle** value goes down.
* **Disk Bytes**: The speed of disk operations (bytes per second).
* **Disk IOPS**: The number of disk operations per second.
* **Memory**: The use of RAM in bytes. At high loads, the value of the **Free** parameter goes down while those of other parameters go up.
* **Network Bytes**: The speed of data exchange over the network (bytes per second).
* **Network Packets**: The number of packets exchanged over the network per second.
