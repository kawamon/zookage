# syntax=docker/dockerfile:experimental
ARG openjdk_image

# hadolint ignore=DL3006
FROM ${openjdk_image}

# hadolint ignore=DL3008
RUN apt-get update \
  && apt-get install -y --no-install-recommends curl less netcat vim \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && groupadd --gid 1000 zookage \
  && useradd --create-home --uid 1000 --gid zookage zookage \
  && groupadd --gid 1010 hadoop \
  && groupadd --gid 1011 hdfs \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1011 --gid hdfs --groups hadoop hdfs \
  && groupadd --gid 1012 yarn \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1012 --gid yarn --groups hadoop yarn \
  && groupadd --gid 1013 mapred \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1013 --gid mapred --groups hadoop mapred \
  && groupadd --gid 1014 httpfs \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1014 --gid httpfs httpfs \
  && groupadd --gid 1020 hive \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1020 --gid hive hive \
  && groupadd --gid 1030 tez \
  && useradd --system --no-log-init --shell /sbin/nologin --uid 1030 --gid tez tez \
  && mkdir /var/log/hadoop \
  && chown root:hadoop /var/log/hadoop \
  && chmod 775 /var/log/hadoop \
  && mkdir /var/log/httpfs \
  && chown httpfs:httpfs /var/log/httpfs

RUN curl \
    --location \
    --output /usr/local/bin/kubectl \
    https://storage.googleapis.com/kubernetes-release/release/v1.19.3/bin/linux/amd64/kubectl \
  && chmod 755 /usr/local/bin/kubectl \
  && curl \
    --location \
    --output /opt/gohdfs-v2.1.1-linux-amd64.tar.gz \
    https://github.com/colinmarc/hdfs/releases/download/v2.1.1/gohdfs-v2.1.1-linux-amd64.tar.gz \
  && tar \
    --strip-components=1 \
    --directory=/usr/local/bin \
    -zxvf \
    /opt/gohdfs-v2.1.1-linux-amd64.tar.gz \
    gohdfs-v2.1.1-linux-amd64/hdfs \
  && mv /usr/local/bin/hdfs /usr/local/bin/gohdfs \
  && rm -rf /opt/gohdfs-v2.1.1-linux-amd64.tar.gz

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

USER zookage
ENTRYPOINT ["entrypoint.sh"]
