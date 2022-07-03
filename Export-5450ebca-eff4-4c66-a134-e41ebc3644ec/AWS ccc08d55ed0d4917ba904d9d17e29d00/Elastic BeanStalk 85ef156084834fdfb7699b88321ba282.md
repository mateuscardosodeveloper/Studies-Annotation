# Elastic BeanStalk

- É voltado para o desenvolvedor distribuir sua aplicação no AWS
- Utiliza componentes como:
    - EC2
    - ASG
    - ELB
    - RDS
    - Outros
- Tudo em um só ambiente
- É possível mudar as configurações
- BeanStalk é grátis. Você paga pelos recursos que utilizar

![Diagrama de um fluxo de controle **Beanstalk**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_13-50-14.png)

Diagrama de um fluxo de controle **Beanstalk**

- Gerenciado pelo AWS
    - Configuração da Instãncia / Sistema Operacional gerenciado pelo BeanStalk
    - Deployment é configurável mas executado pelo BeanStalk
- Somente o código da aplicação é responsabilidade do desenvolvedor
- 3 tipos de arquitetura:
    - Single Instance Deployment (1 instãncia): Ideal para testes
    - LB + ASG: Ideal para pré-produção e produção de aplicações web
    - ASG only: Ideal para não web apps (serviços)
- Elastic BeanStalk tem 3 componentes:
    - Application
    - Application version: Cada deployment recebe uma versão
    - Environment name (dev, test, prod…): pode receber qualquer nome
- Application versions são implantadas para Environments
- Rollback: restaura a aplicação anterior
- Controle total do ciclo de vida (lifecyclo) dos environments

![Fluxo dos componentes **Beanstalk**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_13-58-25.png)

Fluxo dos componentes **Beanstalk**

- Aceita configurações do Docker:
    - Single Container Docker
    - Multicontainer Docker
    - Preconfigured Docker
    - Se a plataforma não for suportada, é possivel criar uma Personalizada (avançado)
- Suporta as seguintes plataformas:
    - GO
    - Java SE
    - Java com Tomcat
    - .NET (Windows Server com IIS)
    - Node.js
    - Python
    - Ruby
    - Packet Builder

### Deployment Modes

- All at once: Atualiza tudo de uma vez só. Aplicação fica indisponível durante atualização.
- Rolling: Atualiza grupo de instancias, quando finalizar o primeiro grupo de instancias são considerados Healthy. BeanStalk começa a atualizar o próximo bucket
- Rolling with Additional Batches: similar ao Rolling porém inicia novas instâncias para instalar a nova versão (a versão antiga continua dispónivel)
- Immutable: inicia novas instâncias em um novo ASG, publica a aplicação para as novas instâncias e então alterna o tráfego para as novas instâncias quando elas são consideras saudáveis (healthy)

### Deployment: All at once

![Diagrama de como funcionao deployment **All at once**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_21-20-31.png)

Diagrama de como funcionao deployment **All at once**

### Deployment: Rolling

![Diagrama de como funcionao deployment **Rolling**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_21-22-25.png)

Diagrama de como funcionao deployment **Rolling**

### Deployment: Rolling with additional batches

![Diagrama de como funcionao deployment **Rolling with additional batches**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_21-24-15.png)

Diagrama de como funcionao deployment **Rolling with additional batches**

### Deployment: Immutable

![Diagrama de como funcionao deployment **Immutable**](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_21-25-34.png)

Diagrama de como funcionao deployment **Immutable**

### Deployment: Blue / Green

- Não é uma função do ElasticBeanStalk
- Cria um novo Envorinment e publica a V2
- Novo environment (green) pode ser validado Roll Back se necessário
- Route 53 pode ser configurado com Weighted Policy para direcionar parte do tráfego
- Use BeanStalk para altenar “swap URL” quando terminar com o teste do novo environment

![Screenshot from 2022-05-30 21-29-05.png](Elastic%20BeanStalk%2085ef156084834fdfb7699b88321ba282/Screenshot_from_2022-05-30_21-29-05.png)

### Extensions

- Deve ser feito upload do arquivo zip para o BeanStalk
- Configuração pode ser feita console ou por arquivo
- Requerimentos:
    - Pasta .ebextensions/ no root do código fonte
    - YAML/JSON format
    - Extensão .config (exemplo: logging.config)
    - Pode-se modificar alguns parametros usando: option_settings
    - Capacidade para adicionar recursos tal como RDS, ElasticCache, DynamoDB
- Recursos gerenciados pelo .ebextensions é apagado quando environment é apagado
- É possível instalar um CLI chamado EB CLI para trabalhar com Elastic BeanStalk usando CLI
- Como funciona
    - ElasticBeanStalk utiliza CloudFormation
- Mecanismo
    - Código deve ser em formato zip
    - Arquivo zip é carregado para cada intância EC2
    - Cada Instância EC2 resolve Dependencias (lento). Exemplo instala os pacotes de depedencias do python dentro da EC2, por isso fica lento
        - Otimização em caso de longos Deployment: Subir os arquivos com os pacotes de dependencias instalados dentro do arquivo .zip