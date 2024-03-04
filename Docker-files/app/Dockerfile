FROM openjdk:11 AS BUILD_IMAGE

# Build stage to install dependencies
RUN apt update && apt install maven -y

# Create a directory for the project
WORKDIR /app

# Use a dedicated command for cloning the Git repository
RUN git clone https://github.com/devopshydclub/vprofile-project.git /app

#CMD ls -la

# Build the project within the container
RUN git checkout docker && mvn install

# Tomcat stage with the built application
FROM tomcat:9-jre11

# Clean up existing webapps (optional)
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the WAR file from the build stage
COPY --from=BUILD_IMAGE /app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
