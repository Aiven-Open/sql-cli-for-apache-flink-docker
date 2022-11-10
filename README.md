# SQL CLI for Apache Flink® on Docker®


This docker-compose provides an Apache Flink® SQL CLI image updated to the 1.16.0 version. It's inspired by [this](https://github.com/wuchong/flink-sql-demo/tree/v1.11-EN/sql-client).


It makes use of the `flink:1.16.0-scala_2.12` images and of the `ftisiot/flink-sql-client:1.16.0` which is based on the same `flink:1.16.0-scala_2.12` image.

The `sql-client` service maps a `~/kafkacerts/` folder to `/certs` which can be used to create and pass files like Keystores when SSL authentication is needed (e.g. with Apache Kafka®).

Includes the SQL connectors to:
* [Elasticsearch® 7](https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-elasticsearch7/1.16.0/flink-sql-connector-elasticsearch7-1.16.0.jar)
* [Apache Kafka®](https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-kafka/1.16.0/flink-sql-connector-kafka-1.16.0.jar)
* [AVRO](https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-avro-confluent-registry/1.16.0/flink-sql-avro-confluent-registry-1.16.0.jar)
* [JDBC](https://repo.maven.apache.org/maven2/org/apache/flink/flink-connector-jdbc/1.16.0/flink-connector-jdbc-1.16.0.jar)
* [PostgreSQL 42.5.0](https://jdbc.postgresql.org/download/postgresql-42.5.0.jar)
* [Flink Faker](https://github.com/knaufk/flink-faker/releases/download/v0.5.0/flink-faker-0.5.1.jar) allowing to [generate fake data](https://github.com/knaufk/flink-faker)

## Usage

You need both [docker](https://docs.docker.com/engine/install/) and [docker-compose](https://docs.docker.com/compose/install/) installed.

Clone the current repository, navigate to the `flink-sql-cli` folder, then execute

```
docker-compose up -d
```

This will start the 3 nodes Flink cluster, to check which nodes are available use

```
docker-compose ps
```

The result should be similar to the below

```
Name                      Command               State                Ports              
-----------------------------------------------------------------------------------------------
flink_jobmanager_1    /docker-entrypoint.sh jobm ...   Up      6123/tcp, 0.0.0.0:8081->8081/tcp
flink_sql-client_1    /docker-entrypoint.sh            Up      6123/tcp, 8081/tcp              
flink_taskmanager_1   /docker-entrypoint.sh task ...   Up      6123/tcp, 8081/tcp              
```

Apache Flink Web UI is now available at `localhost:8081`

![Flink Web UI](img/flink-web-ui.png)


To Access the SQL CLI, execute

It could be done via one-line command
```
docker exec -it sql-client /opt/sql-client/sql-client.sh
```

Or step by step instructions
```
docker ps
```

And take note of the `CONTAINER_ID` related to the `ftisiot/flink_sql_cli:1.14.3` image (`af00e6ef943f` in the below example)

```
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS         PORTS                              NAMES
af00e6ef943f   ftisiot/flink_sql_cli:1.16.0   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   6123/tcp, 8081/tcp                 flink_sql-client_1
22a459232385   flink:1.16.0-scala_2.12        "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   6123/tcp, 8081/tcp                 flink_taskmanager_1
8b4d5c087800   flink:1.16.0-scala_2.12        "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   6123/tcp, 0.0.0.0:8081->8081/tcp   flink_jobmanager_1
```

Now execute the following code to enter the docker container in interactive mode

```
docker exec -it <CONTAINER_ID> /bin/bash
```

Finally execute

```
./sql-client.sh
```

This will popup Flink sql cli

```
Reading default environment from: file:/opt/flink/conf/sql-client-conf.yaml
No session environment specified.

Command history file path: /root/.flink-sql-history
                                   ▒▓██▓██▒
                               ▓████▒▒█▓▒▓███▓▒
                            ▓███▓░░        ▒▒▒▓██▒  ▒
                          ░██▒   ▒▒▓▓█▓▓▒░      ▒████
                          ██▒         ░▒▓███▒    ▒█▒█▒
                            ░▓█            ███   ▓░▒██
                              ▓█       ▒▒▒▒▒▓██▓░▒░▓▓█
                            █░ █   ▒▒░       ███▓▓█ ▒█▒▒▒
                            ████░   ▒▓█▓      ██▒▒▒ ▓███▒
                         ░▒█▓▓██       ▓█▒    ▓█▒▓██▓ ░█░
                   ▓░▒▓████▒ ██         ▒█    █▓░▒█▒░▒█▒
                  ███▓░██▓  ▓█           █   █▓ ▒▓█▓▓█▒
                ░██▓  ░█░            █  █▒ ▒█████▓▒ ██▓░▒
               ███░ ░ █░          ▓ ░█ █████▒░░    ░█░▓  ▓░
              ██▓█ ▒▒▓▒          ▓███████▓░       ▒█▒ ▒▓ ▓██▓
           ▒██▓ ▓█ █▓█       ░▒█████▓▓▒░         ██▒▒  █ ▒  ▓█▒
           ▓█▓  ▓█ ██▓ ░▓▓▓▓▓▓▓▒              ▒██▓           ░█▒
           ▓█    █ ▓███▓▒░              ░▓▓▓███▓          ░▒░ ▓█
           ██▓    ██▒    ░▒▓▓███▓▓▓▓▓██████▓▒            ▓███  █
          ▓███▒ ███   ░▓▓▒░░   ░▓████▓░                  ░▒▓▒  █▓
          █▓▒▒▓▓██  ░▒▒░░░▒▒▒▒▓██▓░                            █▓
          ██ ▓░▒█   ▓▓▓▓▒░░  ▒█▓       ▒▓▓██▓    ▓▒          ▒▒▓
          ▓█▓ ▓▒█  █▓░  ░▒▓▓██▒            ░▓█▒   ▒▒▒░▒▒▓█████▒
           ██░ ▓█▒█▒  ▒▓▓▒  ▓█                █░      ░░░░   ░█▒
           ▓█   ▒█▓   ░     █░                ▒█              █▓
            █▓   ██         █░                 ▓▓        ▒█▓▓▓▒█░
             █▓ ░▓██░       ▓▒                  ▓█▓▒░░░▒▓█░    ▒█
              ██   ▓█▓░      ▒                    ░▒█▒██▒      ▓▓
               ▓█▒   ▒█▓▒░                         ▒▒ █▒█▓▒▒░░▒██
                ░██▒    ▒▓▓▒                     ▓██▓▒█▒ ░▓▓▓▓▒█▓
                  ░▓██▒                          ▓░  ▒█▓█  ░░▒▒▒
                      ▒▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒░░▓▓  ▓░▒█░

    ______ _ _       _       _____  ____  _         _____ _ _            _  BETA   
   |  ____| (_)     | |     / ____|/ __ \| |       / ____| (_)          | |  
   | |__  | |_ _ __ | | __ | (___ | |  | | |      | |    | |_  ___ _ __ | |_
   |  __| | | | '_ \| |/ /  \___ \| |  | | |      | |    | | |/ _ \ '_ \| __|
   | |    | | | | | |   <   ____) | |__| | |____  | |____| | |  __/ | | | |_
   |_|    |_|_|_| |_|_|\_\ |_____/ \___\_\______|  \_____|_|_|\___|_| |_|\__|

        Welcome! Enter 'HELP;' to list all available commands. 'QUIT;' to exit.


Flink SQL>
```

## Trademarks

Apache Kafka, Apache Flink are either registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries.. Aiven has no affiliation with and is not endorsed by The Apache Software Foundation. Docker is a trademark and property of their respective owners. All product and service names used in this website are for identification purposes only and do not imply endorsement.
