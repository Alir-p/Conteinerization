FROM python:3.9.16-slim

WORKDIR /code

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY app.py .

EXPOSE 8080

CMD ["python", "./app.py"]
