FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV CATALINA_HOME=/opt/tomcat
ENV PATH=$CATALINA_HOME/bin:$PATH

# Install dependencies
RUN apt-get update && \
    apt-get install -y openjdk-11-jdk curl && \
    rm -rf /var/lib/apt/lists/*

# Download and extract Tomcat
RUN curl -fsSL https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.24/bin/apache-tomcat-10.1.24.tar.gz | tar xz -C /opt && \
    mv /opt/apache-tomcat-10.1.24 $CATALINA_HOME && \
    rm -rf $CATALINA_HOME/webapps/examples $CATALINA_HOME/webapps/docs

EXPOSE 8080

WORKDIR $CATALINA_HOME

CMD ["catalina.sh", "run"]