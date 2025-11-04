FROM python:3.9-slim
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*
RUN pip install --no-cache-dir \
    mlflow==2.8.1 \
    psycopg2-binary \
    boto3
WORKDIR /app
EXPOSE 5000
CMD ["mlflow", "server", "--host", "0.0.0.0"]
