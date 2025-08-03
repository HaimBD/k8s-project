FROM python:2.7-onbuild
ENV PYTHONPATH=/usr/src/app
ENV TEST='test123456'
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "producer.py", "-p", "${RABBITMQ_DEFAULT_PORT}", "-s", "${RABBITMQ_DEFAULT_SERVICE}", "-m", "${RABBITMQ_DEFAULT_MESSAGE}", "-r", "30"]