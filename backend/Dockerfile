# Backend build
FROM maven:3.8.5-openjdk-17 AS backend-build
WORKDIR /app
COPY backend/pom.xml /app/pom.xml
COPY backend/src /app/src
RUN mvn clean package -DskipTests

# Frontend build
FROM node:18 AS frontend-build
WORKDIR /frontend
COPY frontend /frontend
RUN npm install
RUN npm run build

# Final Stage: Run Application
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=backend-build /app/target/JobPortal-0.0.1-SNAPSHOT.jar app.jar
COPY --from=frontend-build /frontend/build /static
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
