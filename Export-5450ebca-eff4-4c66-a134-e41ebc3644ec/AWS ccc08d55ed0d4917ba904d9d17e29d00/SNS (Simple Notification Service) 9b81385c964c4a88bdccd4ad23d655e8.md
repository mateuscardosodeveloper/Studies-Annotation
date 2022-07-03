# SNS (Simple Notification Service)

- Como enviar 1 mensagem para vários destinatários?
- Funciona no sistema Publisher / Topic / Subscriber

![Fluxo do servico **SNS**](SNS%20(Simple%20Notification%20Service)%209b81385c964c4a88bdccd4ad23d655e8/Screenshot_from_2022-06-19_14-03-46.png)

Fluxo do servico **SNS**

- Producer somente envia mensagem para 1 SNS Topic
- Pode ter até 10 milhões de Subscribers por Topic
- Os Subscrivers irão receber todas as mensagens enviadas (atenção para o novo recurso: filtrar mensagens)
- Limite de 100 mil Topics
- Subscribers pode ser:
    - SQS
    - HTTP, HTTPS
    - Lambda
    - Emails
    - SMS messages
    - Mobile Notifications

### Integração com vários serviços AWS

- Alguns serviços podem enviar dados diretamente para SNS
- CloudWatch (para alarmes)
- Notificação para Auto Scaling Groups
- Amazon S3 (bucket events)
- CloudFormation (Ex. Falhas no build)
- E muitos outros

### Como Publicar

- Topic Publish (com seu servidor AWS, usando SDK)
    - Criar Topic
    - Criar Subscription
    - Publish o Topic
- Direct Publish (para mobile apps SDK)
    - Criar a plataforma da aplicação
    - Criar a plataforma Endpoint
    - Publicar para a plataforma Endpoint
    - Funciona com Google GCM, Apple APNS, Amazon ADM e outros

### SNS + SQS - Fan Out

- Publique uma vez usando SNS, receba em varios SQS
- Totalmente Dissociado
- Sem perda de dados
- Possibilidade de adicionar Subscriber a qualquer momento
- SQS permite processamento retardado
- SQS permite Retries of Work
- Pode ter vários Wokers em um Queue e 1 só Woker no outro Queue

![Screenshot from 2022-06-19 14-38-49.png](SNS%20(Simple%20Notification%20Service)%209b81385c964c4a88bdccd4ad23d655e8/Screenshot_from_2022-06-19_14-38-49.png)