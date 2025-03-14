FROM python:3.11.0
EXPOSE 5000
ENV FLASK_APP=index
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
WORKDIR /app
COPY . /app

# Aplica as migrações e marca a versão atual
RUN flask db upgrade
RUN flask db stamp head

RUN chmod +x /app/entrypoint.sh
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/metrics
RUN mkdir -p /tmp/metrics 
CMD ["/app/entrypoint.sh"]