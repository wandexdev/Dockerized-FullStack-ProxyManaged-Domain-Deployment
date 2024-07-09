FROM python:3.11.9-alpine

# environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/backend

# working directory
WORKDIR /backend

# system dependencies
RUN apk add --no-cache curl

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | POETRY_HOME=/opt/poetry python && \
    cd /usr/local/bin && \
    ln -s /opt/poetry/bin/poetry && \
    poetry config virtualenvs.create false

# Copy poetry.lock* and pyproject.toml
COPY ./pyproject.toml ./poetry.lock* /backend/

# Install project dependencies
RUN poetry install --no-root

# Copy all backend files
COPY . /backend/
RUN chmod +x /backend/prestart.sh

EXPOSE 8000

# Command to run the application
CMD ["/bin/sh", "-c", "poetry run sh ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]

