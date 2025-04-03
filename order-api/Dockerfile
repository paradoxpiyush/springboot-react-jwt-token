FROM maven:3.8-openjdk-17-slim AS build
WORKDIR /app
COPY pom.xml .
COPY .mvn/ .mvn/
COPY mvnw .
RUN chmod +x mvnw
# Download dependencies first (for better caching)
RUN ./mvnw dependency:go-offline -B

# Copy source and build
COPY src/ src/
RUN ./mvnw package -DskipTests

################################################################################3

# Use a lightweight JRE for runtime
FROM eclipse-temurin:17-jre-alpine

# Set the working directory
WORKDIR /app

# Install jq
RUN apk --no-cache add jq

# Create a non-root user
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copy the jar file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the application port
EXPOSE 8080

# Set healthcheck
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:8080/actuator/health || exit 1


# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
