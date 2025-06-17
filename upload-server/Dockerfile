FROM python:3.13-slim

WORKDIR /app

COPY app.py .

RUN pip install --no-cache-dir flask prometheus_flask_exporter

VOLUME ["/uploads"]

CMD ["python", "app.py"]
