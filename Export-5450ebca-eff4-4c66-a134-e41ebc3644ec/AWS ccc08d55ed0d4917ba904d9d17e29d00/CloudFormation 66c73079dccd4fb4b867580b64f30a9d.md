# CloudFormation

### (O exame não pede que você escreva código em CloudFormation para certificação Developer AWS)

- Infraestrutura como código
- Automatiza o processo de criar infraestrutura
    - Em outra região
    - Em outra conta AWS
    - Na mesma região, se tudo for deletado
- CloudFormation é uma forma declarativa de representar sua infraestrutura na AWS para a maioria dos recursos
- Em CloudFormation pode dizer coisas do tipo:
    - Crie um Security Group
    - Crie 2 instâncias EC2 usando esse Security Group
    - Crie 2 Elastic IP para estas 2 instâncias EC2
    - Crie um S3 Bucket
    - Crie um Load Balancer (ELB) na frente dessas Instâncias EC2
- Então, CloudFormation criará tudo que você pediu, na ordem correta. (Não é necessário especificar a ordem)

### Vantagens

- Infraestrutura como código
    - Nenhum recurso é criado manualmente (ótimo para controle)
    - Código pode usar controle de versão. Exemplo: Git
    - Mudanças na Infraestrutura são vista como código
- Custo
    - Cada recurso recebe um identificador, fácil de saber quando o stack custa
    - É possível estimar o custo usando CLoudFormation Template
    - É possível programar seu ambiente de desenvolvimento para terminar as 17h e ser recriado ás 9h
- Produtividade
    - Possibilidade de destruir e criar recursos AWS dinamicamente
    - Diagrama da sua Infraestrutura é gerado automaticamente
    - Programação Declarativa. Sem necessidade de especificar a order
- Possível criar vários stacks para vários níveis (layer)
    - VPC stack
    - Network stack
    - App stack
- Não crie stacks do zero:
    - Use modelos disponíveis na web

### Como Funciona

- Templates precisam ser uploaded para S3 e então podem ser referenciados no CloudFormation
- Para atualizar o Template, não é possível editar o Template atual, é preciso fazer o upload de nova versão do Template.
- Stacks são identificados pelo nome
- Deletar um Stack. Deleta os Artifacts que foram criados pelo CloudFormation

### Deploy CloudFormation Template

- Manualmente:
    - Editando o Template no CloudFormation Designer
    - Usar o console para digitar os parametros
- Automaticamente:
    - Editar o Template usando arquivo YAM ou JSON
    - Usando CLI para Deploy Templates
    - Método recomendado quando se deseja automatizar sua plataforma

### Estrutura

Componentes dos Templates:

1. Resources: Recursos AWS declarados no Template (Obrigatório, são como se fosem os serviços disponíveis da aws para serem utilizados, como EC2, Lambda entre outros)
2. Parameters: Entradas dinâmicas para seu Templates (usuário imputa dados, como nome de uma EC2)
3. Mapping: Constantes para seu Template
4. Outputs: Referência aos Resources que foram criados
5. Conditionals: Lista de condições para criar Resources
6. Metadata

Template Helpers:

1. Reference
2. Functions

### YAML

- YAML e JSON podem ser usados com CloudFormation
- Suporta:
    - Key Value Pair
    - Nested Objects
    - Suporta Arrays
    - Strings em multiplas linhas
    - Pode incluir comentários

### Resource

- Resource é o centro do CloudFormation Template (obrigatório)
- Representa os diferentes componentes da AWS que serão criados e configurados
- Resource podem referenciar outros Resources
- Existem mais de 224 tipos de Resources
- Resources Types Identifiers são escritos no formato:
    - AWS::nome-produto-aws::nome-data-type

### Parameters

- Parameters é a forma de entrarmos com dados para a criação de Resources
- Exemplos de uso de Parameters:
    - Reusar seus Templates dentro de Empresa
    - Quando algumas Entradas não podem ser definidas antecipadamente
- Parameters são muito importantes e previnem erros
- Quando usar?
- Faça a seguinte pergunta:
    - A configuração desse CloudFormation vai mudar no futuro? se sim, use parameters.
- Não é necessário atualizar o Template para mudar seu conteúdo

