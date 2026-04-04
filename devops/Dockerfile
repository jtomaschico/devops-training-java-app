# Usamos una imagen base de Maven con OpenJDK
FROM maven:3.8.6-openjdk-11-slim AS build

#Metadatos
LABEL maintainer="bimation@gmail.com"
LABEL description="Aplicación Java con Maven y Docker"

# Definir el argumento de versión (usado para pasar el valor de VERSION en el build)
ARG VERSION

# Instalar dependencias básicas (como sudo, curl, bash, make, Node.js)
RUN apt-get update && \
    apt-get install -y sudo curl bash make && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    node -v && npm -v

ENV APP_HOME=/home/app    

# Copiar solo el archivo `pom.xml` para aprovechar el cache de Maven
COPY pom.xml /home/app/pom.xml

# Establecer el directorio de trabajo
WORKDIR $APP_HOME

# Descargar las dependencias de Maven (esto se puede cachear)
RUN mvn dependency:go-offline

# Copiar todo el código fuente
COPY ./src /home/app/src

# Compilación del proyecto
RUN mvn versions:set -DnewVersion=${VERSION} && \
    mvn install

# Stage de "Package" donde empaquetamos la aplicación con OpenJDK
FROM eclipse-temurin:11-jre-jammy

# Crear el usuario joseto (en caso de que se necesite para ejecutar la aplicación)
RUN useradd -ms /bin/bash joseto

# Establecer usuario
USER joseto


# Copiar el archivo JAR desde el contenedor anterior (con Maven)
COPY --from=build /home/app/target/*.jar /usr/local/lib/app.jar
#COPY --from=build /home/app/target/spring-boot-2-hello-world-${VERSION}.jar /usr/local/lib/app.jar

# Definir el punto de entrada para ejecutar la aplicación
CMD ["java", "-jar", "/usr/local/lib/app.jar"]
















