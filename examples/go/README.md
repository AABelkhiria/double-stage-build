# Minimal Multi-Stage Docker Go Example

This folder contains a **very minimal Go application** and a multi-stage Dockerfile that demonstrates how to:

1. Build the Go application in a **builder** stage (using the official Golang image).
2. Copy the resulting binary into a **final** lightweight Alpine image.

## Getting Started

1. **Clone the repository** (or copy this folder into your own repo).

2. **Open a terminal** in the `examples/minimal-example` directory.

3. **Build the Docker image**:
    ```bash
    docker build -t minimal-example .
    ```

4. **Run a container using the newly built image**:
    ```bash
    docker run --rm --name minimal-example minimal-example
    ```
    You should see output similar to:
    ```
    Hello from the minimal example!
    ```

## How It Works

- **Stage 1**: Uses `golang:1.20` to compile the Go code.
- **Stage 2**: Uses `alpine:3.17`, then copies the compiled binary from Stage 1. This results in a much **smaller final image** because it doesn’t include all the Golang build tools.

## Customizing the Example

- If you prefer a different language, adjust the Dockerfile accordingly.
- To expose a port (for web services), uncomment or add `EXPOSE 8080` and change the application code to listen on that port.

### Additional Tips

- **Reduce Build Times**: Keep your dependencies (like `go.mod` / `requirements.txt` / `package.json`) separate so Docker can cache them properly.
- **Security**: For production, consider creating a non-root user in the final stage (`adduser` / `addgroup` commands on Alpine) or using more secure base images (like `Distroless`).
