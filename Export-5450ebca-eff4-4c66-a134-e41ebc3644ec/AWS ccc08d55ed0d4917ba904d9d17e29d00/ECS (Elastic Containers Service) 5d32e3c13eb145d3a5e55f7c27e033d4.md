# ECS (Elastic Containers Service)

### Clusters

- ECS Cluster são grupos lógicos de EC2
- EC2 roda ECS agent (Docker container)
- ECS Agent registra a instância EC2 no ECS cluster
- EC2 roda um AMI especial, feito exclusivamente para ECS

![Screenshot from 2022-06-30 22-35-35.png](ECS%20(Elastic%20Containers%20Service)%205d32e3c13eb145d3a5e55f7c27e033d4/Screenshot_from_2022-06-30_22-35-35.png)

### Task Definition

- Task Definition são Metadata no formato JSON que diz ao ECS como rodar Docker Container
- Contém informações cruciais sobre:
    - Image Name
    - Port Binding para Container e Host
    - Memória e CPU requerida
    - Environment Variables
    - Informações de Rede
    - IAM Role

![Screenshot from 2022-06-30 22-54-10.png](ECS%20(Elastic%20Containers%20Service)%205d32e3c13eb145d3a5e55f7c27e033d4/Screenshot_from_2022-06-30_22-54-10.png)

### Service

- ECS Services define quantos Tasks devem rodar e como eles devem rodar
- Eles garantem que o número de Tasks definidas fique sempre rodando no Cluster de EC2
- Pode trabalhar junto com ELB, NLB, ALB se necessário

### Service com Load Balancer

![Screenshot from 2022-07-01 23-03-55.png](ECS%20(Elastic%20Containers%20Service)%205d32e3c13eb145d3a5e55f7c27e033d4/Screenshot_from_2022-07-01_23-03-55.png)

- Questão de prova bastante perguntada para certificação AWS
    - Qual é o nome da função do Load Balance que permite que seja direcionados diferentes EC2 para portas diferentes, é o **Dynamic Port Forwarding**