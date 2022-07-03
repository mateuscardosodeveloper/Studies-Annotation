# API Gateway

![Application Programming Interface](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-27_22-44-48.png)

Application Programming Interface

- AWS Lambda + API Gateway: Sem infraestrutura para gerenciar
- Gerencia Versões da API (v1, v2, v3)
- Gerencia diferentes Environmets (dev, test, prod…)
- Gerencia Segurança (Autenticação e Autorização)
- Cria API Keys, gerencia Request Throttling
- Importação Swagger / Open API para definição rápida de APIs
- Transforma e Valida Requests e Responses
- Gera SDK e especificação da API
- Cache API Response

### Integração

- Fora do VPC
    - AWS Lambda (mais comum)
    - Endpoint na EC2
    - Load Balancers
    - Qualquer outro Serviço AWS
    - Acesso público através de HTTP
- Dentro do VPC
    - AWS Lambda no VPC
    - EC2 endpoint no VPC

### Deployment Stages

- Alterar um API e salvar não siginifica que as alterações entrarão em vigor
- É preciso Deploy para as alterações terem efeito
- Isso pode causar alguma confusão
- Mudanças são Deployed para Stages (quantas você quiser)
- Exemplos de nomes para Stages (dev, test, prod)
- Cada Stage tem sua própria configuração
- É possível usar Rollback nos Stages

### Stage Variables

- Stage variables são como Environment Variables para API Gateway
- Use para mudanças frequentes na configuração do API
- Pode ser usados para:
    - Lambda Function ARN
    - HTTP endpoint
    - Parameter Mapping Templates
- Exemplo
    - Configure HTTP Endpoint
    - Passar configuração de parametros para AWS Lambda através de mapping templates
    - Stage Variables são passados para o objeto “context” na Lambda Function

### Stage Variables e Lambda Aliases

- Criamos um Stage Variable para indicar o correspondente Lambda Alias
- Nosso API gateway irá automaticamente invocar a correta Lambda Function

![Screenshot from 2022-06-27 23-08-28.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-27_23-08-28.png)

### Canary Deploymeny

- Possível habilitar Canary Deployment para qualquer Stage (Normalmente prod)
- Escolha o percentual de tráfego para cada API

 

![Screenshot from 2022-06-27 23-10-44.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-27_23-10-44.png)

### Mapping Template

- Mapping Template pode ser usado para modificar Request / Response
- Renomear parametros
- Modificar body content
- Adicionar Headers
- Map JSON para XML para enviar para o backend ou para o cliente
- Use Velocity Template Language (VTL): para loop, if e etc
- Filtra resultado da sáida (se necessário remove dados desnecessários)

### Swagger / Open API spec

- Forma comum de definir REST API, usando definição da API com código
- Importa Swagger / OpenAPI 3.0 spec para API Gateway
    - Method
    - Method Request
    - Integration Request
    - Method Response
    - + AWS extensions para API Gateway e configuração de cada opção
- Pode exportar API como Swagger / OpenAPI spec
- Usando Swagger podemos gerar SDK para nossa aplicação

### Caching API Response

- Caching reduz o número de chamadas feitas para o backend
- Default time TTL (time to live) é 300 segundos (mínimo 0s, máximo 3600s)
- Cache são definidos por Stage
- Opção de criptografia
- Capacidade do cache entre 0.5GB e 237GB
- Possibilidade de alterar a configuração do cache para um método específico
- Habilidade de limpar o cache inteiro imediatamente
- Clientes pode invalidar o cache com o Header: CacheControl:max-age=0 (com a correta autorização IAM)

![Screenshot from 2022-06-28 22-46-46.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-28_22-46-46.png)

### Logging, Monitoring, Tracing

- CloudWatch Logs:
    - Habilita CloudWatch logging no nível Stage (com Log Level)
    - Capacidade de alterar as configurações por API (Ex. ERROR, DEBUG, INFO)
    - Log contém informação sobre Request, Response Body
- CloudWatch Metrics:
    - Por Stage
    - Possibilidade de habilitar Detailed Metrics
- X-Ray:
    - Habilitar Tracing para pegar informação extra sobre Requests no API Gateway
    - X-Ray API Gateway + AWS Lambda oferecem uma solução completa

### CORS (Cross-Origin Resource Sharing)

- CORS deve ser habilitado quando API recebe chamadas de outro domínio
- OPTIONS Pre-Flight Request deve conter os seguintes headers:
    - Access-Control-Allow-Methods
    - Access-Control-Allow-Headers
    - Access-Control-Allow-Origin
- CORS pode ser habilitado pelo console AWS

### Usage Plans e API Keys

- Possibilidade de limitar o uso da API
- Usage Plans:
    - Throttling: Configura a capacidade normal e Burst
    - Quotas: número de request por dia / semana / mês
    - Associado com API Stages
- API Keys:
    - Gera uma por cliente
    - Associado com Usage Plans
- Possibilidade de rastrear o uso da API Key

### Segurança

- Permissão IAM
    - Criar IAM Policy com a devida autorização e anexar ao User / Role
    - API Gateway verifica permisão IAM passada durante a chamada
    - Ideal para fornecer acesso dentro da infraestrutura AWS
    - Utiliza a capacidade “Sig v4” onde as credenciais IAM estão no Header

![Screenshot from 2022-06-28 23-19-53.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-28_23-19-53.png)

- Lambda Authorizer ou Custom Authorizers
- Usa AWS Lambda para validar o token no heaer que está sendo passado
- Opção para Cache do resultado da autenticação
- Lambda deve retornar uma IAM Policy para o usuário

![Screenshot from 2022-06-28 23-23-07.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-28_23-23-07.png)

- Cognito User Pools
    - Cognito gerencia User Lifecycle
    - API gateway verifica a identidade automaticamente do AWS Cognito
    - Não é necessário nenhuma implementação
    - Cognito só trabalha na autenticação, não na autorização

![Screenshot from 2022-06-28 23-25-31.png](API%20Gateway%20b8a978cfa59d4f78a430926ff46ca9f2/Screenshot_from_2022-06-28_23-25-31.png)

### Segurança Resumo

- IAM:
    - Ideal para users/roles que já existem na sua conta AWS
    - Gerencia autenticação e autorização
    - Utiliza Sig v4
- Custom Authorizer:
    - Ideal para tokens de aplicações de terceiros
    - Flexível sobre o que um IAM Policy retorna
    - Gerencia autenticação e autorização
    - Cobrado por chamada a Lambda Function
- Cognito User Pool
    - Gerenciado por você (pode usar Facebook, Google, Amazon)
    - Sem necessidade de escrever código
    - Deve-se implementar autorização no Backend