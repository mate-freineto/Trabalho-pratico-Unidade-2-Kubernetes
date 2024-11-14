# Trabalho prático Unidade 2 Kubernetes

---

Descompacte o arquivo `trabalho-pratico-unidade2-kubernetes.tar.gz`

```sh
tar -xzvf trabalho-pratico-unidade2-kubernetes.tar.gz
```

```sh
trabalho-pratico-unidade2-kubernetes
├── README.md
├── backend-deployment.yaml
├── backend-hpa.yaml
├── backend-service.yaml
├── frontend-deployment.yaml
├── frontend-service.yaml
├── guessgameapp/
├── postgres-deployment.yaml
├── postgres-pvc.yaml
├── postgres-service.yaml
└── postgres-storage.yaml
```

---

## Guia de Implantação do GuessGameApp

## Introdução

Este documento fornece um guia detalhado sobre como implantar o sistema GuessGameApp em um cluster Kubernetes usando Minikube. O sistema é composto por um banco de dados PostgreSQL, um serviço backend e um serviço frontend.

## Pré-requisitos

Antes de começar, certifique-se de ter os seguintes requisitos instalados:

- Minikube
- kubectl
- Helm

## Passos para Implantação

Siga os passos abaixo para implantar o sistema GuessGameApp:

### 1. Iniciar o Minikube

Inicie o Minikube se ele ainda não estiver em execução:

```sh
minikube start
```

### 2. Aplicar os Manifests do Kubernetes

Aplique os seguintes manifests do Kubernetes na ordem especificada para implantar o banco de dados PostgreSQL, o serviço backend e o serviço frontend.

#### Armazenamento do PostgreSQL

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/postgres-storage.yaml
```

#### PersistentVolumeClaim do PostgreSQL

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/postgres-pvc.yaml
```

#### Deployment do PostgreSQL

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/postgres-deployment.yaml
```

#### Serviço do PostgreSQL

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/postgres-service.yaml
```

#### Serviço do HPA

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/backend-hpa.yaml
```

#### Deployment do Backend

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/backend-deployment.yaml
```

#### Serviço do Backend

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/backend-service.yaml
```

#### Deployment do Frontend

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/frontend-deployment.yaml
```

#### Serviço do Frontend

```sh
kubectl apply -f trabalho-pratico-unidade2-kubernetes/frontend-service.yaml
```

## Testando a Implantação

Para testar a implantação, você pode usar o comando `kubectl port-forward` para encaminhar o serviço frontend para sua máquina local:

```sh
kubectl port-forward svc/frontend 8080:8080
```

Abra o seu navegador e navegue para `http://127.0.0.1:8080` para acessar o frontend do GuessGameApp.

## Limpando o Ambiente

Após os testes, você pode remover as configurações do cluster deletando os recursos aplicados:

```sh
kubectl delete -f trabalho-pratico-unidade2-kubernetes/
```

Isso limpará todos os recursos criados durante a implantação.

---

# Bônus - Helm Charts

## Utilizando o Helm Chart para Executar o GuessGameApp

Para facilitar a implantação do GuessGameApp, siga os passos abaixo utilizando o Helm.

### Estrutura do Diretório do Chart

```text
guessgameapp/
├── Chart.yaml
├── templates
│   ├── deployment-backend.yaml
│   ├── deployment-frontend.yaml
│   ├── deployment-postgres.yaml
│   ├── hpa-backend.yaml
│   ├── postgres-pvc.yaml
│   ├── postgres-storage.yaml
│   ├── service-backend.yaml
│   ├── service-frontend.yaml
│   └── service-postgres.yaml
└── values.yaml
```

## Passos para Implantação

Siga os passos abaixo para implantar o sistema GuessGameApp:

### 1. Iniciar o Minikube

Inicie o Minikube se ele ainda não estiver em execução:

```sh
minikube start
```

### 2. Implantar o GuessGameApp

Implante o GuessGameApp usando o Helm chart:

```sh
helm upgrade --install guessgameapp trabalho-pratico-unidade2-kubernetes/guessgameapp/
```

### 3. Testar a Implantação

Para testar a implantação, você pode usar o comando `kubectl port-forward` para encaminhar o serviço frontend para sua máquina local:

```sh
kubectl port-forward svc/frontend 8080:8080
```

Abra o seu navegador e navegue para `http://127.0.0.1:8080` para acessar o frontend do GuessGameApp.

### 4. Desinstalar o GuessGameApp

Após os testes, você pode desinstalar o GuessGameApp usando o comando Helm:

```sh
helm uninstall guessgameapp
```

Isso limpará todos os recursos criados durante a implantação.
