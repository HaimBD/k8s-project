FROM python:2.7-onbuild
ENV PYTHONPATH=/usr/src/app
ENV TEST='test12345678'
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "consumer.py", "-p", "5672", "-s", "rabbitmq-helm"]