#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

ARG base_img
FROM $base_img
WORKDIR /
RUN mkdir ${SPARK_HOME}/python

RUN apt-get update && \
    apt-get install -y python python-pip && \
    pip install --upgrade pip setuptools && \
    rm -rf /var/lib/apt/lists/*

ENV PATH ${PATH}:/usr/local/bin

RUN apt-get update && \
    apt-get install build-essential python-dev wget && \
    wget https://bootstrap.pypa.io/get-pip.py && \
    python ./get-pip.py && \
    rm get-pip.py && \
    pip install \
        requests \
        google-cloud-vision==0.31.0 \
        numpy \
        matplotlib && \
    apt-get remove --purge build-essential python-dev wget && \        
    rm -rf /var/lib/apt/lists/*

COPY python/lib ${SPARK_HOME}/python/lib
ENV PYTHONPATH ${SPARK_HOME}/python/lib/pyspark.zip:${SPARK_HOME}/python/lib/py4j-*.zip
ENV PYTHONIOENCODING utf8

RUN pip install https://storage.googleapis.com/lumen5-170819-spark-search-jobs/packages/graphframes-0.7.0.tar.gz

WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]
