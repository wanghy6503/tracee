# JBoss7 with deployed tracee-examples-jaxws and tracee-examples-ejb
#
# VERSION       1.0

FROM dockerfile/java
MAINTAINER Daniel Wegener "daniel.wegener@holisticon.de"

ENV DEBIAN_FRONTEND noninteractive

# Installing maestro-ng helpers
RUN apt-get -y install python python-pip git
RUN pip install git+git://github.com/signalfuse/maestro-ng

# Download & install Tomcat 7
RUN cd / && wget -q http://www.eu.apache.org/dist/tomcat/tomcat-7/v7.0.52/bin/apache-tomcat-7.0.52.tar.gz \
&& tar -xf apache-tomcat-7.0.52.tar.gz && rm apache-tomcat-7.0.52.tar.gz \
&& ln -s apache-tomcat-7.0.52 apache-tomcat

# Enable Tomcat remote debugging
ENV JPDA_ADDRESS 8000
ENV JPDA_TRANSPORT dt_socket
ENV CATALINA_HOME /apache-tomcat

# Download logback
ADD http://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.6.6/slf4j-api-1.6.6.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.6.6/jul-to-slf4j-1.6.6.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.6.6/jcl-over-slf4j-1.6.6.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/ch/qos/logback/logback-classic/0.9.30/logback-classic-0.9.30.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/ch/qos/logback/logback-core/0.9.30/logback-core-0.9.30.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/net/logstash/logback/logstash-logback-encoder/2.4/logstash-logback-encoder-2.4.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/me/moocar/logback-gelf/0.10p2/logback-gelf-0.10p2.jar $CATALINA_HOME/lib/
ADD http://repo1.maven.org/maven2/com/google/code/gson/gson/2.2.1/gson-2.2.1.jar $CATALINA_HOME/lib/

# Bridge JUL to Logback
# ADD logging.properties /apache-tomcat/conf/logging.properties

ENV CATALINA_OPTS -Dlogback.configurationFile=file:$CATALINA_HOME/conf/logback.xml

ADD logback.xml $CATALINA_HOME/conf/

# Download tracee-artifacts
ADD https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=de.holisticon.util.tracee.examples&a=tracee-examples-webapp&e=war&v=LATEST $CATALINA_HOME/webapps/tracee-frontend.war

EXPOSE 8000 8080

ENTRYPOINT env && /apache-tomcat/bin/catalina.sh run