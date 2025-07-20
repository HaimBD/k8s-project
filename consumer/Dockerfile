FROM python:2.7-onbuild
ENV PYTHONPATH=/usr/src/app
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "consumer.py", "-p", "5672", "-s", "rabbitmq"]