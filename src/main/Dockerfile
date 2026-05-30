# --- Stage 1: Build the application using Maven ---
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /app

# Copy all your project files into the container
COPY . .

# Package the application into a WAR file, skipping local unit tests
RUN mvn clean package -DskipTests

# --- Stage 2: Run the application using Tomcat 11 ---
FROM tomcat:11.0-jdk21

# Clean out default Tomcat landing apps to prevent conflicts
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the compiled .war file from Stage 1 into Tomcat's deployment folder
# Renaming it to ROOT.war means your app loads directly on the main domain URL
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]