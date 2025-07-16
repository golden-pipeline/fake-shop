## Fake-shop

Repositório contendo a aplicação **fake-shop**, uma loja fictícia desenvolvida com o objetivo de simular um e-commerce online. A aplicação é construída com **Flask** em Python, com páginas renderizadas em HTML, e utiliza uma pipeline reutilizável baseada na *Golden Pipeline*.

### 📋 Índice

- [Descrição](#descrição)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Execução Local](#execução-local)
- [Empacotamento e Deploy](#empacotamento-e-deploy)
- [Pipeline CI/CD](#pipeline-cicd)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Contribuição](#contribuição-2)
- [Licença](#licença-2)

### 📖 Descrição

O `fake-shop` é uma aplicação web que simula uma loja online, permitindo visualizar e navegar por produtos de maneira fictícia. Ela é usada como base de testes e demonstração dos fluxos de CI/CD da organização.

### 🛠 Tecnologias Utilizadas

- Python
- Flask
- HTML/CSS
- Gunicorn
- Docker

### 💻 Execução Local

Para rodar a aplicação localmente, utilize os comandos abaixo:

```bash
python -m flask db upgrade
python -m gunicorn --bind 0.0.0.0:5000 index:app
```

> 💡 Certifique-se de que todas as variáveis de ambiente estejam definidas corretamente antes da execução.

### 📦 Empacotamento e Deploy

A aplicação é empacotada via Docker. O `Dockerfile` executa um `entrypoint.sh` que roda os comandos necessários para iniciar o serviço.

```bash
docker build -t fake-shop .
docker run -p 5000:5000 --env-file .env fake-shop
```


### 🚀 Pipeline CI/CD

A aplicação `fake-shop` utiliza um **workflow reutilizável do GitHub Actions**, baseado na [Golden Pipeline](#1-remote-pipeline), para automatizar os processos de CI/CD:

```yaml
jobs:
  remote-pipeline:
    uses: golden-pipeline/remote-pipeline/.github/workflows/fake-shop-deploy-local.yml@feature-ecommerce
```

#### Etapas Automatizadas:

1. **Checkout do código**
2. **Build da imagem Docker** com base no seguinte `Dockerfile`:
   - Utiliza a imagem `python:3.11-alpine`
   - Instala dependências do sistema e Python (incluindo `gunicorn`, `flask`, etc.)
   - Copia e instala os pacotes listados em `requirements.txt`
   - Define o diretório `/app` como `WORKDIR`
   - Executa o script de entrada `entrypoint.sh` com as instruções de inicialização da aplicação
   - Expõe a porta `5000`
   - Define a variável `PROMETHEUS_MULTIPROC_DIR=/tmp/metrics` para exportação de métricas para o Prometheus

3. **Push da imagem para o repositório**
4. **Deploy via ArgoCD** em ambiente de desenvolvimento

> 📊 A aplicação também exporta métricas para o **Prometheus**, usando a integração com o Gunicorn e o Flask via `prometheus_client`.

> 💡 A estrutura do Dockerfile garante que o cache seja utilizado de forma eficiente e que as métricas sejam corretamente armazenadas no volume temporário `/tmp/metrics`.



### 🔐 Variáveis de Ambiente

As seguintes variáveis são necessárias para o funcionamento correto da aplicação:

| Variável        | Descrição                            |
|-----------------|----------------------------------------|
| `DB_HOST`       | Host do banco de dados PostgreSQL     |
| `DB_USER`       | Usuário do banco                      |
| `DB_PASSWORD`   | Senha do usuário                      |
| `DB_NAME`       | Nome do banco de dados                |
| `DB_PORT`       | Porta de conexão                      |

> 🔒 Recomendado o uso de um arquivo `.env` para facilitar o carregamento seguro das variáveis.

### 🤝 Contribuição

No momento, o repositório não possui testes automatizados nem documentação da API. Sugestões e contribuições são bem-vindas para evolução do projeto.
