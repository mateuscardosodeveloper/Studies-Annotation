# ECR (Elastic Container Registry)

- Até agora nós usamos Docker Image do Docker Hub (público)
- ECR é um respositório privado
- Acesso é controlado através de IAM (Policy)
- É necessário executar alguns comandos para Push e Pull
    - $(aws ecr get-login —no-include-email —region eu-east-1)
    - docker push 123456789.dkr.ecr.us-east-1.amazonaws.com/imagename:latest
    - docker pull 123456789.dkr.ecr.us-east-1.amazonaws.com/imagename:latest