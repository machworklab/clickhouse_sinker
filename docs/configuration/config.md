# Config Items
> Here we use json with comments for documentation

```
{
  // clickhouse configs, it's map[string]ClickHouse for multiple clickhouse
  "clickhouse": {
    // hosts for connection, it's Array(Array(String))
    // we can put hosts with same shard into the inner array
    // it helps data deduplication for ReplicateMergeTree when driver error occurs
    "hosts": [
      [
        "127.0.0.1"
      ]
    ],
    "port": 9000,
    "username": "default"
    "password": "",
    "db": "default",  // database name
    // retryTimes when error occurs in inserting datas
    "retryTimes": 0,
  },

  // kafka configs
  "kafka": {
    "brokers": "127.0.0.1:9093",

    // SSL
    "tls": {
      "enable": false,
      // Required. It's the CA certificate with which Kafka brokers certs be signed.
      "caCertFiles": "/etc/security/ca-cert",
      // Required if Kafka brokers require client authentication.
      clientCertFile: "",
      // Required if and only if ClientCertFile is present.
      clientKeyFile: "",
    }

    // SASL
    "sasl": {
      "enable": false,
      // Mechanism is the name of the enabled SASL mechanism.
      // Possible values: PLAIN, SCRAM-SHA-256, SCRAM-SHA-512, GSSAPI (defaults to PLAIN)
      "mechanism": "PLAIN",
      // Username is the authentication identity (authcid) to present for
      // SASL/PLAIN or SASL/SCRAM authentication
      "username": "",
      // Password for SASL/PLAIN or SASL/SCRAM authentication
      "password": "",
      "gssapi": {
        // authtype - 1. KRB5_USER_AUTH, 2. KRB5_KEYTAB_AUTH
        "authtype": 0,
        "keytabpath": "",
        "kerberosconfigpath": "",
        "servicename": "",
        "username": "",
        "password": "",
        "realm": "",
        "disablepafxfast": false
      }
    },

    // kafka version, if you use sarama, the version must be specified
    "version": "2.2.1"
  },

  "task": {
    "name": "daily_request",
    // kafka topic
    "topic": "topic",
    // kafka consume from earliest or latest
    "earliest": true,
    // kafka consumer group
    "consumerGroup": "group",

    // message parser
    "parser": "json",

    // clickhouse table name
    "tableName": "daily",

    // columns of the table
    "dims": [
      {
        "name": "day",
        "type": "Date",
        "sourceName": "day"
      },
      ...
    ],

    // if it's specified, the schema will be auto mapped from clickhouse,
    "autoSchema" : true,
    // "this columns will be excluded by insert SQL "
    "excludeColumns": []

    // shardingKey is the column name to which sharding against
    "shardingKey": "",
    // shardingPolicy is `stripe,<interval>`(requires ShardingKey be numerical) or `hash`(requires ShardingKey be string)
    "shardingPolicy": "",

    // batch size to insert into clickhouse
    "bufferSize": 90000,
    // min batch size to insert into clickhouse
    "minBufferSize": 1,

    // msg bytes per message
    "msgSizeHint": 1000,

    // interval flush the batch
    "flushInterval": 5,
  },

  // log level
  "logLevel": "debug"
  }
}
```
