FROM ubuntu:18.04

RUN apt update && apt install -y dnsutils vim tmux wget curl gnupg watch apt-transport-https

RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
RUN apt update && apt install -y kubectl openjdk-8-jdk

ARG spark_jars=jars
ARG img_path=kubernetes/dockerfiles
ARG k8s_tests=kubernetes/tests

RUN set -ex && \
    mkdir -p /opt/spark && \
    mkdir -p /opt/spark/work-dir && \
    touch /opt/spark/RELEASE && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd

COPY ${spark_jars} /opt/spark/jars
COPY bin /opt/spark/bin
COPY sbin /opt/spark/sbin
COPY ${img_path}/spark/entrypoint.sh /opt/
COPY examples /opt/spark/examples
COPY ${k8s_tests} /opt/spark/tests
COPY data /opt/spark/data

ENV SPARK_HOME /opt/spark

WORKDIR /opt/spark/work-dir

RUN rm $SPARK_HOME/jars/guava-14.0.1.jar
ADD http://central.maven.org/maven2/com/google/guava/guava/23.0/guava-23.0.jar $SPARK_HOME/jars
ADD https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-latest-hadoop2.jar $SPARK_HOME/jars
ADD https://jdbc.postgresql.org/download/postgresql-42.1.1.jar $SPARK_HOME/jars

WORKDIR /

RUN apt update && \
    apt install -y python python3 python-pip python3-pip && \
    rm -rf /root/.cache

COPY python/ ${SPARK_HOME}/python
ENV PYTHONPATH ${SPARK_HOME}/python/lib/pyspark.zip:${SPARK_HOME}/python/lib/py4j-*.zip
ENV PYTHONIOENCODING utf8

RUN pip install requests google-cloud-vision==0.31.0 numpy

RUN pip install https://storage.googleapis.com/lumen5-170819-spark-search-jobs/packages/graphframes-0.7.0.tar.gz

RUN pip install matplotlib

WORKDIR /opt/spark

ENTRYPOINT ["/bin/bash"]
