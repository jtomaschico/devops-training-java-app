#
# Build stage
#
FROM maven:3.9.6-eclipse-temurin-17 as build
ARG VERSION=0.0.1-SNAPSHOT

# Instalar Node.js 20 y dependencias necesarias
RUN apt-get update && \
    apt-get install -y curl bash && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    node -v && npm -v

COPY ./src /home/app/src
COPY ./pom.xml /home/app

WORKDIR /home/app

# Configurar versión y compilar
RUN mvn versions:set -DnewVersion=${VERSION}
RUN mvn install

#
# Package stage
#
FROM eclipse-temurin:11-jre-jammy
ARG VERSION=0.0.1-SNAPSHOT

COPY --from=build /home/app/target/spring-boot-2-hello-world-${VERSION}.jar /usr/local/lib/app.jar

ENTRYPOINT ["java","-jar","/usr/local/lib/app.jar"]


