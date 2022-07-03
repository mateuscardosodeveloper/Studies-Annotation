# DynamoDB

- Gerenciado pela AWS, Alta disponibilidade com réplicas em 3 AZ
- Banco de Dados NoSQL
- Escalonável para quantidade muito grande de dados, banco de dados distrubuído
- Milhões de requisições por segundo, trilhões de linhas, centenas de Terabytes de armazenamento
- Rápido e consistente em performance (Baixa latência)
- Integrado com IAM para segurança, autorização e administração
- Event Driven Programming com DynamoDB Streams
- Baixo Custo
- Auto Scaling
- DynamoDB é feito de tabelas
- Primary key (Hash) é obrigatório e deve ser definido no momento que criamos a tabela
- Tabela pode ter um número infinito de itens (rows)
- Cada item tem Attributes (Pode ser adicionado a qualquer moment, pode ser nulo)
- Tamanho máximo do Item é 400KB
- Tipo de dados suportados:
    - Scalar Types: String, Number, Binary, Boolean, Null
    - Document Type: List, Map
    - Set Types: String Set, Number Set, Binary Set

### Primery Key

- Opção 1: Somente Partition Key (Hash)
- Partition Key deve ser única para cada item
- Partition Key deve ser diversificada para os dados serem distribuídos
- Example: user_id

![Screenshot from 2022-06-23 22-09-53.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-23_22-09-53.png)

- Opção 2: Partition Key + Sort Key (Range)
- A combinação deve ser única
- Dados são agrupados pela Partition Key
- Sort Key == Range Key
- Exemplo:

![Screenshot from 2022-06-23 22-06-59.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-23_22-06-59.png)

### Provisioned Throughput

- Tabela deve ter leitura e escrita provisionadas
- Read Capacity Units (RCU)
- Write Capacity Units (WCU)
- Opção de configurar auto-scaling para throughput
- Throughput pode temporariamente exceder o limite estabelecido usando “burst credit”
- Se o burst credit acabar, você receberá um “ProvisionedThroughputException”
- É Recomendado fazer um Exponential Back-Off Retry
- Novo Recurso: DynamoDB Read/Write capacity mode: On-demand

### Write Capacity Unit (WCU)

- 1 Write Capacity Unit (WCU) representa uma escrita por segundo para 1 item até 1KB em tamanho
- Se o item for maior que 1KB, mais WCU serão consumidos

- Exemplo 1: escrever 10 itens de 2KB cada por segundo (10 * 2 = 20 WCU)
- Exemplo 2: escrever 6 itens de 4.1KB cada por segundo (6 * 5 = 30 WCU)
- Exemplo3: escrever 120 itens de 2KB cada por **minuto** (120 * 2 / 60 = 4 WCU)

### Strongly Coonsistent Read vs Eventually Consistent Read

- Eventually Consistent Read: Se lermos um item logo após uma escrita, é possível lermos um valor inesperado devido a replicação
- Strongly Consistent Read: Se lermos um item de uma escrita, leremos o valor correto
- Por padrão (default), DynamoDB usa Eventually Consistent Reads, porém GetItem, Query e Scan tem um parametro chamado ConsistentRead que pode ser modificado para True

![Screenshot from 2022-06-23 22-36-15.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-23_22-36-15.png)

### Read Capacity Units (RCU)

- 1 Read Capacity Unit (RCU) representa 1 strongly consistent read por segundo ou 2 eventually consistent reads por segundo, para 1 item até 4KB
- Se o item for maior que 4KB, mais RCU serão consumidos

- Exemplo 1: 10 Strongly Consistent Read por segundo de 4KB cada
    - 10 * 4KB / 4KB = 10 RCU
- Exemplo 2: 16 Eventually Consistent Read por segundo de 12KB cada
    - (16 / 2) * (12 / 4) = 24 RCU
- Exemplo 3: 10 Strongly Consistent Read por segundo de 6KB cada
    - 10 * 8KB / 4 = 20 RCU (arrendondamos 6KB para 8KB)

