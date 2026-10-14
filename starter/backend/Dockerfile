FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    libpcre2-dev \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pipenv

RUN useradd --uid 1000 --create-home app

RUN mkdir -p /var/www/app && chown -R app:app /var/www/app

COPY Pipfile* /app/

RUN pipenv install --system --deploy

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
