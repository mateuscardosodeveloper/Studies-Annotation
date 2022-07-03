# SAM (Serverless Application Model)

- Framework para desenvolver e publicar aplicações serverless
- Configuração no formato YAML
- Gera complexos CloudFormation de um simples arquivo SAM YAML
- Suporta qualquer coisa no CloudFormation: Outputs, Mapping, Parameters, Resourcers…
- Somente 2 comandos para publicar no AWS
- SAM pode usar CodeDeploy para deploy Lambda functions
- SAM pode ajudar você a rodar Lambda, API Gateway, DynamoDB localmente

- Transform Header indica que é um template SAM:
    - Transform: ‘AWS::Serverless-2016-10-31’
- Write Code:
    - AWS::Serverless::Function              (Lambda)
    - AWS::Serverless::Api                      (API Gateway)
    - AWS::Serverless::SimpleTable        (DynamoDB)
- Package & Deploy:
    - aws cloudformation package / sam package
    - aws cloudformation deploy / sam deploy

![Como functiona o **SAM**](SAM%20(Serverless%20Application%20Model)%2049ab7f89a8cd4e1aac296ce6b938adf3/Screenshot_from_2022-06-29_00-03-53.png)

Como functiona o **SAM**