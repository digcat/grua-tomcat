FROM mq/openjdk8

RUN apk-install --update curl ca-certificates

ENV TOMCAT_VERSION=8.0.32

RUN curl \
  --silent \
  --location \
  --retry 3 \
  --cacert /etc/ssl/certs/ca-certificates.crt \
  "https://archive.apache.org/dist/tomcat/tomcat-8/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz" \
    | gunzip \
    | tar x -C /usr/ \
    && mv /usr/apache-tomcat* /usr/tomcat \
    && rm -rf /usr/tomcat/webapps/examples /usr/tomcat/webapps/docs

RUN wget https://oss.sonatype.org/content/repositories/public/io/hawt/hawtio-default/1.4.63/hawtio-default-1.4.63.war -O /usr/tomcat/webapps/hawtio.war

# SET CATALINE_HOME and PATH
ENV CATALINA_HOME /usr/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin

ADD ./files/run.sh /usr/local/bin/run
RUN chmod +x /usr/local/bin/run

EXPOSE 8080

# override ENTRYPOINT defined in mq/openjdk8.
ENTRYPOINT [ "/bin/sh", "-c" ]
CMD ["/usr/local/bin/run"]
