# cfk-kafka-connect-yugabyte

## Deploy Kafka Connect using CFK

```bash
## Install Confluent for Kubernetes
$ helm repo add confluentinc https://packages.confluent.io/helm
$ kubectl create ns confluent
$ helm upgrade --install operator confluentinc/confluent-for-kubernetes -n confluent
$ kubectl apply -f confluent-platform-singlenode.yaml -n confluent
```

## Verify if Kafka Connect Plugins for Yugabyte have been installed

```bash
## Forward Kafka Connect Pod REST Port
$ kubectl port-forward pod/connect-0 8083:8083 -n confluent
Forwarding from 127.0.0.1:8083 -> 8083
Forwarding from [::1]:8083 -> 8083
Handling connection for 8083

## Verify Connector Plugins
$ curl -X GET http://localhost:8083/connector-plugins | jq
[
  {
    "class": "io.confluent.connect.jdbc.JdbcSinkConnector",
    "type": "sink",
    "version": "10.7.6"
  },
  {
    "class": "io.confluent.connect.jdbc.JdbcSourceConnector",
    "type": "source",
    "version": "10.7.6"
  },
  {
    "class": "io.debezium.connector.yugabytedb.YugabyteDBConnector",
    "type": "source",
    "version": "1.9.5.y.20241-SNAPSHOT"
  },
  {
    "class": "org.apache.kafka.connect.mirror.MirrorCheckpointConnector",
    "type": "source",
    "version": "7.3.0-ce"
  },
  {
    "class": "org.apache.kafka.connect.mirror.MirrorHeartbeatConnector",
    "type": "source",
    "version": "7.3.0-ce"
  },
  {
    "class": "org.apache.kafka.connect.mirror.MirrorSourceConnector",
    "type": "source",
    "version": "7.3.0-ce"
  }
]
```
