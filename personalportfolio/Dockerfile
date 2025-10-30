# ----- Stage 1: The "Builder" -----
# We use an official Maven image that already has Java 21
FROM maven:3.8-eclipse-temurin-21 AS build

# Set a working directory inside this builder
WORKDIR /app

# Copy just the pom.xml and download dependencies
# This is a fast way to cache your libraries
COPY pom.xml .
RUN mvn dependency:go-offline

# Now copy the rest of your source code
COPY src ./src

# Run the maven build to create the .jar file
# -DskipTests skips running tests during this build
RUN mvn package -DskipTests

# ----- Stage 2: The "Final App" -----
# Use a slim, lightweight image with only Java 21
FROM eclipse-temurin:21-jre-alpine

# Set the working directory
WORKDIR /app

# Copy the .jar file we built in Stage 1
# The JAR file will be in /app/target/ and will be the only .jar
COPY --from=build /app/target/*.jar app.jar

# Tell the world which port to "open"
EXPOSE 8080

# The command to run your app
ENTRYPOINT ["java", "-jar", "app.jar"]