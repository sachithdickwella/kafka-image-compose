# Dockerfile to create a docker image for Kafka under the defined guideline.
# Copyright (C) 2023  Sachith Dickwella
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.

FROM --platform=linux/amd64 ubuntu:mantic-20230624

RUN groupadd kafka  && useradd -g kafka --home /home/kafka kafka
USER kafka:kafka

WORKDIR /home/kafka
COPY jre/ ./jre

ENV JAVA_HOME=/home/kafka/jre

ARG KAFKA_VERSION=3.5.0
ARG KAFKA_FILE_BIN=kafka_2.13-${KAFKA_VERSION}.tgz

ADD https://downloads.apache.org/kafka/3.5.0/${KAFKA_FILE_BIN} ./
ADD https://downloads.apache.org/kafka/3.5.0/${KAFKA_FILE_BIN}.asc ./
ADD https://downloads.apache.org/kafka/KEYS ./

RUN apk add --no-cache gnupg \
        && gpg --print-md SHA256 ${KAFKA_FILE_BIN} \
        && tar -xzf ${KAFKA_FILE_BIN} -C . --strip-components=1 \
        && rm -f KEYS ${KAFKA_FILE_BIN}.asc ${KAFKA_FILE_BIN} \
        && chown kafka:kafka -R . \
        && chmod 755 -R . \

USER kafka:kafka


#

#
#ADD https://dlcdn.apache.org/kafka/3.2.1/${KAFKA_FILE_BIN} .
#ADD https://dlcdn.apache.org/kafka/3.2.1/${KAFKA_FILE_BIN}.asc .
#ADD https://dlcdn.apache.org/kafka/KEYS .
#
#RUN apk add --no-cache gnupg \
#        && gpg --print-md SHA256 ${KAFKA_FILE_BIN} \
#        && tar -xzf ${KAFKA_FILE_BIN} -C . --strip-components=1 \
#        && rm -f KEYS ${KAFKA_FILE_BIN}.asc ${KAFKA_FILE_BIN} \
#        && chown kafka:kafka -R . \
#        && chmod 755 -R . \
#        && apk add bash
#
#USER kafka:kafka
#
#COPY bin ./bin
#COPY config ./config
#COPY build-scripts ./build-scripts
#
#ENV KAFKA_HEAP_OPTS="-Xms128m -Xmx512m"
#ENV KAFKA_BROKER_ID=0
#ENV KAFKA_NETWORK_THREAD_COUNT=3
#ENV KAFKA_IO_THREAD_COUNT=8
#ENV KAFKA_SOCKET_SEND_BUFFER_BYTES=102400
#ENV KAFKA_SOCKET_RECEIVE_BUFFER_BYTES=102400
#ENV KAFKA_SOCKET_REQUEST_MAX_BYTES=104857600
## Kafka log basic cpnfiguration environment variables.
#ENV KAFKA_LOG_DIRECTORY="/tmp/kafka-logs"
#ENV KAFKA_DEFAULT_PARTITION_COUNT=1
#ENV KAFKA_RECOVERY_THREADS_PER_DATA_DIRECTORY_COUNT=1
## Kafka internal topic settings configuration environment
## variables.
#ENV KAFKA_OFFSET_TOPIC_REPLICATION_FACTOR=1
#ENV KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR=1
#ENV KAFKA_TRANSACTION_STATE_LOG_MIN_ISR=1
## Kafka log flush policy configuration environment variables.
#ENV KAFKA_LOG_FLUSH_INTERVAL_MESSAGE_COUNT=10000
#ENV KAFKA_LOG_FLUSH_INTERVAL_MILLISECONDS=100
## Kafka log retention policy configuration environemnt
## variables.
#ENV KAFKA_LOG_RETENTION_HOURS=168
#ENV KAFKA_LOG_RETENTION_BYTES=1073741824
#ENV KAFKA_LOG_SEGMENT_BYTES=1073741824
#ENV KAFKA_LOG_SEGMENT_CHECK_INTERVAL_MILLISECONDS=300000
## Zookeeper connection and managements configuration
## environemnt variables.
#ENV KAFKA_ZK_CONNECT=127.0.0.1:2181
#ENV KAFKA_ZK_CONNECTION_TIMEOUT_MILLISECONDS=18000
## Group coordinator settings configuration environment
## variables.
#ENV KAFKA_GROUP_INITIAL_RE_BALANCE_DELAY_MILLISECONDS=0
#
#ENTRYPOINT [ "/bin/bash", "bin/entrypoint.sh", "config/server.properties" ]