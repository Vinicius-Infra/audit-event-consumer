# 🚀 Java Audit Service - Microservices

![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%232496ED.svg?style=for-the-badge&logo=docker&logoColor=white)
![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=Apache%20Maven&logoColor=white)
![Swagger](https://img.shields.io/badge/-Swagger-%23C1272D?style=for-the-badge&logo=swagger&logoColor=white)

Esta é uma API REST desenvolvida em Java 17 com Spring Boot 3, focada em auditoria de eventos e persistência de logs distribuídos. O projeto atua como o serviço de suporte para o ecossistema de microsserviços, garantindo o rastreamento de todas as operações de negócio.

---

## 🛠️ Tecnologias e Ferramentas
Linguagem: Java 17

Framework: Spring Boot 3.2.5

Persistência: Spring Data JPA & Hibernate

Banco de Dados: PostgreSQL 15

Documentação: SpringDoc OpenAPI (Swagger)

Build Tool: Maven

Mensageria: Spring Kafka (consumidor em desenvolvimento)

Infraestrutura: Docker & Docker Compose

---

## 🏗️ Arquitetura do Projeto
O projeto segue a estrutura padrão de camadas do Spring, garantindo separação de responsabilidades e alta manutenibilidade:

model: Entidades JPA que definem a estrutura dos logs de auditoria.

repository: Interfaces de comunicação com o banco de dados PostgreSQL.

service: Lógica para processamento e filtragem de eventos recebidos.

controller: Endpoints REST otimizados para recebimento de logs e consultas.

config: Configurações de infraestrutura, CORS e personalização do OpenAPI.

---

## 🚀 Como Rodar o Projeto

Pré-requisitos
JDK 17+

Docker & Docker Compose

1. Subir o Banco de Dados (PostgreSQL)
O Postgres sobe na porta 5433 para evitar conflito com o banco da user-management-service (5434):

docker-compose up -d

O container cria automaticamente o banco microservicesdb, e as tabelas são geradas pelo Hibernate.

2. Compilar e Executar

# Limpar e compilar
./mvnw clean install

# Executar a aplicação
./mvnw spring-boot:run

A aplicação estará disponível em http://localhost:8083.

---

## 📖 Documentação da API
Com a aplicação rodando, você pode acessar a interface interativa do Swagger para testar os endpoints e visualizar os schemas:

🔗 Swagger UI: http://localhost:8083/swagger-ui/index.html

📄 OpenAPI JSON: http://localhost:8083/v3/api-docs

---


## 🧪 Endpoints Principais

Método	                    Endpoint	                      Descrição

GET	                        /audit	                        Lista todos os eventos de auditoria registrados.
POST	                     /audit/log	                      Recebe e armazena um novo evento (UserId, Operação, etc).
GET	                      /hello	                          Endpoint de health check para teste de conectividade.

---


## 🛡️ Observabilidade e Integração
Hoje a integração é síncrona, via HTTP: a [user-management-service](https://github.com/Vinicius-Infra/user-management-service) (Kotlin) chama `POST /audit/log` a cada operação de usuário, registrando:

CREATE: Quando um novo usuário é inserido.

UPDATE: Quando dados de usuários são alterados.

DELETE: Quando um rastro de remoção é necessário.

### Próximo passo: integração assíncrona via Kafka
A user-management-service já publica eventos de criação no tópico `audit-log-topic`, e este serviço já tem o `spring-kafka` configurado (`group-id: audit-group`). O próximo passo é implementar o consumidor (`KafkaConsumerService`) para trocar a chamada HTTP por eventos, com retry e Dead Letter Topic.

---

Autor: Marcos Vinícius da Silva Barreto

LinkedIn: [vinicius-barreto-tec](https://www.linkedin.com/in/vinicius-barreto-tec/)

GitHub: Vinicius-Infra

Developed with ❤️ and Java.
