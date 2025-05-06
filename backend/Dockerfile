# ---------- base stage ----------
FROM python:3.12-slim AS base
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    POETRY_VERSION=2.3.1

RUN apt-get update && apt-get install -y build-essential libpq-dev && \
    pip install --no-cache-dir poetry

# tylko manifest i lock
WORKDIR /app
COPY pyproject.toml poetry.lock ./
RUN poetry config virtualenvs.create false && poetry install --no-root --no-interaction --no-ansi

# ---------- prod stage ----------
FROM base AS prod
COPY . .
CMD ["gunicorn", "config.asgi:application", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
