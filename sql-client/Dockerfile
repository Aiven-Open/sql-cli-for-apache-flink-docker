###############################################################################
#  Licensed to the Apache Software Foundation (ASF) under one
#  or more contributor license agreements.  See the NOTICE file
#  distributed with this work for additional information
#  regarding copyright ownership.  The ASF licenses this file
#  to you under the Apache License, Version 2.0 (the
#  "License"); you may not use this file except in compliance
#  with the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
# limitations under the License.
###############################################################################


###############################################################################
#SQL CLI - inspired by https://github.com/wuchong/flink-sql-demo/tree/v1.11-EN/sql-client
###############################################################################

FROM flink:1.17.1-scala_2.12-java11

# Create CLI lib folder
COPY sql-client/bin/* /opt/sql-client/
RUN mkdir -p /opt/sql-client/lib

# Download connector libraries
RUN wget -P /opt/sql-client/lib/ https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-elasticsearch7/3.0.1-1.17/flink-sql-connector-elasticsearch7-3.0.1-1.17.jar; \
    wget -P /opt/sql-client/lib/ https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-kafka/1.17.1/flink-sql-connector-kafka-1.17.1.jar; \
    wget -P /opt/sql-client/lib/ https://repo.maven.apache.org/maven2/org/apache/flink/flink-connector-jdbc/3.1.1-1.17/flink-connector-jdbc-3.1.1-1.17.jar; \
    wget -P /opt/sql-client/lib/ https://repo.maven.apache.org/maven2/org/apache/flink/flink-json/1.7.1/flink-json-1.7.1.jar; \
    wget -P /opt/sql-client/lib/ https://jdbc.postgresql.org/download/postgresql-42.5.0.jar; \
    wget -P /opt/sql-client/lib/ https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-avro-confluent-registry/1.17.1/flink-sql-avro-confluent-registry-1.17.1.jar; \
    wget -P /opt/sql-client/lib/ https://github.com/knaufk/flink-faker/releases/download/v0.5.1/flink-faker-0.5.1.jar; 

# Copy configuration
COPY sql-client/conf/* /opt/flink/conf/

WORKDIR /opt/sql-client
ENV SQL_CLIENT_HOME /opt/sql-client

COPY sql-client/docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]