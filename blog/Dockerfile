# Use the latest Maven version to build the application
FROM maven:latest AS build


# Set the working directory for the build
WORKDIR /app

# Copy the Maven project files
COPY pom.xml .
COPY src ./src

# Update all packages to their latest versions
RUN apt-get update && apt-get upgrade -y
RUN apt-get install --only-upgrade perl-base
RUN apt-get install --only-upgrade zlib1g

# Build the application
RUN mvn clean package

# Use a minimal Java runtime for the final image
FROM openjdk:21-jdk-slim

# Add a volume to hold the application data
VOLUME /tmp

# The application's jar file
ARG JAR_FILE=target/blog-0.0.1-SNAPSHOT.jar

# Copy the jar file from the build stage to the final image
COPY --from=build /app/${JAR_FILE} app.jar

# Run the jar file
ENTRYPOINT ["java","-jar","/app.jar"]