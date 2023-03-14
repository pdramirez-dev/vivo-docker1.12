FROM tomcat:9-jdk11-openjdk

MAINTAINER Ralph OFlinn <roflinn@uab.edu>
MAINTAINER Justin Merz <jrmerz@ucdavis.edu>
MAINTAINER Quinn Hart <qjhart@ucdavis.edu>
label   update="Pablo Enrique Diaz <pdramirez.h@gmail.com>" 

RUN apt-get update
RUN apt-get install -y maven git

WORKDIR /usr/local/

#RUN git clone https://github.com/vivo-project/Vitro.git Vitro -b rel-1.12.2-RC
#RUN git clone https://github.com/vivo-project/VIVO.git VIVO -b rel-1.12.2-RC

ENV VITRO_VERSION 1.12.2
RUN wget -O Vitro.zip https://github.com/vivo-project/Vitro/releases/download/vitro-${VITRO_VERSION}/vitro-${VITRO_VERSION}.zip 
RUN unzip Vitro.zip -d Vitro
RUN rm Vitro.zip

ENV VIVO_VERSION 1.12.2
RUN wget -O VIVO.zip https://github.com/vivo-project/VIVO/releases/download/vivo-${VIVO_VERSION}/VIVO-${VIVO_VERSION}.zip
RUN unzip VIVO.zip -d VIVO
RUN rm VIVO.zip

ARG TDB_FILE_MODE=direct
ENV JAVA_OPTS="${JAVA_OPTS} -Dtdb:fileMode=$TDB_FILE_MODE"

WORKDIR /usr/local/VIVO/
COPY ./example-settings.xml example-settings.xml
RUN mvn install -s example-settings.xml

# clean up
RUN rm -r /usr/local/tomcat/webapps/docs
RUN rm -r /usr/local/tomcat/webapps/examples

# Set properties
# Adjust logging, you can mount over this in docker-compose to further adjust
COPY ./log4j.properties /usr/local/tomcat/webapps/VIVO/WEB-INF/classes

WORKDIR /usr/local/VIVO/home/config

COPY ./dockercompose.runtime.properties runtime.properties
COPY ./example.applicationSetup.n3 applicationSetup.n3
#COPY ./dockercompose.runtime.properties /usr/local/vivo/home/config/runtime.properties
#COPY ./example.applicationSetup.n3 /usr/local/vivo/home/config/applicationSetup.n3

RUN chmod ugo+w -R /usr/local/tomcat/temp
RUN chmod ugo+w -R /usr/local/VIVO/home

RUN export CATALINA_OPTS="-Xms512m -Xmx512m -XX:MaxPermSize=128m"
