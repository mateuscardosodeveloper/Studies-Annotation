# KMS (Key Management Service)

- KMS é a forma mais popular de criptografar dados na AWS
- Forma fácil de controlar acesso aos dados.
- Keys gerenciadas pelas AWS
- Totalmente integrado com Autorização IAM
- Funciona nos bastidores para os seguintes serviços AWS
    - Amazon EBS: Criptografa volumes
    - Amazon S3: Criptografa do lado Servidor para objetos
    - Amazon Redshift: Criptografia de dados
    - Amazon RDS: Criptografia de dados
    - Amazon SSM: Parameter store
    - É possivel utilizar KMS via CLI e SDK
- Sempre que quiser compatilhar informações sensíveis use KMS
    - Senha de Banco de Dados
    - Credenciais para serviços externos
    - Private Key de certificados SSL
- No KMS, CMK é usado para criptografar dados e nunca pode ser lida pelo usuário, CMK pode ser alternada para aumentar a segurança
- Nunca coloque suas credenciais em texto puro, especialmente no seu código
- KMS pode criptografar no máximo 4KB por chamada
- Se os dados forem maiores que 4KB, use Envelope Encryption
- Para dar acesso ao KMS para outra pessoa:
    - Key Policy deve permitir acesso do usuário
    - IAM Policy deve permitir chamadas a API
- Gerencia Keys e Polices
    - Create
    - Rotation Polices
    - Disable
    - Enable
- Possibilidade de auditar o uso da Key usando CloudTrail
- Existem 3 tipos de Customer Master Key (CMK):
    - AWS Managed Service Default CMK: Grátis
    - User Key Criada no KMS: $1 por mês
    - User Key importada (deve ser 256-bit symmetric key): $1 por mês
    - + Custo por API call no KMS: $0.03 / 10000calls

![Fluxo de funcionamento da criptografia **KMS**](KMS%20(Key%20Management%20Service)%20c259526a0a9c41dca1c76b694660f3d6/Screenshot_from_2022-07-02_16-14-10.png)

Fluxo de funcionamento da criptografia **KMS**

### Encryption SDK

- Como fazer se você precisar criptografar mais de 4KB usando KMS?
- Para isso, precisamos usar Envelope Encryption
- Envelope Encryption dá um pouco de trabalho para implementar
- AWS Encryption SDK facilita o uso do Envelope Encryption
- É diferente do S3 Encryption SDK
- Encryption SDK também existe como ferramenta CLI que nós podemos instalar

- Para a prova:
    - Tudo que iver mais de 4KB deve ser criptografado usando
    
           Encryption SDK = Envelope Encryption = GerenerateDataKey API
    

![Criptografa **Envolope Encryption**](KMS%20(Key%20Management%20Service)%20c259526a0a9c41dca1c76b694660f3d6/Screenshot_from_2022-07-02_16-41-59.png)

Criptografa **Envolope Encryption**

![Descriptografa **Envolope Encryption**](KMS%20(Key%20Management%20Service)%20c259526a0a9c41dca1c76b694660f3d6/Screenshot_from_2022-07-02_16-45-35.png)

Descriptografa **Envolope Encryption**

### Parameter Store

- Armazenamento Seguro para configuração e Secrets
- Criptografia usando KMS (opcional)
- Serverless, escalável, durável, SDK fácil de usar, grátis
- Rastreamento das versões de configuração e Secrets
- Gerenciamento de configuração usando Path e IAM
- Notificações com CloudWatch Events
- Integração com CloudFormation

![Screenshot from 2022-07-02 16-56-49.png](KMS%20(Key%20Management%20Service)%20c259526a0a9c41dca1c76b694660f3d6/Screenshot_from_2022-07-02_16-56-49.png)

![Hierarquia **Parameter Store**](KMS%20(Key%20Management%20Service)%20c259526a0a9c41dca1c76b694660f3d6/Screenshot_from_2022-07-02_17-02-03.png)

Hierarquia **Parameter Store**