### Throttling

- Se excedermos RCU ou WCU recebemos um ProvisionedThroughputException
- Razão:
    - Hot Key: Partition Key sendo acessado muitas vezes
    - Hot Partitions:
    - Itens muito grandes: RCU e WCU dependem do tamanho do Item
- Soluções:
    - Exponential Back-Off quando uma exceção é encontrada (recurso incluso no SDK)
    - Distribuir Partition Keys o máximo possível
    - Se for problema com RCU, podemos usar DynamoDB Accelerator (DAX)

### LSI (Local Secondary Index)

- Key Alternativo para tabela, local ao hash key
- Máximo de 5 Local Secondary Indexs por tabela
- Sort Key consiste de exatamente um Atributo
- O atributo que você escolher deve ser String, Number ou Binary
- LSI deve ser definido no momento da criação da tabela

![Screenshot from 2022-06-25 12-29-51.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-25_12-29-51.png)

### GSI (Global Secundary Index)

- Para aumentar a velocidade dos Queries para não Key atributos, use Global Secondary Index
- GSI = Partition Key + Sort Key (opcional)
- Index é uma “nova” tabela e podemos projetar atributos a ela
    - A Partition Key e Sort Key da tabela original são sempre projetada (KEYS_ONLY)
    - Pode-se especificar atributos para projetar (INCLUDE)
    - Pode-se usar todos os atributos da tabela principal (ALL)
- Deve-se definir RCU / WCU para índice
- Possibilidade de adicionar / modificar GSI (not LSI)

![Screenshot from 2022-06-25 12-38-36.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-25_12-38-36.png)

### Concurrency

- DynamoDB tem uma função chamada “Conditional Update / Delete”
- Isso garante que o item não foi modificado antes de ser alterado
- Isso faz DynamoDB Optimistic Locking / Concurrency Database

![Screenshot from 2022-06-25 12-49-15.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-25_12-49-15.png)

### DAX (DynamodDB Accelerator)

- Não é necessário nenhuma alteração na aplicação
- Escrita vai para o DAX e DAX escreve no DynamoDB
- Latência de Mocrisegndos para leituras e queries no Cache
- Resolve o problema de Hot Key (muitas leituras na mesma partição)
- 5 minutos TTL (Cache) padrão
- Até 10 nodes no cluster
- Multi AZ (mínimo de nodes recomendado para produção)
- Segurança (Criptografia em repouso com KMS, VPC, IAM, CloudTrail…)

![Screenshot from 2022-06-25 12-53-30.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-25_12-53-30.png)

### Streams

- Mudança no DynamoDB (Create, Update, Delete) pode iniciar um DynamoDB Stream
- Esse Stream pode ser lido pela AWS Lambda para:
    - Reagir a mundaças em tempo real (enviar um email de boas vindas, entre outras coisas.)
    - Análise
    - Criar derivative tables / views
    - Insert no ElasticSearch
- Possível implementar Cross Region Replication usando Streams
- Strem tem 24 horas de retenção
- Pode configurar qual tipo de Event Stream de seja receber

![Screenshot from 2022-06-25 13-07-42.png](DynamoDB%20885c334db85c4dab897af8a9ebfa87e0/Screenshot_from_2022-06-25_13-07-42.png)

### Segurança e Outros Recursos

- Security:
    - VPC Endpoint disponível para acessar DynamoDB sem Internet
    - Acesso totalmente controlado pelo IAM
    - Criptografia em repouso usando KMS
    - Criptografia em transito usando SSL / TLS
- Backup e Restore
    - Restauração Point in time (igual RDS)
    - Sem impacto na performace
- Global Tables
    - Multi Region, Replicável, Alta Performance
- Amazon DMS pode ser usado para migrar para DynamoDB (Mongo, Oracle, MySQL, S3 e entre outros)
- É possível instalar DynamoDB em uma máquina local para desenvolvimento