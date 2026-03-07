FROM maven:3.8.6-openjdk-11-slim as build

# Crear usuario joseto
RUN useradd -ms /bin/bash joseto

# Ejecutar las instalaciones y configuraciones como root
RUN apt-get update && \
    apt-get install -y sudo curl bash make && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs npm && \
    node -v && npm -v

# Cambiar al usuario joseto
USER joseto

# Copiar el código y el archivo de configuración
COPY ./src /home/app/src
COPY ./pom.xml /home/app

WORKDIR /home/app

# Compilación con Maven
RUN mvn versions:set -DnewVersion=${VERSION}
RUN mvn install

# Package stage
FROM eclipse-temurin:11-jre-jammy

# Copiar el archivo JAR desde la etapa de build
COPY --from=build /home/app/target/spring-boot-2-hello-world-${VERSION}.jar /usr/local/lib/app.jar

ENTRYPOINT ["java","-jar","/usr/local/lib/app.jar"]














