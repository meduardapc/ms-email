# 📧 ms-email - Microserviço de Envio de Emails

Microserviço responsável por consumir mensagens do RabbitMQ, processar dados de envio e realizar o envio de emails utilizando SMTP (Gmail).
As informações de cada envio são persistidas em um banco PostgreSQL.

Este projeto faz parte de um ambiente de microsserviços e foi desenvolvido para estudos utilizando Spring Boot, RabbitMQ, PostgreSQL, Log4j2 e Java 21.

---

✔️ Fluxo resumido

1. Outro microserviço publica um objeto EmailDTO na fila RabbitMQ.
2. O ms-email consome essa fila automaticamente via @RabbitListener.
3. A mensagem é convertida em um modelo de domínio.
4. O SMTP envia o e-mail via Gmail.
5. O status é persistido no banco PostgreSQL com data e horário do envio.
6. Também é possível o envio manual via endpoint HTTP.

---

## Tecnologias utilizadas

- Java 21
- Spring Boot 3.3.4
- Spring Web
- Spring Mail (SMTP Gmail)
- Spring AMQP
- Spring Validation
- JPA / Jakarta Persistence
- Lombok
- RabbitMQ
- PostgreSQL
- Log4j2
---

## Funcionalidades

- Recebe mensagens da fila do RabbitMQ ou endpoint REST.
- Envia e-mails usando Spring Mail.
- Salva histórico de envio no banco de dados.
- Atualiza status.
- Registra eventos, erros e fluxo da aplicação pelo logging configurado via Log4j2.

---

## Configurações do _application.properties_

```
server.port=8080

spring.datasource.url=jdbc:postgresql://localhost:5432/ms-email
spring.datasource.username=postgres
spring.datasource.password=SUA_SENHA
spring.jpa.hibernate.ddl-auto=update

spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=SEU_EMAIL
spring.mail.password=SUA_SENHA
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true

spring.rabbitmq.addresses=amqps://SEU_URL_RABBITMQ
spring.rabbitmq.queue=SUA_QUEUE

logging.level.com.ms.email= TRACE
logging.level.root= DEBUG
```

---

## Endpoint REST

Exemplo de mensagem enviada à fila:

```json
{
  "ownerRef": "Sistema",
  "emailFrom": "seuemail@teste.com",
  "emailTo": "destino@teste.com",
  "subject": "Testando envio",
  "text": "Olá! Este é um teste de envio de e-mail."
}
```

Exemplo de retorno bem-sucedido:

```json
{
  "emailId": 1,
  "ownerRef": "UserService",
  "emailFrom": "seuemail@gmail.com",
  "emailTo": "destinatario@gmail.com",
  "subject": "Bem-vindo!",
  "text": "Seu cadastro foi realizado com sucesso.",
  "sendDateEmail": "2025-12-01T15:22:10",
  "statusEmail": "SENT"
}
```

---

## Créditos

Projeto desenvolvido seguindo o tutorial do canal:  
https://www.youtube.com/@MichelliBrito

O objetivo foi aprender na prática: arquitetura de microsserviços, mensageria com RabbitMQ, envio de emails com Spring Boot, persistência com JPA e PostgreSQL, logs com Log4j2, padrões e boas práticas.
