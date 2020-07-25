# syntax=docker/dockerfile:experimental
FROM maven:3.6.3-jdk-8 AS builder

WORKDIR /root/hive
COPY . .
RUN --mount=type=cache,target=/root/.m2 \
  mvn clean install \
    -Pdist \
    -DskipTests \
    -Dmaven.javadoc.skip=true \
  && mv \
    ./packaging/target/apache-hive-$(mvn help:evaluate -Dexpression=project.version -q -DforceStdout)-bin/apache-hive-$(mvn help:evaluate -Dexpression=project.version -q -DforceStdout)-bin \
    ./packaging/target/hive

FROM openjdk:8

COPY --from=builder /root/hive/packaging/target/hive /opt/hive