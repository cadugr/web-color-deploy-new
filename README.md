# web-color-deploy

Este projeto demonstra como implantar uma aplicação web simples em um cluster Kubernetes. A aplicação exibe uma página com uma cor de fundo, definida pela imagem de contêiner utilizada no manifesto de implantação.

## Premissas

Antes de começar, você precisará ter o seguinte:

1.  **Um cluster Kubernetes em execução:** Você pode usar um ambiente local como [Minikube](https://minikube.sigs.k8s.io/docs/start/), [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/), [Docker Desktop](https://www.docker.com/products/docker-desktop/) ou um cluster em um provedor de nuvem como Google Kubernetes Engine (GKE), Amazon EKS ou Azure AKS.
2.  **`kubectl` instalado e configurado:** A ferramenta de linha de comando do Kubernetes (`kubectl`) deve estar instalada e configurada para se comunicar com seu cluster. Você pode verificar a conexão com o comando:
    ```bash
    kubectl cluster-info
    ```

## Como Executar o Projeto

Siga os passos abaixo para implantar a aplicação no seu cluster.

### 1. Aplicar o Manifesto de Implantação

O arquivo `k8s/deployment.yaml` contém as definições dos recursos necessários para a aplicação, incluindo um `Deployment` para gerenciar os pods e um `Service` para expô-los.

Para aplicar este manifesto, execute o seguinte comando na raiz do projeto:

```bash
kubectl apply -f k8s/deployment.yaml
```

### 2. Verificar o Status da Implantação

Após aplicar o manifesto, o Kubernetes começará a criar os recursos. Você pode verificar o status da implantação e dos pods com os comandos:

```bash
# Verificar se o deployment foi criado com sucesso
kubectl get deployment webcolor

# Verificar se os pods estão em execução (STATUS deve ser "Running")
kubectl get pods -l app=webcolor
```

### 3. Acessar a Aplicação

O manifesto cria um serviço do tipo `LoadBalancer`, que expõe a aplicação à internet através de um endereço de IP externo.

Para encontrar o IP externo, execute:

```bash
kubectl get service webcolor
```

Aguarde até que um endereço de IP apareça na coluna `EXTERNAL-IP`. Isso pode levar alguns minutos, dependendo do seu provedor de nuvem.

Assim que o IP estiver disponível, copie-o e cole no seu navegador para ver a aplicação.

> **Nota para Ambientes Locais:** Se você estiver usando um cluster local como o Minikube, o IP externo pode não ser provisionado automaticamente. Nesse caso, você pode expor o serviço com o seguinte comando:
> ```bash
> minikube service webcolor
> ```
> Este comando abrirá a aplicação diretamente no seu navegador padrão.
