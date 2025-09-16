# Build stage
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY gradle gradle
COPY gradlew .
COPY build.gradle settings.gradle ./
RUN chmod +x ./gradlew
RUN ./gradlew dependencies --no-daemon
COPY src src
RUN ./gradlew bootJar --no-daemon

# Runtime stage
FROM eclipse-temurin:17-jre-alpine
RUN addgroup -g 1001 -S spring && adduser -u 1001 -S spring -G spring
USER spring:spring
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-Xmx512m", "-jar", "app.jar"]
