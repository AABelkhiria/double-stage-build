## Stage 1: Builder
## Possible builder images (uncomment the one you need, or supply your own):
# FROM golang:1.20 as builder
# FROM node:18-alpine as builder
# FROM python:3.10-slim as builder
# FROM maven:3.8.7-eclipse-temurin-17 as builder
# FROM gradle:7.5-jdk17 as builder
# FROM rust:1.68 as builder
# FROM php:8.1-cli as builder

FROM golang:1.20 as builder

# Set the working directory in the builder container
WORKDIR /app

# Copy your dependencies files and run dependency installation

## Example for Go projects: Copy the go.mod and go.sum files to install dependencies first.
# COPY go.mod go.sum ./
# RUN go mod download

## For Node.js, you might do:
# COPY package*.json ./
# RUN npm install

## For Python, you might do:
# COPY requirements.txt ./
# RUN pip install --user --no-cache-dir -r requirements.txt

## For Java with Maven:
# COPY pom.xml ./
# RUN mvn dependency:go-offline

# Copy the rest of your source code
COPY . .

## Build/compile your project For Go:
# RUN go build -o myapp .

## For Node.js (React, Angular, etc.):
# RUN npm run build

## For Python, “building” might not be strictly necessary; or you might compile extensions. Adapt commands as needed.

## For Java with Maven:
# RUN mvn package -DskipTests


## Stage 2: Final
## Choose a lightweight runtime image. Examples:
# FROM alpine:3.17 as final
# FROM debian:stable-slim as final
# FROM gcr.io/distroless/base-debian11 as final (distroless, minimal)
# FROM node:18-alpine as final (if you need Node.js for runtime)

FROM alpine:3.17 as runner

# Create a directory in the final container
WORKDIR /app

# Copy only the final build artifact(s) from the builder stage
COPY --from=builder /app/myapp /app/

## For Node.js:
# COPY --from=builder /app/build /app/build

## For Java:
# COPY --from=builder /app/target/my-app.jar /app/

## For Python:
# COPY --from=builder /root/.local /root/.local
# ENV PATH=/root/.local/bin:$PATH
# COPY --from=builder /app /app

# (Optional) Expose your desired port (if running a webserver, etc.)
EXPOSE 8080

# Specify default command to run your application
CMD ["/app/myapp"]
# If Node.js:
# CMD ["node", "build/server.js"]
# If Java JAR:
# CMD ["java", "-jar", "/app/my-app.jar"]
# If Python:
# CMD ["python", "/app/app.py"]
