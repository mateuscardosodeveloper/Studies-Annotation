# Kinesis

- Kinesis é uma alternativa ao Apache Kafka
- Ideial para Log de Aplicação, Metrics, IoT, clickstream
- Ideal para Real-Time Big Data
- Ideial para Streaming Processing Frameworks (Spark, NiFi…)
- Dados são automaticamente replicados para 3 AZs

- Kinesis Streams: baixa latência escalonável
- Kinesis Analytics: para análise em tempo real de streams usando SQL
- Kinesis Firehose: Carrega o stream no S3, Redshift, ElasticSearch…

![Fluxo de funcionamento do **Kinesis**](Kinesis%201fd4297b9b2049968e9ae9cb6bb3c9cb/Screenshot_from_2022-06-19_15-11-13.png)

Fluxo de funcionamento do **Kinesis**

### Stream

- Stream são divididos em Shards / Partitions
- Retenção dos dados é de 1 dia mas pode ser estendido até 7 dias
- Habilidade de reprocessar / replay data
- Multiplas aplicações podem acessar o mesmo stream
- Processamento em tempo real e throughput escalonável
- Uma vez que os dados são inseridos no Kinesis, eles não pode ser apagados (immutability)

![Diagrama de funcionamento do **Kinesis Stream**](Kinesis%201fd4297b9b2049968e9ae9cb6bb3c9cb/Screenshot_from_2022-06-19_15-16-27.png)

Diagrama de funcionamento do **Kinesis Stream**

### Stream Shards

- 1 Stream é constituído por 1 ou mais Shards
- Capacidade por Shard
    - 1MB/s ou 1000 messages/s para escrita
    - 2MB/s para leitura
- Cobrado por Shard provisionado. Pode ter quantos Shards forem necessários
- Bathing ou por mensagem
- Número de Shards pode ser ajustável conforme necessidade
- Records são ordenados por Shard

### API - Put Records

- PutRecord API + Partion key é hashed para determinar Shard ID
- Mesma Key vai para a mesma partition (Ajuda a ordenar as Keys
- Mensagens enviadas recebem um número sequencial
- Escolha um Partition Key que seja distribuída (Previnir “hot partition”)
- Use user_id se tiver vários usuários
- Não use Estado_id se 90% dos usuários são do mesmo Estado

![Screenshot from 2022-06-19 15-36-57.png](Kinesis%201fd4297b9b2049968e9ae9cb6bb3c9cb/Screenshot_from_2022-06-19_15-36-57.png)

- Use batching com PutRecords para reduzir custo e aumentar throughput
- ProvisionedThroughputExceeded se consumir acima do limite
- Podemos usar CLI, AWS, SDK, ou Producer Libraries de vários frameworks

### Exceptions

- ProvisionedThroughputExceeded Exceptions
- Ocorre quando é enviado mais dados do que o Shard suporta
- Verifique se Partition Key é distribuída de forma inteligente
- Solução:
    - Use Retris with backoff
    - Aumente o número de Shards
    - Escolha Partition Key de forma inteligente

### API - Consumers

- Pode ser um consumidor normal (CLI, SDK e etc)
- Pode usar Kenesis Client Library (Java, Node Python, Ruby, .NET)
    - KCL usa DynamoDB para fazer checkpoint offsets
    - KCL usa DynamoDB para rastrear outros Workers e dividir o trabalho entre os Shards

![Screenshot from 2022-06-19 15-46-53.png](Kinesis%201fd4297b9b2049968e9ae9cb6bb3c9cb/Screenshot_from_2022-06-19_15-46-53.png)

### Security

- Controle de Acesso / Autorização usando IAM Polices
- Criptografia em trânsito usando HTTPS endpoints
- Criptografia em repouso usando KMS
- Possibilidade de Criptografar/Decriptografar dados usando Client Side (mais complicado)
- VPC Endpoints disponível para Kinesis para acesso dentro do VPC

### Data Analytics

- Análise em tempo real no Kinesis Streams usando SQL
- Kinesis Data Analytics:
    - Auto Scaling
    - Managed: Não é necessário provisionar servidores
    - Continuous: Tempo Real
- Cobrado somente pelo que é usado
- Pode criar Streams a partir de queries tempo real

### Firehose

- Gerenciado pela AWS
- Quase Real Time (60 ms latency)
- Carrega dados dentro do Redshift, Amazon S3, ElasticSearch Splunk
- Escalonamento automático
- Suporta vários formatos de dados (cobrado por conversão)
- Cobrado pela quantidade de dados que passam pelo Firehose