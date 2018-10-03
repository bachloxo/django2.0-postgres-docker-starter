FROM python:3.6-slim

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

# https://github.com/jazzband/pip-tools
WORKDIR /pip-tools
ADD requirements.in /pip-tools/
RUN pip install --no-cache-dir pip-tools
RUN pip-compile --header --annotate
RUN pip install --no-cache-dir -r requirements.txt
WORKDIR /web

# https://github.com/resin-io-library/base-images/issues/273
RUN mkdir -p /usr/share/man/man1 \
    && mkdir -p /usr/share/man/man7

# PostgreSQL client
ADD pgpass /root/.pgpass
RUN DEBIAN_FRONTEND=noninteractive apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends postgresql-client-common postgresql-client-9.6 \
    && chmod 600 /root/.pgpass \
    && DEBIAN_FRONTEND=noninteractive apt-get clean \
    && rm -rf /var/lib/apt/lists/*
