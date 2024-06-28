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

## Checksum for yugabyte-cdc-source.zip - update [confluent-platform.singlenode.yaml#L81](https://github.com/nav-nandan/cfk-kafka-connect-yugabyte/blob/main/confluent-platform-singlenode.yaml#L81) if new version of jars used
```bash
$ shasum -a 512 yugabyte-cdc-source.zip
3a52ee9f22956fa85a6b1eb5ac066d41e4b05657700d747b320be0dac18da7148748bf27970fe0379ee8f652fe3f1c842f2df587e3d0e7a76a2593a4f7dd1df4  yugabyte-cdc-source.zip
```
