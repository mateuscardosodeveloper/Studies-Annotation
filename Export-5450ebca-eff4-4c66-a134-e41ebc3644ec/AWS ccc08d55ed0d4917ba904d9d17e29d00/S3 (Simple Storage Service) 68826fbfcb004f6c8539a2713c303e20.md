# S3 (Simple Storage Service)

- S3 permite armazenar objetos (arquivos) in “buckets” (diretórios/pastas)
- Buckets precisam ter nome globalmente únicos
- Buckets são definidos a nível de Region
- Como nomear um Bucket:
    - Não é permitido letra maiúscula
    - Não é permitido underscore “_”
    - Tamanho de 3 a 63 caracteres
    - Deve começar com Letra minúscula ou número
- Não existe o conceito de pasta dentro do bucket
- Característica dos objetos:
    - Tamanho máximo é 5TB
    - Se for fazer upload de arquivo maior que 5GB deve usar “mult-part upload”

### Versioning

- É possível habilitar version para arquivos no s3
- É habilitado no nível Bucket
- Version: 1, 2, 3…
- Vantagens de utilizar version:
    - Protege contra apagamento acidental
    - Fácil de restaurar versões anteriores
- Se version não estiver habilitado o arquivo tera version = “null”

### Criptografia - (Questão bastante cobrada para certificação AWS desenvolvedor)

- Existem 4 maneiras de criptografar objetos no s3:
    - SSE-S3: Criptografa objetos usuando Key gerenciada pela AWS
    - SSE-KMS: Key gerenciada pela Key Manager Service
    - SSE-C: Usuário gerencia a própria chave (Key)
    - Client Side Encryption: Criptografia no Lado do Cliente
- É importante saber qual método aplicar para cada situação

### SSE-S3

- SSE-S3: Criptografa objetos usuando Key gerenciada pela AWS
- Objeto é criptografado no lado Servidor
- Criptografia tipo AES-256
- Header deve ser “x-amz-server-side-encryption”:”AES256”

![Diagrama do fluxo da criptografia **SSE-S3**](S3%20(Simple%20Storage%20Service)%2068826fbfcb004f6c8539a2713c303e20/Screenshot_from_2022-05-28_11-30-51.png)

Diagrama do fluxo da criptografia **SSE-S3**

### SSE-KMS

- SSE-KMS: Key gerenciada pela Key Manager Service
- Vantagens de usar KMS: Controlado pelo usuário + rastreamento para auditoria
- Objeto criptografado no lado servidor
- Header deve ser “x-amz-server-side-encryption”:”aws:kms”

![Diagrama do fluxo da criptografia **SSE-KMS**](S3%20(Simple%20Storage%20Service)%2068826fbfcb004f6c8539a2713c303e20/Screenshot_from_2022-05-28_11-34-14.png)

Diagrama do fluxo da criptografia **SSE-KMS**

### SSE-C

- SSE-C: Usuário gerencia a própria chave (Key)
- S3 não armazena Encription Key fornecida pelo cliente
- É obrigatório o uso de HTTPS
- Encryption Key deve estar presente no header da requisição

![Diagrama do fluxo da criptografia **SSE-C**](S3%20(Simple%20Storage%20Service)%2068826fbfcb004f6c8539a2713c303e20/Screenshot_from_2022-05-28_11-38-20.png)

Diagrama do fluxo da criptografia **SSE-C**

### Client Side Encryption

- Biblioteca como por exeplo Amazon S3 Encryption Cliente
- Cliente deve criptografar os dados antes de enviar para o S3
- Cliente deve descriptografar os dados depois de baixar do S3
- Cliente deve gerenciar keys e ciclo da criptografia

![Diagrama do fluxo da criptografia **Client Side Encryption**](S3%20(Simple%20Storage%20Service)%2068826fbfcb004f6c8539a2713c303e20/Screenshot_from_2022-05-28_11-43-29.png)

Diagrama do fluxo da criptografia **Client Side Encryption**

### Segurança

- Usuário:
    - IAM policies - Qual API é permitida para cada usuário
- Recursos:
    - Bucket Policies - Regras para o bucket - Permite cross account
    - Objet Access Control List (ACL) - permite controle mais granular
    - Bucket Access Control List (ACL) - menos comun
- JSON polices:
    - Resource: Bucket and Objects
    - Action: Allow ou Deny API
    - Effect: Allow ou Deny
    - Principal: Conta ou usuário para aplica a policy
- Use Bucket policy para:
    - Permitir acesso público ao bucket
    - Forçar objetos para serem criptografados ao se fazer upload
    - Permitir acesso de outras contas (cross account)
- Networking:
    - Suporta VPC endpoints (acesso a recursos utilizando endereço AWS)
- Logging e Auditoria:
    - Acesso ao S3 pode ser armazenado em outro bucket
    - API calls pode ser registrados no AWS Cloud Trail
- User Security (Segurança nível usuário)
    - MFA (multi factor authentication) pode ser requerido em Buckets com Versioned habilitado, para deletar objetos
    - Signed URLs: URLs que são válidas somente por um período

### Websites

- S3 pode ser usado para hospedar  **websites estáticos**
- Se você receber o status code 403 (forbidden), verificar se seu bucket está com acesso publico permetido.

### CORS (Cross Resource Sharing) - Pergunta frequente do exame.

- Se for solicitar dados de outro Bucket, é necessário hibilitar CORS
- CORS permite limitar o número de websites que podem requisitar arquivos do S3 Bucket (usado para limitar custo)

![Como o cors funciona para permitir que um website acesse objetos de um bucket.](S3%20(Simple%20Storage%20Service)%2068826fbfcb004f6c8539a2713c303e20/Screenshot_from_2022-05-28_12-24-53.png)

Como o cors funciona para permitir que um website acesse objetos de um bucket.

### Consistency Mode

- Ler depois de escrever para PUTS de objetos novos
    - Assim que o objeto é escrito ele pode ser lido no Bucket
    - Isso é verdade, com exceção se foi feito um GET antes para ver se o objeto existe, caso você inseriu o objeto e fizer a leitura para verificar se o objeto existe AWS ira retornar que o objeto não existe, porque ele retorna o cache da memória que foi feito do primeiro GET anterior (isso também se aplica quando é feito atualização ou deleção do objeto). A informação do cache em memória fica armazenad por um tempo, até ser atualizada novamente.

### Extras:

- Quando a pasta **.aws** está configurada com as credenciais do IAM do usuário, ele tem acesso aos serviços da aws via linha de comando (cli), porém ele tem somente acesso as permissões da quele usuário que foi configurado as credencias.
- Alguns comandos do aws cli para o s3.

```bash
aws s3 cp <diretório bucket> <diretório maquina local> # download arquivo s3
aws s3 cp <diretório maquina local> <diretório bucket> # upload arquivo para s3
aws s3 rm <diretório do bucket> # deleta um arquivo.
aws s3 rb <bucket> # deleta um bucket
aws s3 mb s3://<name bucket> # para criar um novo bucket.
```