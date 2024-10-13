# syntax=docker/dockerfile:1

ARG PYTHON_VERSION=3.12
FROM python:${PYTHON_VERSION}

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=false \
    POETRY_VIRTUALENVS_CREATE=false

WORKDIR /app

ARG UID=10001 
ARG GID=10001 

RUN addgroup \
    --system \
    --gid "${GID}" \
    app

RUN adduser \
    --system \
    --disabled-password \
    --home "/home/app" \
    --shell "/bin/bash" \
    --uid "${UID}" \
    --ingroup app \
    app

RUN apt update
RUN pip install --upgrade pip 
RUN pip install "poetry==1.8.3"

COPY --chown=app:app poetry.lock pyproject.toml /app/

RUN poetry install --no-dev --no-root

RUN chown -R app:app ./

USER app

COPY --chown=app:app . ./

EXPOSE 8000