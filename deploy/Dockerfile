FROM gcr.io/redeo-all/docker-core-base:adoptopenjdk-8u202-jdk-411-nr-alpine
LABEL maintainer="corerealestate@navent.com"

WORKDIR /home/navent/app

ARG NODE_ENV
ENV NODE_ENV=$NODE_ENV
RUN echo ${NODE_ENV}

ARG JMX_IP
ENV JMX_IP=$JMX_IP
RUN echo ${JMX_IP}

ARG JMX_PORT
ENV JMX_PORT=$JMX_PORT
RUN echo ${JMX_PORT}

COPY realestate-bsre-mobile-0.11.1.jar realestate-bsre-legacy.jar

EXPOSE 8080
EXPOSE 1099

CMD java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap \
	-XX:+UseContainerSupport -XX:InitialRAMPercentage=60.0 -XX:MaxRAMPercentage=60.0 \
	-Dcom.sun.management.jmxremote \
	-Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.local.only=false \
	-Dcom.sun.management.jmxremote.port=${JMX_PORT} -Dcom.sun.management.jmxremote.rmi.port=${JMX_PORT} \ 
	-Dcom.sun.management.jmxremote.ssl=false -Djava.rmi.server.hostname=${JMX_IP} \
	-Dnewrelic.config.file=/home/navent/app/newrelic.yml -Dnewrelic.config.license_key=${NR_LICENSE} \
	-Dnewrelic.config.app_name="${NR_APPNAME}" -javaagent:/home/navent/app/newrelic.jar \
	-jar /home/navent/app/realestate-bsre-legacy.jar --spring.profiles.active=${NODE_PROFILE}