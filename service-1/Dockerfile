# Use an official OpenJDK runtime as the base image
FROM openjdk:17-slim


# Copy the built JAR file from your local machine to the container
COPY target/*.jar app.jar

# Command to run the application
CMD ["java", "-jar", "app.jar"]
