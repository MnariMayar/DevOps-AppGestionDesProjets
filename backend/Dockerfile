# Étape 1 : compiler le projet avec Maven et Java 17
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copier le fichier Maven
COPY pom.xml .

# Télécharger les dépendances
RUN mvn dependency:go-offline

# Copier le code source
COPY src ./src

# Compiler le projet
RUN mvn clean package -DskipTests


# Étape 2 : exécuter l'application
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copier le JAR généré
COPY --from=build /app/target/*.jar app.jar

# Port du backend Spring Boot
EXPOSE 8080

# Lancer l'application
ENTRYPOINT ["java", "-jar", "app.jar"]
