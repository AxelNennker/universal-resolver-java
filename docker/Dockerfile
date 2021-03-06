# Dockerfile

FROM debian:stretch
MAINTAINER Markus Sabadello <markus@danubetech.com>

USER root

RUN apt-get -y update

RUN apt-get install -y --no-install-recommends openjdk-8-jdk maven

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
ENV PATH $JAVA_HOME/bin:$PATH

# build dependencies

RUN apt-get install -y --no-install-recommends git

RUN cd /opt/ && git clone https://github.com/decentralized-identity/did-common-java.git
RUN cd /opt/did-common-java/ && mvn clean install -DskipTests

# build uni-resolver-web

ADD . /opt/uni-resolver-java

RUN cd /opt/uni-resolver-java && mvn clean
RUN cd /opt/uni-resolver-java && mvn install -N -DskipTests
RUN cd /opt/uni-resolver-java/uni-resolver-core && mvn install -N -DskipTests
RUN cd /opt/uni-resolver-java/driver && mvn install -N -DskipTests
RUN cd /opt/uni-resolver-java/driver-http && mvn install -N -DskipTests
RUN cd /opt/uni-resolver-java/uni-resolver-local && mvn install -N -DskipTests
RUN cd /opt/uni-resolver-java/uni-resolver-web && mvn install -N -DskipTests

# done

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 8080

RUN chmod a+rx /opt/uni-resolver-java/docker/run-uni-resolver-web.sh
CMD "/opt/uni-resolver-java/docker/run-uni-resolver-web.sh"
