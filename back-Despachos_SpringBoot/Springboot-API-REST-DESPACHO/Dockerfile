FROM maven:3.8.8-eclipse-temurin-17 AS build
WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

RUN addgroup -S devopsgroup && adduser -S devopsuser -G devopsgroup

COPY --from=build /app/target/*.jar app.jar

RUN chown -R devopsuser:devopsgroup /app

USER devopsuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]