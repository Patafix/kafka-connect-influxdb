
# Introduction

# Source Connectors


## InfluxDBSourceConnector

The InfluxDBSourceConnector is used to emulate an InfluxDB host and receive messages written by an InfluxDB client.



### Important

This connector listens on a network port. Running more than one task or running in distributed mode can cause some undesired effects if another task already has the port open. It is recommended that you run this connector in :term:`Standalone Mode`.



### Configuration

##### `http.enable`
*Importance:* High

*Type:* Boolean

*Default Value:* true


Flag to determine if http should be enabled.
##### `http.port`
*Importance:* High

*Type:* Int

*Default Value:* 8080

*Validator:* ValidPort{start=1000, end=65535}


Port the http listener should be started on.
##### `https.enable`
*Importance:* High

*Type:* Boolean

*Default Value:* false


Flag to determine if https should be enabled.
##### `https.port`
*Importance:* High

*Type:* Int

*Default Value:* 8443

*Validator:* ValidPort{start=1000, end=65535}


Port the https listener should be started on.
##### `topic.prefix`
*Importance:* High

*Type:* String

*Default Value:* influxdb.



##### `allowed.databases`
*Importance:* Medium

*Type:* List

*Default Value:* []



##### `health.check.enable`
*Importance:* Medium

*Type:* Boolean

*Default Value:* true


Flag to determine if a health check url for a load balancer should be configured.
##### `health.check.path`
*Importance:* Medium

*Type:* String

*Default Value:* /healthcheck


Path that will respond with a health check.
##### `https.key.manager.password`
*Importance:* Medium

*Type:* Password

*Default Value:* [hidden]


The key manager password.
##### `https.key.store.password`
*Importance:* Medium

*Type:* Password

*Default Value:* [hidden]


The password for the ssl keystore.
##### `https.key.store.path`
*Importance:* Medium

*Type:* String


Path on the local filesystem that contains the ssl keystore.
##### `https.trust.store.password`
*Importance:* Medium

*Type:* Password

*Default Value:* [hidden]


The password for the ssl trust store.
##### `https.trust.store.path`
*Importance:* Medium

*Type:* String


The key manager password.
##### `thread.pool.max.size`
*Importance:* Medium

*Type:* Int

*Default Value:* 100

*Validator:* [10,...,1000]


The maximum number of threads for the thread pool to allocate.
##### `thread.pool.min.size`
*Importance:* Medium

*Type:* Int

*Default Value:* 10

*Validator:* [10,...,1000]


The minimum number of threads for the thread pool to allocate.
##### `http.idle.timeout.ms`
*Importance:* Low

*Type:* Int

*Default Value:* 30000

*Validator:* [5000,...,300000]


The number of milliseconds idle before a connection has timed out.
##### `https.idle.timeout.ms`
*Importance:* Low

*Type:* Int

*Default Value:* 30000

*Validator:* [5000,...,300000]


The number of milliseconds idle before a connection has timed out.

#### Examples

##### Standalone Example

This configuration is used typically along with [standalone mode](http://docs.confluent.io/current/connect/concepts.html#standalone-workers).

```properties
name=InfluxDBSourceConnector1
connector.class=com.github.jcustenborder.kafka.connect.influxdb.InfluxDBSourceConnector
tasks.max=1
```

##### Distributed Example

This configuration is used typically along with [distributed mode](http://docs.confluent.io/current/connect/concepts.html#distributed-workers).
Write the following json to `connector.json`, configure all of the required values, and use the command below to
post the configuration to one the distributed connect worker(s).

```json
{
  "config" : {
    "name" : "InfluxDBSourceConnector1",
    "connector.class" : "com.github.jcustenborder.kafka.connect.influxdb.InfluxDBSourceConnector",
    "tasks.max" : "1"
  }
}
```

Use curl to post the configuration to one of the Kafka Connect Workers. Change `http://localhost:8083/` the the endpoint of
one of your Kafka Connect worker(s).

Create a new instance.
```bash
curl -s -X POST -H 'Content-Type: application/json' --data @connector.json http://localhost:8083/connectors
```

Update an existing instance.
```bash
curl -s -X PUT -H 'Content-Type: application/json' --data @connector.json http://localhost:8083/connectors/TestSinkConnector1/config
```



# Sink Connectors


## InfluxDBSinkConnector

The InfluxDBSinkConnector is used to write data from a :term:`Kafka topic`_ to an InfluxDB host. When there are more than one record in a batch that have the same measurement, time, and tags, they will be combined to a single point an written to InfluxDB in a batch.






### Configuration

##### `influxdb.database`
*Importance:* High

*Type:* String


The influxdb database to write to.
##### `influxdb.url`
*Importance:* High

*Type:* String


The url of the InfluxDB instance to write to.
##### `influxdb.password`
*Importance:* High

*Type:* Password

*Default Value:* [hidden]


The password to connect to InfluxDB with.
##### `influxdb.username`
*Importance:* High

*Type:* String


The username to connect to InfluxDB with.
##### `influxdb.consistency.level`
*Importance:* Medium

*Type:* String

*Default Value:* ONE

*Validator:* ValidEnum{enum=ConsistencyLevel, allowed=[ALL, ANY, ONE, QUORUM]}


The default consistency level for writing data to InfluxDB.
##### `influxdb.timeunit`
*Importance:* Medium

*Type:* String

*Default Value:* MILLISECONDS

*Validator:* ValidEnum{enum=TimeUnit, allowed=[NANOSECONDS, MICROSECONDS, MILLISECONDS, SECONDS, MINUTES, HOURS, DAYS]}


The default timeunit for writing data to InfluxDB.
##### `influxdb.gzip.enable`
*Importance:* Low

*Type:* Boolean

*Default Value:* true


Flag to determine if gzip should be enabled.
##### `influxdb.log.level`
*Importance:* Low

*Type:* String

*Default Value:* NONE

*Validator:* ValidEnum{enum=LogLevel, allowed=[NONE, BASIC, HEADERS, FULL]}


influxdb.log.level

#### Examples

##### Standalone Example

This configuration is used typically along with [standalone mode](http://docs.confluent.io/current/connect/concepts.html#standalone-workers).

```properties
name=InfluxDBSinkConnector1
connector.class=com.github.jcustenborder.kafka.connect.influxdb.InfluxDBSinkConnector
tasks.max=1
topics=< Required Configuration >
influxdb.database=< Required Configuration >
influxdb.url=< Required Configuration >
```

##### Distributed Example

This configuration is used typically along with [distributed mode](http://docs.confluent.io/current/connect/concepts.html#distributed-workers).
Write the following json to `connector.json`, configure all of the required values, and use the command below to
post the configuration to one the distributed connect worker(s).

```json
{
  "config" : {
    "name" : "InfluxDBSinkConnector1",
    "connector.class" : "com.github.jcustenborder.kafka.connect.influxdb.InfluxDBSinkConnector",
    "tasks.max" : "1",
    "topics" : "< Required Configuration >",
    "influxdb.database" : "< Required Configuration >",
    "influxdb.url" : "< Required Configuration >"
  }
}
```

Use curl to post the configuration to one of the Kafka Connect Workers. Change `http://localhost:8083/` the the endpoint of
one of your Kafka Connect worker(s).

Create a new instance.
```bash
curl -s -X POST -H 'Content-Type: application/json' --data @connector.json http://localhost:8083/connectors
```

Update an existing instance.
```bash
curl -s -X PUT -H 'Content-Type: application/json' --data @connector.json http://localhost:8083/connectors/TestSinkConnector1/config
```



