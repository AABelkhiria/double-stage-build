# Minimal Multi-Stage Docker Python Example

This folder contains a **very minimal Flask application** and a multi-stage Dockerfile that demonstrates how to:

1. Build a Python application in a **builder** stage (using the official Python image).
2. Copy the necessary files into a **final** lightweight Alpine image.

## Getting Started

### Prerequisites
- **Docker installed** on your machine.

### Steps to Run

1. **Clone the repository** (or copy this folder into your own repo).

2. **Open a terminal** in the project directory.

3. **Build the Docker image**:
    ```bash
    docker build -t flask-app .
    ```

4. **Run a container using the newly built image**:
    ```bash
    docker run -p 5000:5000 flask-app
    ```

5. **Access the Flask application**:
    - Open your browser and visit: [http://127.0.0.1:5000](http://127.0.0.1:5000)
    - Or test using `curl`:
      ```bash
      curl http://127.0.0.1:5000
      ```

## How It Works

- **Stage 1 (Builder)**: Uses `python:3.10-alpine` to install dependencies.
- **Stage 2 (Final)**: Uses another `python:3.10-alpine` image, then copies the necessary files from Stage 1, resulting in a **smaller final image**.

## Customizing the Example

- Modify `main.py` to change the Flask app behavior.
- To change the exposed port, edit the `EXPOSE` instruction in the Dockerfile and adjust `docker run` accordingly.

### Additional Tips

- **Reduce Build Times**: Keep dependencies (like `requirements.txt`) separate so Docker can cache them properly.
- **Security**: For production, consider using a **non-root user** in the final stage or more secure base images (like `Distroless`).

