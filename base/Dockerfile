# DOCKER-VERSION 1.0.1
FROM ubuntu:14.04
LABEL version="1.0"
LABEL description="AEM base image"
MAINTAINER pramar

ENV DEBIAN_FRONTEND noninteractive

# Install utility for AEM
ADD aemInstaller.py /aem/aemInstaller.py
#ADD main.py /main.py

# Add the oak-run jar and compaction.sh for compaction
ADD oak-run-1.2.2.jar /aem/author/oak-run.jar
ADD compaction.sh /aem/author/compaction.sh
ADD org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg /aem/author/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg
ADD oak-run-1.2.2.jar /aem/publish/oak-run.jar
ADD compaction.sh /aem/publish/compaction.sh
ADD org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg /aem/publish/org.apache.jackrabbit.oak.plugins.segment.SegmentNodeStoreService.cfg

RUN apt-get update
RUN apt-get -y install software-properties-common
RUN apt-get -y install ipython ipython3
RUN apt-get -y install python-psutil
RUN apt-get -y install python-pycurl
#RUN apt-get -y install libxrender-dev

RUN apt-add-repository -y ppa:webupd8team/java
RUN DEBIAN_FRONTEND=noninteractive apt-get update -y 

# Enable silent install
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections

RUN apt-get install -y --force-yes oracle-java8-installer

ENV JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre/bin/java
ENV PATH $PATH:/usr/lib/jvm/java-8-oracle/jre/bin/java

#RUN ["python","main.py","-i"]
