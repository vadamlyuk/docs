---
editable: false
sourcePath: en/_api-ref/mdb/greenplum/api-ref/Cluster/get.md
---


# Method get
Returns the specified Greenplum® cluster.
 
To get the list of available Greenplum® clusters, make a [list](/docs/managed-greenplum/api-ref/Cluster/list) request.
 
## HTTP request {#https-request}
```
GET https://mdb.api.cloud.yandex.net/mdb/managed-greenplum/v1/clusters/{clusterId}
```
 
## Path parameters {#path_params}
 
Parameter | Description
--- | ---
clusterId | Required. ID of the Greenplum® Cluster resource to return. To get the cluster ID, use a [list](/docs/managed-greenplum/api-ref/Cluster/list) request.  The maximum string length in characters is 50.
 
## Response {#responses}
**HTTP Code: 200 - OK**

```json 
{
  "id": "string",
  "folderId": "string",
  "createdAt": "string",
  "name": "string",
  "config": {
    "version": "string",
    "backupWindowStart": {
      "hours": "integer",
      "minutes": "integer",
      "seconds": "integer",
      "nanos": "integer"
    },
    "access": {
      "dataLens": true,
      "webSql": true
    },
    "zoneId": "string",
    "subnetId": "string",
    "assignPublicIp": true
  },
  "description": "string",
  "labels": "object",
  "environment": "string",
  "monitoring": [
    {
      "name": "string",
      "description": "string",
      "link": "string"
    }
  ],
  "masterConfig": {
    "resources": {
      "resourcePresetId": "string",
      "diskSize": "string",
      "diskTypeId": "string"
    },
    "config": {
      "effectiveConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "timezone": "string",
        "pool": {
          "mode": "string",
          "size": "integer",
          "clientIdleTimeout": "integer"
        },
        "maxPreparedTransactions": "integer",
        "runawayDetectorActivationPercent": "integer",
        "tcpKeepalivesCount": "integer",
        "tcpKeepalivesInterval": "integer",
        "readableExternalTableTimeout": "integer",
        "gpInterconnectSndQueueDepth": "integer",
        "gpInterconnectQueueDepth": "integer",
        "logStatement": "string",
        "logDuration": true,
        "optimizerAnalyzeRootPartition": true,
        "gpExternalMaxSegs": "integer",
        "gpFtsProbeTimeout": "integer",
        "gpWorkfileCompression": true,
        "gpAutostatsModeInFunctions": "string"
      },
      "userConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "timezone": "string",
        "pool": {
          "mode": "string",
          "size": "integer",
          "clientIdleTimeout": "integer"
        },
        "maxPreparedTransactions": "integer",
        "runawayDetectorActivationPercent": "integer",
        "tcpKeepalivesCount": "integer",
        "tcpKeepalivesInterval": "integer",
        "readableExternalTableTimeout": "integer",
        "gpInterconnectSndQueueDepth": "integer",
        "gpInterconnectQueueDepth": "integer",
        "logStatement": "string",
        "logDuration": true,
        "optimizerAnalyzeRootPartition": true,
        "gpExternalMaxSegs": "integer",
        "gpFtsProbeTimeout": "integer",
        "gpWorkfileCompression": true,
        "gpAutostatsModeInFunctions": "string"
      },
      "defaultConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "timezone": "string",
        "pool": {
          "mode": "string",
          "size": "integer",
          "clientIdleTimeout": "integer"
        },
        "maxPreparedTransactions": "integer",
        "runawayDetectorActivationPercent": "integer",
        "tcpKeepalivesCount": "integer",
        "tcpKeepalivesInterval": "integer",
        "readableExternalTableTimeout": "integer",
        "gpInterconnectSndQueueDepth": "integer",
        "gpInterconnectQueueDepth": "integer",
        "logStatement": "string",
        "logDuration": true,
        "optimizerAnalyzeRootPartition": true,
        "gpExternalMaxSegs": "integer",
        "gpFtsProbeTimeout": "integer",
        "gpWorkfileCompression": true,
        "gpAutostatsModeInFunctions": "string"
      }
    }
  },
  "segmentConfig": {
    "resources": {
      "resourcePresetId": "string",
      "diskSize": "string",
      "diskTypeId": "string"
    },
    "config": {
      "effectiveConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "maxSlotWalKeepSize": "integer",
        "gpWorkfileLimitPerSegment": "integer",
        "gpWorkfileLimitPerQuery": "integer",
        "gpWorkfileLimitFilesPerQuery": "integer",
        "gpResourceManager": "string",
        "gpResourceGroupCpuLimit": "number",
        "gpResourceGroupMemoryLimit": "number"
      },
      "userConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "maxSlotWalKeepSize": "integer",
        "gpWorkfileLimitPerSegment": "integer",
        "gpWorkfileLimitPerQuery": "integer",
        "gpWorkfileLimitFilesPerQuery": "integer",
        "gpResourceManager": "string",
        "gpResourceGroupCpuLimit": "number",
        "gpResourceGroupMemoryLimit": "number"
      },
      "defaultConfig": {
        "logLevel": "string",
        "maxConnections": "integer",
        "maxSlotWalKeepSize": "integer",
        "gpWorkfileLimitPerSegment": "integer",
        "gpWorkfileLimitPerQuery": "integer",
        "gpWorkfileLimitFilesPerQuery": "integer",
        "gpResourceManager": "string",
        "gpResourceGroupCpuLimit": "number",
        "gpResourceGroupMemoryLimit": "number"
      }
    }
  },
  "masterHostCount": "string",
  "segmentHostCount": "string",
  "segmentInHost": "string",
  "networkId": "string",
  "health": "string",
  "status": "string",
  "maintenanceWindow": {

    // `maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`
    "anytime": {},
    "weeklyMaintenanceWindow": {
      "day": "string",
      "hour": "string"
    },
    // end of the list of possible fields`maintenanceWindow`

  },
  "plannedOperation": {
    "info": "string",
    "delayedUntil": "string"
  },
  "securityGroupIds": [
    "string"
  ],
  "userName": "string",
  "deletionProtection": true,
  "hostGroupIds": [
    "string"
  ]
}
```
A Greenplum® cluster resource.
 
