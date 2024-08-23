# Clickhouse Cluster On-Prem Deployment

This docker compose is to enable Clickhouse on-prem deployment with Zookeeper and Multi-Replica Clickhouse with storage as Wasabi/S3 buckets

## How does this work?

1. Zookeeper syncs data between multiple clickhouse replicas
2. Clickhouse by defaults uses local disk to store the data and every 10s sync to Wasabi/S3. `You can also update the storage config to use s3 as <cold> and use a TTL table in clickhouse to sync`

## How to run?
`docker compose up -d`

## Configurations
1. Change your Bucket endpoint inside the `clickhouse-storage.xml`
```xml
    <endpoint>https://s3.wasabisys.com/WASABI_BUCKET_NAME/data/</endpoint>
    <access_key_id>WASABI_ACCESS_KEY_ID</access_key_id>
    <secret_access_key>WASABI_SECRET_ACCESS_KEY</secret_access_key>
```
2. Set the Username and Password for Clickhouse in `docker-compose.yml`
```yaml
    environment:
      - CLICKHOUSE_USER=<USERNAME>
      - CLICKHOUSE_PASSWORD=<PASSWORD>
```
3. Add multi-replicas/shards in `cluster-config.xml`
```xml
    <shard>
        <replica>
            <host>clickhouse-2</host>
            <port>9000</port>
        </replica>
    </shard>
```

