FROM openjdk:17-jdk-slim
WORKDIR /app
COPY /target/order-api-0.0.1-SNAPSHOT.jar .
RUN apt-get update && \
apt-get install -y --no-install-recommends jq && \
apt-get clean && \
rm -rf /var/lib/apt/lists/*
CMD ["java", "-jar", "order-api-0.0.1-SNAPSHOT.jar"]
EXPOSE 8080