Field | Description
--- | ---
id | **string**<br><p>ID of the Greenplum® cluster. This ID is assigned by Yandex.Cloud at the time of cluster creation.</p> 
folderId | **string**<br><p>ID of the folder that the Greenplum® cluster belongs to.</p> 
createdAt | **string** (date-time)<br><p>Cluster creation timestamp in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
name | **string**<br><p>Required. Name of the Greenplum® cluster. The name is unique within the folder and is 1-63 characters long.</p> <p>The maximum string length in characters is 63.</p> 
config | **object**<br><p>Greenplum® cluster configuration.</p> <p>Greenplum® cluster configuration.</p> 
config.<br>version | **string**<br><p>Version of the Greenplum® server software.</p> 
config.<br>backupWindowStart | **object**<br><p>Time to start the daily backup, in the UTC timezone.</p> <p>Represents a time of day. The date and time zone are either not significant or are specified elsewhere. An API may choose to allow leap seconds. Related types are <a href="https://github.com/googleapis/googleapis/blob/master/google/type/date.proto">google.type.Date</a> and <a href="https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf/timestamp.proto">google.protobuf.Timestamp</a>.</p> 
config.<br>backupWindowStart.<br>hours | **integer** (int32)<br><p>Hours of day in 24 hour format. Should be from 0 to 23. An API may choose to allow the value &quot;24:00:00&quot; for scenarios like business closing time.</p> 
config.<br>backupWindowStart.<br>minutes | **integer** (int32)<br><p>Minutes of hour of day. Must be from 0 to 59.</p> 
config.<br>backupWindowStart.<br>seconds | **integer** (int32)<br><p>Seconds of minutes of the time. Must normally be from 0 to 59. An API may allow the value 60 if it allows leap-seconds.</p> 
config.<br>backupWindowStart.<br>nanos | **integer** (int32)<br><p>Fractions of seconds in nanoseconds. Must be from 0 to 999,999,999.</p> 
config.<br>access | **object**<br><p>Access policy for external services.</p> <p>Greenplum® cluster access options.</p> 
config.<br>access.<br>dataLens | **boolean** (boolean)<br><p>Allows data export from the cluster to Yandex DataLens.</p> 
config.<br>access.<br>webSql | **boolean** (boolean)<br><p>Allows SQL queries to the cluster databases from the Yandex.Cloud management console.</p> 
config.<br>zoneId | **string**<br><p>ID of the availability zone the cluster belongs to. To get a list of available zones, use the <a href="/docs/compute/api-ref/Zone/list">list</a> request.</p> <p>The maximum string length in characters is 50.</p> 
config.<br>subnetId | **string**<br><p>ID of the subnet the cluster belongs to. This subnet should be a part of the cloud network the cluster belongs to (see <a href="/docs/managed-greenplum/api-ref/Cluster#representation">Cluster.networkId</a>).</p> <p>The maximum string length in characters is 50.</p> 
config.<br>assignPublicIp | **boolean** (boolean)<br><p>Whether or not the cluster has a public IP address.</p> <p>After the cluster has been created, this setting cannot be changed.</p> 
description | **string**<br><p>Description of the Greenplum® cluster. 0-256 characters long.</p> 
labels | **object**<br><p>Custom labels for the Greenplum® cluster as ``key:value`` pairs. Maximum 64 labels per resource.</p> 
environment | **string**<br><p>Deployment environment of the Greenplum® cluster.</p> <p>Deployment environment.</p> <ul> <li>PRODUCTION: Stable environment with a conservative update policy: only hotfixes are applied during regular maintenance.</li> <li>PRESTABLE: Environment with more aggressive update policy: new versions are rolled out irrespective of backward compatibility.</li> </ul> 
monitoring[] | **object**<br><p>Monitoring system metadata.</p> 
monitoring[].<br>name | **string**<br><p>Name of the monitoring system.</p> 
monitoring[].<br>description | **string**<br><p>Description of the monitoring system.</p> 
monitoring[].<br>link | **string**<br><p>Link to the monitoring system charts for the Greenplum® cluster.</p> 
masterConfig | **object**<br><p>Configuration of the Greenplum® master subcluster.</p> <p>Configuration of the master subcluster.</p> 
masterConfig.<br>resources | **object**<br><p>Computational resources allocated to Greenplum® master subcluster hosts.</p> <p>A list of computational resources allocated to a host.</p> 
masterConfig.<br>resources.<br>resourcePresetId | **string**<br><p>ID of the preset for computational resources allocated to a host. Available presets are listed in the <a href="/docs/managed-greenplum/concepts/instance-types">documentation</a>.</p> 
masterConfig.<br>resources.<br>diskSize | **string** (int64)<br><p>Volume of the storage used by the host, in bytes.</p> 
masterConfig.<br>resources.<br>diskTypeId | **string**<br><p>Type of the storage used by the host: ``network-hdd``, ``network-ssd`` or ``local-ssd``.</p> 
masterConfig.<br>config | **object**<br><p>Configuration settings of a Greenplum® master server.</p> <p>Set of Greenplum® master subcluster configurations.</p> 
masterConfig.<br>config.<br>effectiveConfig | **object**<br><p>Required. Effective settings for a Greenplum® master subcluster (a combination of settings defined in ``userConfig`` and ``defaultConfig``).</p> <p>Greenplum® master subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>logLevel | **string**<br><p>Controls which message levels in the master subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
masterConfig.<br>config.<br>effectiveConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>timezone | **string**<br><p>The server time zone to be used in DateTime field conversions, specified as an IANA identifier.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>pool | **object**<br><p>Route server configuration.</p> <p>Route server configuration.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>pool.<br>mode | **string**<br><p>Route server pool mode.</p> <p>Route server pool mode.</p> <ul> <li>SESSION: Assign server connection to a client until it disconnects. Default value.</li> <li>TRANSACTION: Assign server connection to a client for a transaction processing.</li> </ul> 
masterConfig.<br>config.<br>effectiveConfig.<br>pool.<br>size | **integer** (int64)<br><p>The number of servers in the server pool. Clients are placed in a wait queue when all servers are busy. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>pool.<br>clientIdleTimeout | **integer** (int64)<br><p>Server pool idle timeout, in seconds. A server connection closes after it has been idle for the specified duration. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>maxPreparedTransactions | **integer** (int64)<br><p>Sets the maximum number of transactions that can be in the &quot;prepared&quot; state simultaneously. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/9.6/runtime-config-resource.html">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>runawayDetectorActivationPercent | **integer** (int64)<br><p>For queries that are managed by resource queues or resource groups, this parameter determines when Greenplum® Database terminates running queries based on the amount of memory the queries are using. A value of 100 disables the automatic termination of queries based on the percentage of memory that is utilized. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#runaway_detector_activation_percent">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>tcpKeepalivesCount | **integer** (int64)<br><p>How many keepalives may be lost before the connection is considered dead. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_count">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>tcpKeepalivesInterval | **integer** (int64)<br><p>How many seconds to wait for a response to a keepalive before retransmitting. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_interval">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>readableExternalTableTimeout | **integer** (int64)<br><p>When an SQL query reads from an external table, the parameter value specifies the amount of time in seconds that Greenplum® Database waits before cancelling the query when data stops being returned from the external table. The default value of 0, specifies no time out. Greenplum® Database does not cancel the query. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#readable_external_table_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpInterconnectSndQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the default UDPIFC interconnect on senders. Increasing the depth from its default value will cause the system to use more memory, but may increase performance. Reasonable values for this parameter are between 1 and 4. Increasing the value might radically increase the amount of memory used by the system. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_snd_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpInterconnectQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the Greenplum® Database interconnect on receivers (when data is received but no space is available to receive it the data will be dropped, and the transmitter will need to resend it) for the default UDPIFC interconnect. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>logStatement | **string**<br><p>Controls which SQL statements are logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_statement">documentation</a>.</p> <p>Controls which SQL statements are logged.</p> <ul> <li>LOG_STATEMENT_UNSPECIFIED: The statement log option is not specified.</li> <li>NONE: Don't log SQL statements.</li> <li>DDL: Log all data definition commands like ``CREATE``, ``ALTER``, and ``DROP`` commands. Default value.</li> <li>MOD: Log ``INSERT``, ``UPDATE``, ``DELETE``, ``TRUNCATE``, and ``COPY FROM`` commands. ``PREPARE`` and ``EXPLAIN ANALYZE`` statements are also logged if their contained command is of an appropriate type.</li> <li>ALL: Log all SQL statements.</li> </ul> 
masterConfig.<br>config.<br>effectiveConfig.<br>logDuration | **boolean** (boolean)<br><p>Causes the duration of every completed statement which satisfies ``logStatement`` to be logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_duration">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>optimizerAnalyzeRootPartition | **boolean** (boolean)<br><p>For a partitioned table, controls whether the ``ROOTPARTITION`` keyword is required to collect root partition statistics when the ``ANALYZE`` command is run on the table. GPORCA uses the root partition statistics when generating a query plan. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#optimizer_analyze_root_partition">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpExternalMaxSegs | **integer** (int64)<br><p>Sets the number of segments that will scan external table data during an external table operation, the purpose being not to overload the system with scanning data and take away resources from other concurrent operations. This only applies to external tables that use the ``gpfdist`` protocol to access external table data. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_external_max_segs">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpFtsProbeTimeout | **integer** (int64)<br><p>Specifies the allowed timeout for the fault detection process (ftsprobe) to establish a connection to a segment before declaring it down. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_fts_probe_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpWorkfileCompression | **boolean** (boolean)<br><p>Specifies whether the temporary files created, when a hash aggregation or hash join operation spills to disk, are compressed. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_compression">documentation</a>.</p> 
masterConfig.<br>config.<br>effectiveConfig.<br>gpAutostatsModeInFunctions | **string**<br><p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_autostats_mode_in_functions">documentation</a>.</p> <p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions.</p> <ul> <li>AUTOSTATS_MODE_IN_FUNCTIONS_UNSPECIFIED: The mode is not specified.</li> <li>MODE_NONE: Disables statistics collection. Default value.</li> <li>ON_CHANGE: Triggers statistics collection only when the number of rows affected exceeds the threshold defined by ``gp_autostats_on_change_threshold`` (default is 2147483647). Operations in functions that can trigger automatic statistics collection with on_change are: ``CREATE TABLE AS SELECT``, ``UPDATE``, ``DELETE``, ``INSERT`` and ``COPY``.</li> <li>ON_NO_STATS: Triggers statistics collection for ``CREATE TABLE AS SELECT``, ``INSERT``, or ``COPY`` operations that are executed in functions on any table that has no existing statistics.</li> </ul> 
masterConfig.<br>config.<br>userConfig | **object**<br><p>User-defined settings for a Greenplum® master subcluster.</p> <p>Greenplum® master subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
masterConfig.<br>config.<br>userConfig.<br>logLevel | **string**<br><p>Controls which message levels in the master subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
masterConfig.<br>config.<br>userConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
masterConfig.<br>config.<br>userConfig.<br>timezone | **string**<br><p>The server time zone to be used in DateTime field conversions, specified as an IANA identifier.</p> 
masterConfig.<br>config.<br>userConfig.<br>pool | **object**<br><p>Route server configuration.</p> <p>Route server configuration.</p> 
masterConfig.<br>config.<br>userConfig.<br>pool.<br>mode | **string**<br><p>Route server pool mode.</p> <p>Route server pool mode.</p> <ul> <li>SESSION: Assign server connection to a client until it disconnects. Default value.</li> <li>TRANSACTION: Assign server connection to a client for a transaction processing.</li> </ul> 
masterConfig.<br>config.<br>userConfig.<br>pool.<br>size | **integer** (int64)<br><p>The number of servers in the server pool. Clients are placed in a wait queue when all servers are busy. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>userConfig.<br>pool.<br>clientIdleTimeout | **integer** (int64)<br><p>Server pool idle timeout, in seconds. A server connection closes after it has been idle for the specified duration. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>userConfig.<br>maxPreparedTransactions | **integer** (int64)<br><p>Sets the maximum number of transactions that can be in the &quot;prepared&quot; state simultaneously. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/9.6/runtime-config-resource.html">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>runawayDetectorActivationPercent | **integer** (int64)<br><p>For queries that are managed by resource queues or resource groups, this parameter determines when Greenplum® Database terminates running queries based on the amount of memory the queries are using. A value of 100 disables the automatic termination of queries based on the percentage of memory that is utilized. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#runaway_detector_activation_percent">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>tcpKeepalivesCount | **integer** (int64)<br><p>How many keepalives may be lost before the connection is considered dead. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_count">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>tcpKeepalivesInterval | **integer** (int64)<br><p>How many seconds to wait for a response to a keepalive before retransmitting. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_interval">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>readableExternalTableTimeout | **integer** (int64)<br><p>When an SQL query reads from an external table, the parameter value specifies the amount of time in seconds that Greenplum® Database waits before cancelling the query when data stops being returned from the external table. The default value of 0, specifies no time out. Greenplum® Database does not cancel the query. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#readable_external_table_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpInterconnectSndQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the default UDPIFC interconnect on senders. Increasing the depth from its default value will cause the system to use more memory, but may increase performance. Reasonable values for this parameter are between 1 and 4. Increasing the value might radically increase the amount of memory used by the system. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_snd_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpInterconnectQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the Greenplum® Database interconnect on receivers (when data is received but no space is available to receive it the data will be dropped, and the transmitter will need to resend it) for the default UDPIFC interconnect. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>logStatement | **string**<br><p>Controls which SQL statements are logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_statement">documentation</a>.</p> <p>Controls which SQL statements are logged.</p> <ul> <li>LOG_STATEMENT_UNSPECIFIED: The statement log option is not specified.</li> <li>NONE: Don't log SQL statements.</li> <li>DDL: Log all data definition commands like ``CREATE``, ``ALTER``, and ``DROP`` commands. Default value.</li> <li>MOD: Log ``INSERT``, ``UPDATE``, ``DELETE``, ``TRUNCATE``, and ``COPY FROM`` commands. ``PREPARE`` and ``EXPLAIN ANALYZE`` statements are also logged if their contained command is of an appropriate type.</li> <li>ALL: Log all SQL statements.</li> </ul> 
masterConfig.<br>config.<br>userConfig.<br>logDuration | **boolean** (boolean)<br><p>Causes the duration of every completed statement which satisfies ``logStatement`` to be logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_duration">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>optimizerAnalyzeRootPartition | **boolean** (boolean)<br><p>For a partitioned table, controls whether the ``ROOTPARTITION`` keyword is required to collect root partition statistics when the ``ANALYZE`` command is run on the table. GPORCA uses the root partition statistics when generating a query plan. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#optimizer_analyze_root_partition">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpExternalMaxSegs | **integer** (int64)<br><p>Sets the number of segments that will scan external table data during an external table operation, the purpose being not to overload the system with scanning data and take away resources from other concurrent operations. This only applies to external tables that use the ``gpfdist`` protocol to access external table data. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_external_max_segs">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpFtsProbeTimeout | **integer** (int64)<br><p>Specifies the allowed timeout for the fault detection process (ftsprobe) to establish a connection to a segment before declaring it down. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_fts_probe_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpWorkfileCompression | **boolean** (boolean)<br><p>Specifies whether the temporary files created, when a hash aggregation or hash join operation spills to disk, are compressed. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_compression">documentation</a>.</p> 
masterConfig.<br>config.<br>userConfig.<br>gpAutostatsModeInFunctions | **string**<br><p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_autostats_mode_in_functions">documentation</a>.</p> <p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions.</p> <ul> <li>AUTOSTATS_MODE_IN_FUNCTIONS_UNSPECIFIED: The mode is not specified.</li> <li>MODE_NONE: Disables statistics collection. Default value.</li> <li>ON_CHANGE: Triggers statistics collection only when the number of rows affected exceeds the threshold defined by ``gp_autostats_on_change_threshold`` (default is 2147483647). Operations in functions that can trigger automatic statistics collection with on_change are: ``CREATE TABLE AS SELECT``, ``UPDATE``, ``DELETE``, ``INSERT`` and ``COPY``.</li> <li>ON_NO_STATS: Triggers statistics collection for ``CREATE TABLE AS SELECT``, ``INSERT``, or ``COPY`` operations that are executed in functions on any table that has no existing statistics.</li> </ul> 
masterConfig.<br>config.<br>defaultConfig | **object**<br><p>Default configuration for a Greenplum® master subcluster.</p> <p>Greenplum® master subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>logLevel | **string**<br><p>Controls which message levels in the master subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
masterConfig.<br>config.<br>defaultConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>timezone | **string**<br><p>The server time zone to be used in DateTime field conversions, specified as an IANA identifier.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>pool | **object**<br><p>Route server configuration.</p> <p>Route server configuration.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>pool.<br>mode | **string**<br><p>Route server pool mode.</p> <p>Route server pool mode.</p> <ul> <li>SESSION: Assign server connection to a client until it disconnects. Default value.</li> <li>TRANSACTION: Assign server connection to a client for a transaction processing.</li> </ul> 
masterConfig.<br>config.<br>defaultConfig.<br>pool.<br>size | **integer** (int64)<br><p>The number of servers in the server pool. Clients are placed in a wait queue when all servers are busy. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>pool.<br>clientIdleTimeout | **integer** (int64)<br><p>Server pool idle timeout, in seconds. A server connection closes after it has been idle for the specified duration. Set to zero to disable the limit.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>maxPreparedTransactions | **integer** (int64)<br><p>Sets the maximum number of transactions that can be in the &quot;prepared&quot; state simultaneously. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/9.6/runtime-config-resource.html">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>runawayDetectorActivationPercent | **integer** (int64)<br><p>For queries that are managed by resource queues or resource groups, this parameter determines when Greenplum® Database terminates running queries based on the amount of memory the queries are using. A value of 100 disables the automatic termination of queries based on the percentage of memory that is utilized. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#runaway_detector_activation_percent">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>tcpKeepalivesCount | **integer** (int64)<br><p>How many keepalives may be lost before the connection is considered dead. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_count">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>tcpKeepalivesInterval | **integer** (int64)<br><p>How many seconds to wait for a response to a keepalive before retransmitting. A value of 0 uses the system default. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#tcp_keepalives_interval">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>readableExternalTableTimeout | **integer** (int64)<br><p>When an SQL query reads from an external table, the parameter value specifies the amount of time in seconds that Greenplum® Database waits before cancelling the query when data stops being returned from the external table. The default value of 0, specifies no time out. Greenplum® Database does not cancel the query. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#readable_external_table_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpInterconnectSndQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the default UDPIFC interconnect on senders. Increasing the depth from its default value will cause the system to use more memory, but may increase performance. Reasonable values for this parameter are between 1 and 4. Increasing the value might radically increase the amount of memory used by the system. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_snd_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpInterconnectQueueDepth | **integer** (int64)<br><p>Sets the amount of data per-peer to be queued by the Greenplum® Database interconnect on receivers (when data is received but no space is available to receive it the data will be dropped, and the transmitter will need to resend it) for the default UDPIFC interconnect. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_interconnect_queue_depth">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>logStatement | **string**<br><p>Controls which SQL statements are logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_statement">documentation</a>.</p> <p>Controls which SQL statements are logged.</p> <ul> <li>LOG_STATEMENT_UNSPECIFIED: The statement log option is not specified.</li> <li>NONE: Don't log SQL statements.</li> <li>DDL: Log all data definition commands like ``CREATE``, ``ALTER``, and ``DROP`` commands. Default value.</li> <li>MOD: Log ``INSERT``, ``UPDATE``, ``DELETE``, ``TRUNCATE``, and ``COPY FROM`` commands. ``PREPARE`` and ``EXPLAIN ANALYZE`` statements are also logged if their contained command is of an appropriate type.</li> <li>ALL: Log all SQL statements.</li> </ul> 
masterConfig.<br>config.<br>defaultConfig.<br>logDuration | **boolean** (boolean)<br><p>Causes the duration of every completed statement which satisfies ``logStatement`` to be logged. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#log_duration">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>optimizerAnalyzeRootPartition | **boolean** (boolean)<br><p>For a partitioned table, controls whether the ``ROOTPARTITION`` keyword is required to collect root partition statistics when the ``ANALYZE`` command is run on the table. GPORCA uses the root partition statistics when generating a query plan. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#optimizer_analyze_root_partition">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpExternalMaxSegs | **integer** (int64)<br><p>Sets the number of segments that will scan external table data during an external table operation, the purpose being not to overload the system with scanning data and take away resources from other concurrent operations. This only applies to external tables that use the ``gpfdist`` protocol to access external table data. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_external_max_segs">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpFtsProbeTimeout | **integer** (int64)<br><p>Specifies the allowed timeout for the fault detection process (ftsprobe) to establish a connection to a segment before declaring it down. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_fts_probe_timeout">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpWorkfileCompression | **boolean** (boolean)<br><p>Specifies whether the temporary files created, when a hash aggregation or hash join operation spills to disk, are compressed. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_compression">documentation</a>.</p> 
masterConfig.<br>config.<br>defaultConfig.<br>gpAutostatsModeInFunctions | **string**<br><p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_autostats_mode_in_functions">documentation</a>.</p> <p>Specifies the mode for triggering automatic statistics collection with ``ANALYZE`` for statements in procedural language functions.</p> <ul> <li>AUTOSTATS_MODE_IN_FUNCTIONS_UNSPECIFIED: The mode is not specified.</li> <li>MODE_NONE: Disables statistics collection. Default value.</li> <li>ON_CHANGE: Triggers statistics collection only when the number of rows affected exceeds the threshold defined by ``gp_autostats_on_change_threshold`` (default is 2147483647). Operations in functions that can trigger automatic statistics collection with on_change are: ``CREATE TABLE AS SELECT``, ``UPDATE``, ``DELETE``, ``INSERT`` and ``COPY``.</li> <li>ON_NO_STATS: Triggers statistics collection for ``CREATE TABLE AS SELECT``, ``INSERT``, or ``COPY`` operations that are executed in functions on any table that has no existing statistics.</li> </ul> 
segmentConfig | **object**<br><p>Configuration of the Greenplum® segment subcluster.</p> <p>Configuration of the segment subcluster.</p> 
segmentConfig.<br>resources | **object**<br><p>Computational resources allocated to Greenplum® segment subcluster hosts.</p> <p>A list of computational resources allocated to a host.</p> 
segmentConfig.<br>resources.<br>resourcePresetId | **string**<br><p>ID of the preset for computational resources allocated to a host. Available presets are listed in the <a href="/docs/managed-greenplum/concepts/instance-types">documentation</a>.</p> 
segmentConfig.<br>resources.<br>diskSize | **string** (int64)<br><p>Volume of the storage used by the host, in bytes.</p> 
segmentConfig.<br>resources.<br>diskTypeId | **string**<br><p>Type of the storage used by the host: ``network-hdd``, ``network-ssd`` or ``local-ssd``.</p> 
segmentConfig.<br>config | **object**<br><p>Configuration settings of a Greenplum® segment server.</p> <p>Set of Greenplum® segment subcluster configurations.</p> 
segmentConfig.<br>config.<br>effectiveConfig | **object**<br><p>Required. Effective settings for a Greenplum® segment subcluster (a combination of settings defined in ``userConfig`` and ``defaultConfig``).</p> <p>Greenplum® segment subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>logLevel | **string**<br><p>Controls which message levels in the segment subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
segmentConfig.<br>config.<br>effectiveConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>maxSlotWalKeepSize | **integer** (int64)<br><p>Specifies the maximum size of WAL files that replication slots are allowed to retain in the ``pg_wal`` directory at checkpoint time. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/current/runtime-config-replication.html">documentation</a>.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpWorkfileLimitPerSegment | **integer** (int64)<br><p>Sets the maximum total disk size that all running queries are allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_segment">documentation</a>.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpWorkfileLimitPerQuery | **integer** (int64)<br><p>Sets the maximum disk size an individual query is allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpWorkfileLimitFilesPerQuery | **integer** (int64)<br><p>Sets the maximum number of temporary spill files (also known as workfiles) allowed per query per segment. Spill files are created when executing a query that requires more memory than it is allocated. The current query is terminated when the limit is exceeded. Set the value to 0 (zero) to allow an unlimited number of spill files. Default value is ``10000``. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_files_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpResourceManager | **string**<br><p>Identifies the resource management scheme currently enabled in the Greenplum® Database cluster. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_manager">documentation</a>.</p> <p>Resource management scheme currently enabled in the Greenplum® Database cluster.</p> <ul> <li>GP_RESOURCE_MANAGER_UNSPECIFIED: The management scheme is not specified.</li> <li>QUEUE: Resource queue-based management.</li> <li>GROUP: Resource group-based management. Default value.</li> </ul> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpResourceGroupCpuLimit | **number** (float)<br><p>Identifies the maximum percentage of system CPU resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_cpu_limit">documentation</a>.</p> 
segmentConfig.<br>config.<br>effectiveConfig.<br>gpResourceGroupMemoryLimit | **number** (float)<br><p>Identifies the maximum percentage of system memory resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_memory_limit">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig | **object**<br><p>User-defined settings for a Greenplum® segment subcluster.</p> <p>Greenplum® segment subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
segmentConfig.<br>config.<br>userConfig.<br>logLevel | **string**<br><p>Controls which message levels in the segment subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
segmentConfig.<br>config.<br>userConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
segmentConfig.<br>config.<br>userConfig.<br>maxSlotWalKeepSize | **integer** (int64)<br><p>Specifies the maximum size of WAL files that replication slots are allowed to retain in the ``pg_wal`` directory at checkpoint time. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/current/runtime-config-replication.html">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig.<br>gpWorkfileLimitPerSegment | **integer** (int64)<br><p>Sets the maximum total disk size that all running queries are allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_segment">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig.<br>gpWorkfileLimitPerQuery | **integer** (int64)<br><p>Sets the maximum disk size an individual query is allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig.<br>gpWorkfileLimitFilesPerQuery | **integer** (int64)<br><p>Sets the maximum number of temporary spill files (also known as workfiles) allowed per query per segment. Spill files are created when executing a query that requires more memory than it is allocated. The current query is terminated when the limit is exceeded. Set the value to 0 (zero) to allow an unlimited number of spill files. Default value is ``10000``. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_files_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig.<br>gpResourceManager | **string**<br><p>Identifies the resource management scheme currently enabled in the Greenplum® Database cluster. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_manager">documentation</a>.</p> <p>Resource management scheme currently enabled in the Greenplum® Database cluster.</p> <ul> <li>GP_RESOURCE_MANAGER_UNSPECIFIED: The management scheme is not specified.</li> <li>QUEUE: Resource queue-based management.</li> <li>GROUP: Resource group-based management. Default value.</li> </ul> 
segmentConfig.<br>config.<br>userConfig.<br>gpResourceGroupCpuLimit | **number** (float)<br><p>Identifies the maximum percentage of system CPU resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_cpu_limit">documentation</a>.</p> 
segmentConfig.<br>config.<br>userConfig.<br>gpResourceGroupMemoryLimit | **number** (float)<br><p>Identifies the maximum percentage of system memory resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_memory_limit">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig | **object**<br><p>Default configuration for a Greenplum® segment subcluster.</p> <p>Greenplum® segment subcluster configuration options.</p> <p>Options not listed here are not supported.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>logLevel | **string**<br><p>Controls which message levels in the segment subcluster are sent to the client.</p> <p>Controls which message levels are sent to the client. Each level includes all the levels that follow it. The later the level, the fewer messages are sent.</p> <ul> <li>LOG_LEVEL_UNSPECIFIED: Level of detail is not specified.</li> <li>TRACE: Include tracing information.</li> <li>DEBUG: Include debugging information.</li> <li>INFORMATION: Include informative messages.</li> <li>WARNING: Include warning messages. Default value.</li> <li>ERROR: Only include error messages.</li> </ul> 
segmentConfig.<br>config.<br>defaultConfig.<br>maxConnections | **integer** (int64)<br><p>Maximum number of inbound connections.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>maxSlotWalKeepSize | **integer** (int64)<br><p>Specifies the maximum size of WAL files that replication slots are allowed to retain in the ``pg_wal`` directory at checkpoint time. For details, see PostgreSQL <a href="https://www.postgresql.org/docs/current/runtime-config-replication.html">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpWorkfileLimitPerSegment | **integer** (int64)<br><p>Sets the maximum total disk size that all running queries are allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_segment">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpWorkfileLimitPerQuery | **integer** (int64)<br><p>Sets the maximum disk size an individual query is allowed to use for creating temporary spill files at each segment. The default value is ``0``, which means a limit is not enforced. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpWorkfileLimitFilesPerQuery | **integer** (int64)<br><p>Sets the maximum number of temporary spill files (also known as workfiles) allowed per query per segment. Spill files are created when executing a query that requires more memory than it is allocated. The current query is terminated when the limit is exceeded. Set the value to 0 (zero) to allow an unlimited number of spill files. Default value is ``10000``. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_workfile_limit_files_per_query">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpResourceManager | **string**<br><p>Identifies the resource management scheme currently enabled in the Greenplum® Database cluster. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_manager">documentation</a>.</p> <p>Resource management scheme currently enabled in the Greenplum® Database cluster.</p> <ul> <li>GP_RESOURCE_MANAGER_UNSPECIFIED: The management scheme is not specified.</li> <li>QUEUE: Resource queue-based management.</li> <li>GROUP: Resource group-based management. Default value.</li> </ul> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpResourceGroupCpuLimit | **number** (float)<br><p>Identifies the maximum percentage of system CPU resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_cpu_limit">documentation</a>.</p> 
segmentConfig.<br>config.<br>defaultConfig.<br>gpResourceGroupMemoryLimit | **number** (float)<br><p>Identifies the maximum percentage of system memory resources to allocate to resource groups on each Greenplum® Database segment node. Note: This server configuration parameter is enforced only when resource group-based management is active. For details, see Greenplum® <a href="https://docs.greenplum.org/6-5/ref_guide/config_params/guc-list.html#gp_resource_group_memory_limit">documentation</a>.</p> 
masterHostCount | **string** (int64)<br><p>Number of hosts in the master subcluster.</p> 
segmentHostCount | **string** (int64)<br><p>Number of hosts in the segment subcluster.</p> 
segmentInHost | **string** (int64)<br><p>Number of segments per host.</p> 
networkId | **string**<br><p>ID of the cloud network that the cluster belongs to.</p> 
health | **string**<br><p>Aggregated cluster health.</p> <p>Aggregated health of the cluster.</p> <ul> <li>HEALTH_UNKNOWN: Health of the cluster is unknown (``health`` for every host in the cluster is UNKNOWN).</li> <li>ALIVE: Cluster is working normally (``health`` for every host in the cluster is ALIVE).</li> <li>DEAD: Cluster is inoperable (``health`` for every host in the cluster is DEAD).</li> <li>DEGRADED: Cluster is working below capacity (``health`` for at least one host in the cluster is not ALIVE).</li> <li>UNBALANCED: Cluster is working below capacity (``health`` for at least one host in the cluster is UNBALANCED).</li> </ul> 
status | **string**<br><p>Current state of the cluster.</p> <p>Current state of the cluster.</p> <ul> <li>STATUS_UNKNOWN: Cluster state is unknown.</li> <li>CREATING: Cluster is being created.</li> <li>RUNNING: Cluster is running normally.</li> <li>ERROR: Cluster has encountered a problem and cannot operate.</li> <li>UPDATING: Cluster is being updated.</li> <li>STOPPING: Cluster is stopping.</li> <li>STOPPED: Cluster has stopped.</li> <li>STARTING: Cluster is starting.</li> </ul> 
maintenanceWindow | **object**<br><p>Window of maintenance operations.</p> <p>A Greenplum® cluster maintenance window. Should be defined by either one of the two options.</p> 
maintenanceWindow.<br>anytime | **object**<br>An any-time maintenance window. <br>`maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`<br><br><p>An any-time maintenance window.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow | **object**<br>A weekly maintenance window. <br>`maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`<br><br><p>A weekly maintenance window.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow.<br>day | **string**<br><p>Day of the week.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow.<br>hour | **string** (int64)<br><p>Hour of the day in the UTC timezone.</p> <p>Acceptable values are 1 to 24, inclusive.</p> 
plannedOperation | **object**<br><p>Maintenance operation planned at nearest <a href="/docs/managed-greenplum/api-ref/Cluster#representation">maintenanceWindow</a>.</p> <p>The operation to perform during maintenance.</p> 
plannedOperation.<br>info | **string**<br><p>The description of the operation, 1-256 characters long.</p> <p>The maximum string length in characters is 256.</p> 
plannedOperation.<br>delayedUntil | **string** (date-time)<br><p>Delay time for the maintenance operation.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
securityGroupIds[] | **string**<br><p>User security groups.</p> 
userName | **string**<br><p>Owner user name.</p> 
deletionProtection | **boolean** (boolean)<br><p>Whether or not cluster is protected from being deleted.</p> 
hostGroupIds[] | **string**<br><p>Host groups hosting VMs of the cluster.</p> 