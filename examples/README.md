# Multi-Stage Docker Build Examples

This repository contains **multi-stage Docker build examples** for different programming languages. Each example demonstrates how to:

- Build an application in a **builder stage** using official language images.
- Copy the necessary artifacts into a **final lightweight image** for optimized performance.

## Available Examples

- **Go**: A minimal example using `golang:1.20` and `alpine:3.17`.
- **Python**: A Flask-based web application using `python:3.10-alpine`.

## Getting Started

Navigate to the specific language directory to see more details and build instructions:

```bash
cd examples/go    # For Go example
cd examples/python  # For Python example
```

Each directory contains its own README with step-by-step instructions.

