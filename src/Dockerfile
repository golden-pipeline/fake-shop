FROM python:3.11-alpine
# Atualiza pacotes e instala dependências do sistema operacional
RUN apk add --no-cache gcc musl-dev libffi-dev openssl-dev python3-dev cargo
EXPOSE 5000
ENV FLASK_APP=index
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
WORKDIR /app
COPY . /app
RUN chmod +x /app/entrypoint.sh
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/metrics
RUN mkdir -p /tmp/metrics 
CMD ["/app/entrypoint.sh"]