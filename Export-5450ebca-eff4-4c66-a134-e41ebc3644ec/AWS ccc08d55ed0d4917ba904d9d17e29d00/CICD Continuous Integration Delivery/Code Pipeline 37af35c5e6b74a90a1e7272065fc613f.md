# Code Pipeline

- Continous Delivery
- É possível ver o fluxo passo a passo
- Source (onde está o código): GitHub, CodeCommit, Amazon S3
- Build: CodeBuild, Jenkins
- Load Test: ferramentas de terceiros
- Deploy: AWS CodeDeploy, beanStalk, CloudFormation, ECS
- Stage:
    - Cada estágio pode ter ações executadas de forma serial ou paralela
    - Exemplos de Stages: Build, Test, Deploy, Load Test…
    - Aprovação manual pode ser habilitada para qualquer Stage (estágio)

### Artifacts

- Cada estágio do pipeline pode criar “artifacts”
- Artifacts são armazenados no S3 e repassados para o próximo estágio

![Diagrama de como funcionar os **Artifacts** do **Code Pipeline**](Code%20Pipeline%2037af35c5e6b74a90a1e7272065fc613f/Screenshot_from_2022-06-02_10-45-53.png)

Diagrama de como funcionar os **Artifacts** do **Code Pipeline**

### Troubleshooting

- Troubleshooting (Solução de Problema)
- Mudanças no CodePipeline Stage acontecem no AWS CloudWatch Events, que pode criar Notificações SNS
    - Possível criar eventos para falhas no pipeline
    - Possível criar eventos para Estágio Cancelados
- Se CodePipeline falhar em um estágio, seu pipeline para e você pode receber informações sobre o erro no console AWS
- AWS CloudTrail pode ser usado para auditar AWS API Calls
- Se Pipeline não puder executar uma tarefa, confira se IAM Service Rolle que está associado tem permissão suficientes (IAM Policy)