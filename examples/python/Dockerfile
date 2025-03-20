## Stage 1: Builder
FROM python:3.10-alpine as builder

# Set the working directory in the builder container
WORKDIR /app

# Copy the requirements.txt file to install dependencies
COPY requirements.txt ./

# Install Python dependencies
RUN pip install --user --no-cache-dir -r requirements.txt

# Copy the rest of your source code (including your Flask app)
COPY . .

## Stage 2: Final
FROM python:3.10-alpine as runner

# Create a directory in the final container
WORKDIR /app

# Copy only the installed dependencies and your Python code from the builder stage
COPY --from=builder /root/.local /root/.local
COPY --from=builder /app/src /app

# Add /root/.local/bin to PATH to avoid warnings and run scripts correctly
ENV PATH=/root/.local/bin:$PATH

# (Optional) Expose the port that the Flask app will use
EXPOSE 5000

# Specify default command to run your Flask application
CMD ["python", "/app/app.py"]
