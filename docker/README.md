# How to Use the Multi-Stage Dockerfile

This document provides a brief overview of how to work with the **multi-stage** `Dockerfile` located in this directory. Multi-stage builds help keep your final Docker images small by separating the build (or “compile”) phase from the final image that runs in production.

---

## 1. Choose Your Base Images

Inside the `Dockerfile`, you’ll see references to different base images (e.g., `golang:1.20`, `node:18-alpine`, `python:3.10-slim`).  
Pick the ones that fit your project:
- **Builder stage**: Use a base image that includes the build tools you need (compilers, package managers, etc.).  
- **Final stage**: Use a minimal image (e.g., `alpine:3.17` or `distroless`) that only contains what’s strictly needed to run your app.

---

## 2. Copy Dependencies First

To make use of Docker’s layer caching, copy your dependency files (`package.json`, `go.mod`, `requirements.txt`, etc.) before copying the rest of your source code. For example:

```dockerfile
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
```

or, for Node.js:

```dockerfile
COPY package*.json ./
RUN npm install
```

This way, if your dependencies don’t change, Docker can reuse the cached layer and skip re-installing them on subsequent builds.

---

## 3. Build or Compile Your Application

In the builder stage, run the commands required to build, compile, or otherwise prepare your application. For instance:

- Go
```dockerfile
RUN go build -o myapp .
```

- Node.js (for production build of a frontend app):
```dockerfile
RUN npm run build
```

- Python
Python typically doesn’t require a “build,” but you might compile extensions or generate assets.
```dockerfile
RUN python setup.py install  # or similar
```

Adjust the commands for your specific stack.

---

## 4. Create a Minimal Final Image

After building your application, the final stage should be based on a lightweight image. For example, `alpine:3.17` or `python:3.10-slim`.
Copy only the compiled binary or files you need from the builder stage:

```dockerfile
COPY --from=builder /app/myapp /app/
```

For Python, if you’re copying a virtual environment or site-packages, make sure to copy those folders as well:

```dockerfile
COPY --from=builder /opt/venv /opt/venv
ENV PATH=/opt/venv/bin:$PATH
```

---

## 5. Expose Ports and Set the Entry Point

If your application listens on a port (e.g., a web server), use EXPOSE. Then set the default command with CMD or ENTRYPOINT:

```dockerfile
EXPOSE 8080
CMD ["/app/myapp"]
```

---

## 6. Build and Run Your Docker Image

From the root of your project (or wherever your Dockerfile is located), build the Docker image:

```bash
docker build -f docker/Dockerfile -t myapp:latest .
```

Then run the container:

```bash
docker run -p 8080:8080 --name my-running-app myapp:latest
```

Visit `http://localhost:8080` in your browser (or wherever your app runs) to confirm it’s working.

---

## 7. Troubleshooting

- **Build Errors:** Check the logs during the “build” stage to ensure you have all needed dependencies and environment variables set.
- **Dependency Mismatch:** If your app complains that it can’t find dependencies in production, ensure they are installed or copied from the builder stage to the final stage.
- **Overly Large Images:** Try to eliminate unnecessary packages, move ephemeral files to .dockerignore, or pick a smaller base image.

---

## Final Thoughts

Multi-stage builds provide a clean separation between build tools and runtime dependencies. This often leads to:
- Smaller final images (faster deployments, less disk usage).
- Clearer workflows (each stage has a single responsibility).