![Exemplo de código  **Parameters do** **CloudFormation**](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-12_21-55-59.png)

Exemplo de código  **Parameters do** **CloudFormation**

- Função **Fn:Ref** é usada para referenciar parameters
- Parameters podem ser usados em qualquer lugar no Template
- Pode-se usar **!Ref** ao invés de **Fn:Ref**
- **!Ref** pode referenciar outros elementos dentro do Template
    
    ![Exemplo de código **!Ref do CloudFormation**](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-12_22-00-21.png)
    
    Exemplo de código **!Ref do CloudFormation**
    

### Mappings

- Mapping
    - Mappings são valores fixos dentro do CloudFormation Template
    - Muito útil para diferenciar dados entre Environments (dev vs prod), Regions, Tipos de AMI e outros
    - Todos os valores são Hardcoded no Template
    - Example

![Screenshot from 2022-06-13 21-45-21.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_21-45-21.png)

### Mappings vs Parameters

- Mapping é ideal quando se sabe os valores que serão usados e esses valores pode vir de variáveis tais como:
    - Region
    - Availability Zone
    - AWS Account
    - Environment (dev vs prod)
- Permite um controle seguro sobre o Template
- Use Parameter quando os valores devem ser definidos pelo usuário

### Acessando Valores Mapping

- Fn:FindlnMap
    - Retorna o valor (value) de uma key
    - !FindlnMap [ MapName, TopLevelKey, SecondLevelKey ]

![Screenshot from 2022-06-13 21-54-05.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_21-54-05.png)

### Outputs

- Output declara valores que podem ser importados por outros Stacks (Se você exporta-lo primeiro)
- Pode-se ver o Output no Console AWS ou usando AWS CLI
- Outputs são úteis quando você define uma rede no CloudFormation e, Output variáveis tal como VPC ID e Subnet ID
- É a melhor forma de colaborar usando Stacks. Cada Expert desenvolve um Stack e exporta as variáveis
- Não é possível deletar um Stack CloudFormation se um Output estiver sendo referenciado por outro Stack CloudFormation
- Exemplos:
    - Criar um SSH Security Group como parte do Template
    - Criar um Ouput que referencia esta Security Group

![Screenshot from 2022-06-13 22-03-07.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_22-03-07.png)

- Continuando:
    - Criar um segundo Template que utiliza o Security Group criado pelo outro Template
    - Usamos a função !ImportValue
    
    ![Screenshot from 2022-06-13 22-05-37.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_22-05-37.png)
    

### Conditions

- Conditions são usados para criar Resouces ou Outputs baseado em uma condição
- Conditions pode ser qualquer condição que você quiser. Alguns exemplos:
    - Environment (dev / test / prod)
    - AWS Region
    - Any Parameter Value

- Funções que podem ser utilizadas:
    - !And
    - !Equals
    - !If
    - !Not
    - !Or

![Screenshot from 2022-06-13 22-11-21.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_22-11-21.png)

- Conditions podem ser aplicados:
    - Resources
    - Output
    - etc

### GetAtt

- Atributos são anexados aos recursos que você cria
- Exemplo: Saber qual é o Availability Zone de uma Instância EC2

![Screenshot from 2022-06-13 22-22-19.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_22-22-19.png)

### Join

- Join Values com Delimiter
    - !Join [ delimiter, [ comma-delimited list of values]]
- Cria “a:b:c”
    - !Join [ “:”, [a, b, c] ]

### Sub

- Fn:Sub ou !Sub é usado para substituir variáveis de um texto. Permite personalizar seu Template
- Exemplo: É possível combinar !Sub com References ou AWS Pseudo Variables
- String deve conter ${VariableName} e irá substituí-las

![Screenshot from 2022-06-13 22-29-05.png](CloudFormation%2066c73079dccd4fb4b867580b64f30a9d/Screenshot_from_2022-06-13_22-29-05.png)

### Rollback

- Stack Creation Fails:
    - Default: tudo volta como era antes (rollback). Tudo é registrado nos logs
    - Opção para desabilitar rollback e investigar o problema
- Stack Update Fails:
    - Stack automaticamente volta para o estágio anterior
    - Possibilidade de ver logs e saber o que aconteceu de errado