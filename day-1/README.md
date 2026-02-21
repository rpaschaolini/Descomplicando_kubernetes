# � Descomplicando o Kubernetes — Expert Mode

## � DAY-1

---

# � O que preciso saber antes de começar?

Durante o **Day-1** nós vamos:

- Entender o que é um container  
- Falar sobre Container Runtime e Container Engine  
- Conhecer o Kubernetes e sua arquitetura  
- Explorar control plane e workers  
- Criar nosso primeiro cluster  
- Fazer deploy de um Pod Nginx  

� Objetivo: ganhar conforto com os conceitos iniciais do Kubernetes.

---

# � Qual distro GNU/Linux devo usar?

Como ferramentas como **systemd** e **journald** são padrão hoje, você pode usar tranquilamente:

- Ubuntu  
- Debian  
- CentOS  
- e similares  

---

# � Sites importantes

## Projeto Kubernetes

- https://kubernetes.io  
- https://github.com/kubernetes/kubernetes/  
- https://github.com/kubernetes/kubernetes/issues  

## Certificações Kubernetes

- https://www.cncf.io/certification/cka/  
- https://www.cncf.io/certification/ckad/  
- https://www.cncf.io/certification/cks/  

---

# � O Container Engine

Antes do Kubernetes, precisamos entender o **Container Engine**.

Ele é responsável por:

- Gerenciar imagens  
- Gerenciar volumes  
- Isolamento de recursos  
- Ciclo de vida do container  
- Rede e storage  

## � Opções populares

- Docker  
- CRI-O  
- Podman  

� O Docker utiliza o **containerd** como runtime.

---

# � OCI — Open Container Initiative

A **OCI** padroniza a criação e execução de containers.

- Criada em 2015  
- Parte da Linux Foundation  
- Mantém o projeto **runc**

� O **runc** é o runtime de baixo nível mais usado.

---

# ⚙️ Container Runtime

O **Container Runtime** é quem executa os containers nos nós.

## � Tipos de runtime

### Low-level
Executam direto no kernel:

- runc  
- crun  
- runsc  

### High-level

Executados por um Container Engine:

- containerd  
- CRI-O  
- Podman  

### Sandbox

Executam containers com isolamento extra:

- gVisor  

### Virtualized

Executam containers dentro de VMs:

- Kata Containers  

---

# ☸️ O que é Kubernetes?

## � Versão resumida

O Kubernetes (k8s):

- Criado pela Google (~2014)  
- Orquestrador de containers  
- Open source  
- Inspirado no Borg  

� "k8s" = k + 8 letras + s

---

## � Versão longa (história rápida)

Evolução dentro do Google:

1. Borg  
2. Omega  
3. Kubernetes  

� Objetivo do k8s:

- Facilitar deploy  
- Gerenciar sistemas distribuídos  
- Melhor uso de recursos  

---

# �️ Arquitetura do Kubernetes

O cluster segue o modelo:


Control Plane + Workers


## � Control Plane

Responsável pelo gerenciamento.

## � Workers

Executam as aplicações.

� Produção recomendada: **mínimo 3 nós**

---

# � Kubernetes local (para estudos)

Ferramentas populares:

- Kind  
- Minikube  
- MicroK8s  
- k3s  
- k0s  

---

# � Componentes do Control Plane

## API Server

- API REST do cluster  
- Comunicação via HTTP/JSON  
- Usado pelo kubectl  

---

## etcd

Banco chave-valor distribuído.

Armazena:

- Configurações  
- Estado do cluster  
- Especificações  

⚠️ Acesso apenas via API.

---

## Scheduler

Decide **em qual nó o Pod vai rodar**, baseado em:

- Recursos disponíveis  
- Estado dos nós  
- Afinidades  

---

## Controller Manager

Garante que o estado atual = estado desejado.

Exemplo:

> Quer 10 pods → ele garante.

---

## Kubelet

Agente que roda nos workers.

Responsável por:

- Iniciar containers  
- Parar containers  
- Manter pods vivos  

---

## Kube-proxy

Funciona como:

- Proxy  
- Load balancer  
- Roteador de rede  

---

# � Portas importantes

## Control Plane

| Porta | Uso |
|------|-----|
| 6443 | API Server |
| 2379-2380 | etcd |
| 10250 | Kubelet |
| 10251 | Scheduler |
| 10252 | Controller Manager |

---

## Workers

| Porta | Uso |
|------|-----|
| 10250 | Kubelet |
| 30000-32767 | NodePort |

---

# � Conceitos-chave do k8s

## � Pod

- Menor objeto do Kubernetes  
- Pode ter 1+ containers  
- Compartilha rede e storage  

---

## � Deployment

Controller que:

- Mantém número de réplicas  
- Gerencia ciclo de vida  
- Atualiza aplicações  

---

## � ReplicaSet

Garante a quantidade de pods.

---

## � Service

Expõe aplicações via:

- ClusterIP  
- NodePort  
- LoadBalancer  

---

# �️ Instalando o kubectl

## GNU/Linux

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
MacOS
brew install kubectl
kubectl version --client
Windows

Baixe o binário oficial no site do Kubernetes.

⚡ Customizando o kubectl
Autocomplete (Bash)
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
Autocomplete (ZSH)
source <(kubectl completion zsh)
Alias
alias k=kubectl
complete -F __start_kubectl k
� Criando cluster local
� Minikube
Requisitos

1 CPU

2 GB RAM

20 GB disco

Instalação (Linux)
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x ./minikube
sudo mv ./minikube /usr/local/bin/minikube
minikube version
� Iniciar cluster
minikube start

Ver nós:

kubectl get nodes
� Kind (Kubernetes in Docker)
Instalação Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
Criar cluster
kind create cluster
kubectl get nodes
� Primeiros passos no k8s
Ver namespaces
kubectl get namespaces
Ver pods do sistema
kubectl get pod -n kube-system
Ver tudo
kubectl get pods -A
� Primeiro Pod (nginx)
kubectl run nginx --image nginx
kubectl get pods

Remover:

kubectl delete pod nginx
� Criando manifesto com dry-run
Template de Pod
kubectl run meu-nginx --image nginx --dry-run=client -o yaml > pod-template.yaml

Criar:

kubectl apply -f pod-template.yaml
� Expondo o Pod
kubectl expose pod meu-nginx
kubectl get services
� Limpando recursos
kubectl get all

kubectl delete -f pod-template.yaml
kubectl delete service nginx
� Conclusão

No Day-1 aprendemos:

✅ Fundamentos de containers
✅ Container Engine vs Runtime
✅ Arquitetura do Kubernetes
✅ Componentes do cluster
✅ Instalação do kubectl
✅ Cluster local
✅ Primeiro Pod
✅ Services

Você agora tem a base para evoluir no Kubernetes. �

⏭️ Próximo passo

No Day-2 vamos aprofundar nos Pods.

Fica ligado… ��

#VAIIII �


---

Se quiser, no próximo passo eu deixo:

- ⭐ badges estilo GitHub  
- � diagrama da arquitetura  
- � versão ultra-didática (bootcamp mode)  
- � ou README padrão do teu repo Descomplicando_kubernetes

Só falar: **“tunar nível hard”** �
