# Stage 1: Builder
FROM golang:1.20 AS builder

# Set the working directory
WORKDIR /app

# Copy go.mod and go.sum first for dependency caching
COPY src/go.mod ./
RUN go mod download

# Copy the rest of the source code
COPY src/ .

# Build the Go application
RUN go build -o minimal-example

# Stage 2: Final
FROM alpine:3.17 AS final

# Create a non-root user for security (optional)
# RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app

# Copy the statically built binary from the builder
COPY --from=builder /app/minimal-example /app/

# Run the application
CMD ["./minimal-example"]
