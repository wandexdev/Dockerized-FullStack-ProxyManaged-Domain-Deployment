FROM python:3.10-slim

# working directory
WORKDIR /backend

# system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libpq-dev \
        python3-dev \
        postgresql-client \
        curl \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | POETRY_HOME=/opt/poetry python && \
    cd /usr/local/bin && \
    ln -s /opt/poetry/bin/poetry && \
    poetry config virtualenvs.create false

# Copy poetry.lock* and pyproject.toml
COPY ./pyproject.toml ./poetry.lock* /backend/

# Install project dependencies
RUN poetry install --no-root --no-interaction

# Copy all backend files
COPY . /backend/
RUN chmod +x /backend/prestart.sh

EXPOSE 8000

# environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/backend

# Command to run the application
CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --reload  --host 0.0.0.0 --port 8000"]
