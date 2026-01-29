# Use Python 3.11 slim image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install git (required for GitPython)
RUN apt-get update && \
    apt-get install -y git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements first for better caching
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY code_agent/ ./code_agent/
COPY config/ ./config/
COPY pyproject.toml setup.py ./

# Create a simple entry point instead of installing package
ENV PYTHONPATH=/app
RUN pip install --no-cache-dir typer rich

# Create non-root user
RUN useradd -m -u 1000 codeagent && \
    chown -R codeagent:codeagent /app

USER codeagent

# Set entrypoint
ENTRYPOINT ["python", "-m", "code_agent.cli"]
CMD ["--help"]

