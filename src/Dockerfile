FROM python:3.11-alpine

# Atualiza pacotes e instala dependências do sistema operacional
RUN apk add --no-cache gcc musl-dev libffi-dev openssl-dev python3-dev cargo

# Atualiza setuptools para corrigir a vulnerabilidade
RUN pip install --no-cache-dir --upgrade setuptools

# Expoe a porta para a aplicação
EXPOSE 5000

# Define variável do Flask
ENV FLASK_APP=index

# Copia os requisitos antes do código para otimizar cache do Docker
COPY requirements.txt .

# Instala dependências do Python
RUN pip install --no-cache-dir -r requirements.txt

# Define o diretório de trabalho
WORKDIR /app

# Copia o código-fonte
COPY . /app

# Dá permissão de execução ao script de entrada
RUN chmod +x /app/entrypoint.sh

# Define variável para métricas do Prometheus
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/metrics
RUN mkdir -p /tmp/metrics 

# Define usuário não-root para rodar o container.
RUN adduser -D appuser
USER appuser

# Comando de execução
CMD ["sh", "/app/entrypoint.sh"]
