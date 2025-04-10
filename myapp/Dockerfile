FROM openjdk:21-slim
COPY target/myapp-1.0-SNAPSHOT.jar /app.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "/app.jar"]
