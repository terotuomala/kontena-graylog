# Graylog (MongoDB, Elasticsearch) for Kontena environment
Kontena Stack for running Graylog. Fluentd outputs the data to Graylog in the GELF format (Graylog Extended Log Format).

### Prerequisites
Increase the `max_map_count` on every host where Elasticsearch will be running.
```
sudo sysctl -w vm.max_map_count=262144
```

### Usage with Kontena
Graylog, MongoDB and Elasticsearch needs instance scoped volumes to persist their data. Create the volumes:
```
kontena volume create --scope instance --driver local mongo-data
kontena volume create --scope instance --driver local elasticsearch-data
kontena volume create --scope instance --driver local graylog-data
```
Install the stack:
```
kontena stack install
```
Enable fluentd forwarding in grid:
```
kontena grid update --log-forwarder fluentd --log-opt fluentd-address=graylog-gelf.GRID-NAME.kontena.local:24224 GRID-NAME
```
Disable fluentd forwarding in grid:
```
kontena grid update --no-log-forwarder GRID-NAME
```

### Additional information
- Databases ``admin`` and ``graylog`` will be created to MongoDB.
- MongoDB users ``admin`` and ``graylog`` will be created and their passwords will be stored to Kontena Vault.
- Graylog uses initial username ``admin`` and password ``admin``
