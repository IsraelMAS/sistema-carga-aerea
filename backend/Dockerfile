# Etapa 1: Compilar la aplicación con Maven y Java 21
FROM eclipse-temurin:21-jdk-jammy AS build
WORKDIR /app

# Copiar el wrapper y la configuración de dependencias
COPY mvnw pom.xml ./
COPY .mvn .mvn

# Dar permisos de ejecución al wrapper y descargar dependencias
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline -B

# Copiar el código fuente y compilar el archivo jar
COPY src src
RUN ./mvnw clean package -DskipTests

# Etapa 2: Imagen ligera para ejecución
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app

# Copiar el .jar compilado desde la etapa anterior
COPY --from=build /app/target/*.jar app.jar

# Puerto por defecto de Render
ENV PORT=8080
EXPOSE 8080

# Iniciar la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]