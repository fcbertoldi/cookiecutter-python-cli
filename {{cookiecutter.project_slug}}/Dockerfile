# Use a minimal Python base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy the source code into the container
COPY . .

# Install build dependencies
RUN pip install --upgrade pip setuptools \
 && pip install .

# Set the entrypoint to the CLI
ENTRYPOINT ["{{cookiecutter.package_name}}"]
