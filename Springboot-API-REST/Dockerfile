# Etapa 1: Build
FROM maven:3.9.6-eclipse-temurin-17-alpine AS build
WORKDIR /app
# Copiamos el pom y las fuentes
COPY pom.xml .
COPY src ./src
# Compilamos el proyecto (ignorando los tests para un build mas rapido)
RUN mvn clean package -DskipTests

# Etapa 2: Produccion
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Copiamos el .jar compilado desde la etapa anterior
COPY --from=build /app/target/*.jar app.jar
# Exponemos el puerto del backend de Ventas (configurado en 8083)
EXPOSE 8083
# Ejecutamos el servicio
ENTRYPOINT ["java", "-jar", "app.jar"]
