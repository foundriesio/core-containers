FROM alpine:latest

LABEL maintainer="kai.zimmermann@bosch-si.com"
LABEL maintainer="mike@foundries.io"

ENV HAWKBIT_VERSION=0.2.1 \
    HAWKBIT_HOME=/opt/hawkbit

# Http port
EXPOSE 8080

RUN apk add --no-cache libressl openjdk8-jre-base
RUN mkdir -p $HAWKBIT_HOME
COPY hawkbit-update-server-$HAWKBIT_VERSION-SNAPSHOT.jar $HAWKBIT_HOME/hawkbit-update-server.jar

VOLUME "$HAWKBIT_HOME/data"

WORKDIR $HAWKBIT_HOME
ENTRYPOINT ["java","-jar","hawkbit-update-server.jar","-Xmx768m -Xmx768m -XX:MaxMetaspaceSize=250m -XX:MetaspaceSize=250m -Xss300K -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+UseCompressedOops -XX:+HeapDumpOnOutOfMemoryError"]
