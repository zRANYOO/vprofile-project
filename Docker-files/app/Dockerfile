# Stage 1: Download code (assuming internet access)
FROM alpine AS download-code

WORKDIR /app
RUN apk add --no-cache git

RUN git clone https://github.com/devopshydclub/vprofile-project.git .

# Stage 2: Build project
FROM openjdk:11 AS build-project

WORKDIR /app
#RUN ls -la /app
COPY --from=download-code /app .

RUN apt update && apt install maven -y

WORKDIR /app/vprofile-project
RUN git checkout docker && mvn install

# Stage 3: Package application (Tomcat image)
FROM tomcat:9-jre11

# Clean up existing webapps (optional)
RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=build-project vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
