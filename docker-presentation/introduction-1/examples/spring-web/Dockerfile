FROM openjdk:8-jdk-slim
MAINTAINER slam.han <slam.han@finnplay.com>

ADD ./target/demo-0.0.1-SNAPSHOT.jar /app/
CMD ["java", "-Xmx200m", "-jar", "/app/demo-0.0.1-SNAPSHOT.jar"]