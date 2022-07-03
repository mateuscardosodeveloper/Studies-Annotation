# SQS (Simple Queue Service)

![Fluxo de funcionamento do serviço de mensageria do SQS.](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-16_23-07-54.png)

Fluxo de funcionamento do serviço de mensageria do SQS.

### Standard Queue

- Gerenciado pela AWS
- Escalona de 1 mensagem por segundo até 10.000 por segundo
- Default Retention of message (Armazenamento de mensagem): 4 dias (máx 14)
- Sem limite de quantas mensagens podem ficar no queue
- Latência menor que 10ms
- Escalonamento horizontal de consumidores
- Pode acontecer de ter mensagem duplicada
- Pode acontecer das mensagens serem consumidas fora de ordem (Best effort ordering)
- Tamanho máximo de cada mensagem é 256KB

### Delay Queue

- Delay a message (consumidores não veem a mensagem imediatamente) máximo até 15 min
- Default é 0 segundos (mensagem fica disponível imediatamente)
- Default pode ser ajustado no queue level
- Default pode ser alterado usando DelaySeconds Parameters

![Screenshot from 2022-06-16 23-14-41.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-16_23-14-41.png)

### Producing Messages

- Definição do Body
- Opção Delay Delivery
- Adicionar Messages Attributes (metadata é opicional)
- Retorna:
    - Message Identified
    - MD5 hash do Body

![Screenshot from 2022-06-16 23-24-26.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-16_23-24-26.png)

### Consuming Messages

- Consumers
- Poll SQS para mensagens (recebe no máximo 10 mensagens por vez)
- Processa mensagens dentro do Visibility Timeout
- Deleta as mensagens usando Message ID e Receipt Handle

![Screenshot from 2022-06-16 23-29-26.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-16_23-29-26.png)

### Visibility Timeout

- Quando o consumidor Polls a Message do Queue, a mensagem fica invisível para outros consumidores por uma período de tempo definido (Visibility Timeout)
    - Ajustável entre 0 e 12 horas (default: 30 segundos)
    - Se o tempo for muito alto (10 minutos) e o consumidor falhar ao processar a mensagem, é necessário espererar muito tempo para que a mensagem esteja disponível novamente.
    - Se o tempo for muito baixo (1 segundo) e o consumidor precisar de mais tempo para processar a mensage, outro consumidor irá receber a mesma mensagem e a mesma mensagem sera processada 2 vezes.
- ChangeMessageVisibility API muda a visibilidade enquanto processa a mensagem
- DeleteMessage API para avisar SQS que a mensagem foi processada

### Dead Letter Queue

- Se o consumidor falhar ao processar a mensagem dentro do Visibility Timeout, a mensagem volta para o Queue
- Podemos escolher quantas vezes uma mensagem pode voltar para o Queue - Redrive Policy
- Quando esse número é atingido, a mensagem vai para Dead Letter Queue (DLQ)
- Depois de criar DLQ, e apontar o SQS Queue para o Dead Letter Queue
- Processe a mensagem no DLQ antes que ela expire

![Screenshot from 2022-06-18 13-51-54.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-18_13-51-54.png)

### Long Polling

- Quando o consumidor requisitar a mensagem do Queue, tem a opção de esperar por mensagens chegarem no Queue, se o Queue estiver vazio
- Essa técnica é chamado Long Polling
- LongPolling diminui o numero de chamadas a SQS API, e aumenta a eficiência da sua aplicação
- O Wait Time pode ser ajustado entre 1 segundo e 20 segundos (recomendado para maioria dos casos)
- Long Polling pode ser habilitado no Nível Queue ou Nível API usando WaitTimeSeconds

![Screenshot from 2022-06-18 14-12-42.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-18_14-12-42.png)

### Diagrama Geral Sobre Serviço SQS

![Screenshot from 2022-06-18 14-14-51.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-18_14-14-51.png)

### FIFO Queue

- First In - First out - Primeiro a entrar é o primeiro a sair (Não disponível em todas as regiões)
- Nome do Queue deve terminar com .fifo
- Baixo throughput (Até 3,000 por segundo com Batching (300/s sem Batching)
- Mensagens são processadas em ordem pelos consumidores
- Mensagens são enviadas somente uma vez
- Sem Delay por Mensagem (somente Delay por Queue)
- Possibilidade de fazer de-duplication baseado em contéudo
- 5-minutos interval de-deplication usando “Duplication ID”
- Message Groups:
    - Possibilidade agrupar mensagens por FIFO ordering usando “Message GroupID”
    - Somente um Worker pode ser designado por Message Group para que as mensagens sejam processadas em ordem
    - Message group é somente um Tag extra na mensagem

### Extended Client

- Limite da Mensagem 256KB, Como enviar mensagem maiores?
- Usando SQS Extended Client (Java Library)

![Screenshot from 2022-06-19 13-51-51.png](SQS%20(Simple%20Queue%20Service)%20b56ce29050dc4988925cc00e755511f6/Screenshot_from_2022-06-19_13-51-51.png)

### Security

- Criptografia em trânsito usando HTTPS endpoint
- Pode usar SSE (Server Side Encryption) usando KMS
    - Pode usar CMK (Custmer Master Key)
    - Pode ajustar Data Key Reuse Period entre 1 minuto e 24 horas
        - Baixo e KMS API será usado con frequência (Custo Elevado)
        - Alto e KMS API será usado com menos frequência (Custo Baixo)
    - SSE somente criptografa o Body, não o metadata (message ID, timestamp, attributes)
- IAM policy deve permitir o uso do SQS
- SQS Queue Access Policy
    - Controlegranular sobre IP
    - Controle sobre o tempo de requisição
- Nenhum VPC endpoint deve ter acesso a internet para cessar SQS