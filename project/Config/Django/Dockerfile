FROM python:3.10-slim AS builder

ENV PYTHONUNBUFFERED=1

ENV PYTHONDONTWRITEBYTECODE=1

ENV PYTHONPYCACHEPREFIX=/tmp/pycache/

ENV DEBIAN_FRONTEND=noninteractive

# COPY ../../requirements.txt requirements.txt
COPY ../../uv.lock uv.lock
COPY ../../pyproject.toml pyproject.toml
# pip install --no-cache-dir -r requirements.txt && \

COPY --from=ghcr.io/astral-sh/uv:0.5.1 /uv /uvx /bin/

RUN apt-get update && \
apt-get install  -y cron build-essential libpq-dev curl --no-install-recommends && \
uv pip install  --system -r pyproject.toml && \   
apt-get clean && \
rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . /app

COPY ./Config/Django/entrypoint.sh ./entrypoint.sh
ENTRYPOINT ["sh", "./entrypoint.sh"]
