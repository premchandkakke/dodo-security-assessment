FROM python:3.6-slim

WORKDIR /app

RUN groupadd -r appgroup && \
    useradd -r -g appgroup -u 10001 appuser

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

RUN chown -R appuser:appgroup /app

USER 10001:10001

EXPOSE 8080

CMD ["python", "app.py"]
