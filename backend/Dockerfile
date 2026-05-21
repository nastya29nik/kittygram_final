FROM python:3.10
WORKDIR /app
COPY requirements.txt .
RUN pip install --default-timeout=100 -r requirements.txt --no-cache-dir
COPY . .
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "kittygram_backend.wsgi"]