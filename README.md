  # ms-email

Microserviço responsável por consumir mensagens do RabbitMQ, processar dados de envio e realizar o envio de emails utilizando SMTP (Gmail).
As informações de cada envio são persistidas em um banco PostgreSQL.

Este projeto faz parte de um ambiente de microsserviços e foi desenvolvido para estudos utilizando Spring Boot, RabbitMQ, PostgreSQL e Java 21.

---

✔️ Fluxo resumido

1. Outro microserviço publica um objeto EmailDTO na fila RabbitMQ.
2. O ms-email escuta essa fila automaticamente (@RabbitListener).
3. Converte a mensagem em um modelo de domínio.
4. Envia o email usando SMTP Gmail.
5. Salva o status (SENT/ERROR) no banco PostgreSQL.
6. Opcionalmente, permite envio manual via endpoint HTTP.

---

## Tecnologias utilizadas

- Java
- Spring Boot
- Spring Mail
- Spring AMQP
- RabbitMQ
- JPA / Jakarta Persistence
- Lombok

---

## Funcionalidades

- Recebe mensagens da fila do RabbitMQ.
- Envia e-mails usando Spring Mail.
- Salva histórico de envio no banco de dados.
- Atualiza status (ENVIADO / ERRO).

---

## Configurações do application.properties

```
server.port=8080

spring.datasource.url=jdbc:postgresql://localhost:5432/ms-email
spring.datasource.username=postgres
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update

spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=SEU_EMAIL
spring.mail.password=SUA_SENHA
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true

spring.rabbitmq.addresses=amqps://SEU_URL_RABBITMQ
spring.rabbitmq.queue=
```

---

## Exemplo de mensagem enviada à fila

```json
{
  "ownerRef": "Sistema",
  "emailFrom": "seuemail@teste.com",
  "emailTo": "destino@teste.com",
  "subject": "Testando envio",
  "text": "Olá! Este é um teste de envio de e-mail."
}
```

## Exemplo de resposta

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

O objetivo foi aprender na prática: arquitetura de microsserviços, integração com RabbitMQ, envio de emails com Spring Boot, persistência com JPA e PostgreSQL, padrões e boas práticas.
