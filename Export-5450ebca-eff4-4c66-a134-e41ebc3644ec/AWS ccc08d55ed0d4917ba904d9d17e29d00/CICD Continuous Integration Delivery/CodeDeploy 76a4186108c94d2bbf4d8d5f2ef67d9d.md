# CodeDeploy

- Deploy aplicação automaticamente para várias instância EC2
- Essas instâncias não são gerenciadas pelo BeanStalk
- Existem muitas formas de lidar com deployments usando ferramentas opensource (Ansible, Terraform, Chef, Puppet, etc…)
- Podemos utilizar AWS CodeDeploy que é gerenciado pela AWS
- Instância EC2 são agrupadas pelo Deployment Group (dev / test / prod)
- Várias opções para definir o tipo de deployment
- CodeDeploy pode ser instegrado com CodePipeline e usar Artifacts
- CodeDeploy pode reusar ferramentas do configuração, funciona com qualquer aplicação, Integração com AutoScaling
- Blue/Green deployment só funciona com EC2. Não funciona com On Premise
- Suporta AWS Lambda Deployment

### Passo a Passo

- Instância EC2 ou Máquinas On Premise deve ter CodeDeploy Agent Instalado
- Agent monitora continuamente AWS CodeDeploy
- CodeDeploy envia appspec.yml
- Aplicação é baixada do GitHub ou S3
- EC2 executará as instruções recebidas
- CodeDeploy Agent reportará sucesso ou falha no deployment

### Componentes

- Application: nome único
- Compute Platform: EC2/On-premise ou Lambda
- Deployment Configuration: Regras do Deployment para Sucesso ou Falha
    - EC2/On-Premise: Pode-se especificar o número mínimo de EC2 saudável para deployment
    - AWS Lambda: Pode-se especificar como o tráfego será roteado para atualizar o Lambda Function
- Deployment Group: grupo de instâncias tagueada (TAGs)
- Deployment Type: In-Place ou Blue/Green
- Application Revision: Código da aplicação + appspec.yml
- Service Role: Role para CodeDeploy realizar as tarefas necessárias
- Target Revision: Destino da Versão da Aplicação

### AppSpec

- File Section: Como copiar os arquivos do Github / S3 para o Destino
- Hooks: Conjunto de instruçõe para Deploy nova versão (hooks podem ter timeouts)
- Em ordem:
    - ApplicationStop
    - DownloadBundle
    - BeforeInstall
    - Install
    - AfterInstall
    - ApplicationStart
    - ValidateService

### Deployment Config

- Configs:
    - One at time: uma instância de cada vez. Se uma falhar o deployment para
    - Half at time: metade, 50%
    - All at Once: Rápido, Downtime, usado em desenvolvimento
    - Custom: personalizado
- Failures:
    - Instâncias ficam in “Failed State”
    - Novos deployments começam primeiro pelas instâncias em “Failed State”
    - Rollback: Deploy o código anterior ou habilita Rollback automático quando falhar ocorrer
- Deployment Targets:
    - Grupo de Instâncias EC2 com Tags
    - Direcionado para um ASG
    - Mix of ASG/Tags para criar deployment segments
    - Personalização de scripts com DEPLOYMENT_GROUP NAME Environment Variables

### Blue Green Deployment

![Fluxo do Deploy **Blue Green**](CodeDeploy%2076a4186108c94d2bbf4d8d5f2ef67d9d/Screenshot_from_2022-06-02_17-02-43.png)

Fluxo do Deploy **Blue Green**