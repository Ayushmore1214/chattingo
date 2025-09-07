# Stage 1: Build the application with Maven
FROM maven:3.8.5-openjdk-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

# Stage 2: Create the final, slim image with only the JRE
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
# Copy the built JAR from the builder stage
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]