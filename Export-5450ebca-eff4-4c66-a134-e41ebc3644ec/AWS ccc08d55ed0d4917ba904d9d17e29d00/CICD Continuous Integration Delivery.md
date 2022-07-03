# CICD Continuous Integration / Delivery (Item obrigatório para certificação Desenvolvedor AWS)

[CodeCommit](CICD%20Continuous%20Integration%20Delivery/CodeCommit%20444f18563d414abf90bf153d6b791098.md)

[Code Pipeline](CICD%20Continuous%20Integration%20Delivery/Code%20Pipeline%2037af35c5e6b74a90a1e7272065fc613f.md)

[CodeBuild](CICD%20Continuous%20Integration%20Delivery/CodeBuild%204e1dca473eb346efb2e65e83a85fa133.md)

[CodeDeploy](CICD%20Continuous%20Integration%20Delivery/CodeDeploy%2076a4186108c94d2bbf4d8d5f2ef67d9d.md)

### Continuous Integration

- Desenvolvedores Push do Código para repositório, frequentemente GitHub ou CodeCommit.
- Build Server verifica/testa o código assim que é atualizado usando CodeBuild ou Jenkins
- O desenvolvedor recebe o feedback sobre o teste e verifica se o código passou no teste ou falhou
- Encontre bugs rapidamente e conserte
- Rápido deployment depois do código ser testado
- Deployment mais frequente
- Desenvolvedor mais felizes

    ![Fluxo **Continuous Integration**](CICD%20Continuous%20Integration%20Delivery/Screenshot_from_2022-06-02_11-18-01.png)

    Fluxo **Continuous Integration**


### Continuous Delivery

- Garante que o código será distribuido de forma confiável quando necessário
- Garante que Deployment pode ser frequente e rápido
- Muda o conceito de atualizar o código 1 vez por semana para 4 vezes por dia

![Fluxo **Continuous Delivery**](CICD%20Continuous%20Integration%20Delivery/Screenshot_from_2022-06-02_11-23-24.png)

Fluxo **Continuous Delivery**

### Etapas CICD

![Diagrama do **CICD** da **AWS**](CICD%20Continuous%20Integration%20Delivery/Screenshot_from_2022-06-02_11-30-21.png)

Diagrama do **CICD** da **AWS**
