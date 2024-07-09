FROM tiangolo/uvicorn-gunicorn-fastapi:python3.10
 
WORKDIR /backend
 
RUN apk add --no-cache curl

RUN curl -sSL https://install.python-poetry.org | POETRY_HOME=/opt/poetry python && \
    cd /usr/local/bin && \
    ln -s /opt/poetry/bin/poetry && \
    poetry config virtualenvs.create false
 
COPY ./pyproject.toml ./poetry.lock* /backend/
 
RUN poetry install --no-root --only main
 
ENV PYTHONPATH=/backend

COPY . /backend/

RUN chmod +x /backend/prestart.sh

EXPOSE 8000
 
COPY ./alembic.ini /app/
 
CMD ["/bin/sh", "poetry run sh ./prestart.sh"]
