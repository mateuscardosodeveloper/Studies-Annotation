# CodeBuild

- Build Service gerenciado pela AWS
- Alternativa para usar outra ferramenta build, tal como Jenkins
- Continuous Scaling (Sem servidor para gerenciar ou provisionar)
- Cobrado somente pelo tempo que demora para completar o build
- Utilizar Docker nos bastidores
- Possibilidade de estender a capacidade usando nossa própria base Docker
- Segurança: Integração com KMS para criptografar os artifacts, IAM para Build, VPC para segurança de rede, CloudTrail para API calls logging
- Source Code pode ser do GitHub, Codecommit ou S3
- Instruções para o Build pode ser definidas no arquivo buildspec.yml. (questão bastante cobrada)
- Logs pode ser enviados para Amazon  S3 e AWS CloudWatch Logs
- Metrics para monitorar estatíscas do CodeBuild
- Use CloudWatch Alarm para detectar falhas no Build e enviar notificações
- Notificações SNS
- Possibilidade de reproduzir CodeBuild localmente para troubleshoot em caso de erro
- Pipeline pode ser defenido dentro do CodePipeline ou dentro do próprio CodeBuild

### Linguagens Suportadas

- Java
- Ruby
- Python
- Go
- Node.js
- Android
- .NET Core
- PHP
- Docker: para usar qualquer ambiente que você quiser

![Fluxo de funcionamento do **CodeBuild**](CodeBuild%204e1dca473eb346efb2e65e83a85fa133/Screenshot_from_2022-06-02_14-49-52.png)

Fluxo de funcionamento do **CodeBuild**

### BuildSpec

- Arquivo buildspec.yml deve estar no root do seu código
- Define as Environment Variables
    - Plaintext variables
    - Secure Secrets: Utiliza SSM Parameter Store
- Phases (define os commandos a serem executados)
    - Install: instala depdencias que você vai precisar no seu Build
    - Pre Build: Comando final para ser executado antes do Build
    - Build: comando para construir o Build
    - Post Build: Etapa Final (exemplo: criar arquivo zip)
- Artifacts: O que vai ser atualizado no S3 (criptografado pelo KMS)
- Cache: Arquivos para por no Cache para acelerar Builds futuros

### Local Build

- Em caso de precisar ir mais fundo para identificar problemas onde os logs não são suficientes
- É possível rodar CodeBuild Localmente no seu Desktop (depois de instalar o Docker)
- Utilizar CodeBuild Agent