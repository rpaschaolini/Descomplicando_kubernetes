# � Descomplicando o Kubernetes — Expert Mode  
## � DAY-3

---

## � Início da aula do Day-3

### � O que iremos ver hoje?

Durante o dia de hoje nós iremos aprender sobre um objeto muito importante no Kubernetes: **o Deployment**.

Vamos ver todos os detalhes para ter uma visão completa sobre:

- O que é um Deployment  
- Como ele funciona  
- Como criar  
- Como atualizar  
- Como fazer rollback  

Agora que já sabemos criar Pods… bora subir o nível de complexidade! �

---

# � O que é um Deployment?

No Kubernetes, um **Deployment** é um objeto que representa uma aplicação.

Ele é responsável por:

- Gerenciar Pods  
- Atualizar Pods  
- Fazer rollback de versões  
- Garantir alta disponibilidade  

� Quando criamos um Deployment, podemos definir o número de réplicas desejadas.

O Deployment garante que:

- Se um Pod morrer → ele recria  
- Se faltar réplica → ele cria  
- Se sobrar → ele remove  

� Isso ajuda MUITO na disponibilidade da aplicação.

---

## � Deployment é declarativo

Ou seja:

> Você define o estado desejado  
> O Kubernetes faz acontecer

---

## � Relação Deployment → ReplicaSet → Pods

Quando criamos um Deployment:

1. Deployment cria um **ReplicaSet**
2. ReplicaSet gerencia os **Pods**

Fluxo:


Deployment → ReplicaSet → Pods


---

# �️ Como criar um Deployment

Crie o arquivo:


deployment.yaml


Conteúdo:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        resources:
          limits:
            cpu: "0.5"
            memory: 256Mi
          requests:
            cpu: 0.25
            memory: 128Mi
� Explicando o manifesto
apiVersion e kind
apiVersion: apps/v1
kind: Deployment

Define:

Tipo do objeto

Versão da API

metadata
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment

Define:

Nome do Deployment

Labels de identificação

replicas
spec:
  replicas: 3

Define quantos Pods queremos.

selector
selector:
  matchLabels:
    app: nginx-deployment

Define quais Pods o Deployment gerencia.

strategy
strategy: {}

Usa estratégia padrão: RollingUpdate.

template

Define como o Pod será criado:

template:
  metadata:
    labels:
      app: nginx-deployment
  spec:
    containers:
    - image: nginx
      name: nginx
� Aplicando o Deployment
kubectl apply -f deployment.yaml
� Verificando o Deployment
kubectl get deployments -l app=nginx-deployment
� Verificando os Pods
kubectl get pods -l app=nginx
� Verificando o ReplicaSet
kubectl get replicasets -l app=nginx
� Detalhes do Deployment
kubectl describe deployment nginx-deployment

Mostra:

Nome

Namespace

Labels

Réplicas

Estratégia

Eventos

ReplicaSet associado

� Atualizando o Deployment

Exemplo: mudar versão do nginx.

image: nginx:1.16.0

Aplicar:

kubectl apply -f deployment.yaml
⚙️ Estratégias de atualização

O Kubernetes possui duas estratégias:

RollingUpdate (padrão)

Recreate

� RollingUpdate

Atualiza Pods gradualmente.

Exemplo:

strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 2
� Parâmetros

maxSurge

� Pods extras permitidos durante update.

maxUnavailable

� Quantos Pods podem ficar indisponíveis.

� Acompanhar rollout
kubectl rollout status deployment nginx-deployment
� Estratégia Recreate

Remove TODOS os Pods e cria novos.

strategy:
  type: Recreate

⚠️ Desvantagem:

Pode causar indisponibilidade

 Rollback de Deployment

Para voltar versão:

kubectl rollout undo deployment nginx-deployment
� Ver histórico
kubectl rollout history deployment nginx-deployment
� Voltar para revisão específica
kubectl rollout undo deployment nginx-deployment --to-revision=1
� Comandos úteis do rollout
kubectl rollout pause deployment nginx-deployment
kubectl rollout resume deployment nginx-deployment
kubectl rollout restart deployment nginx-deployment
kubectl rollout status deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment
kubectl rollout history deployment nginx-deployment
�️ Removendo o Deployment
kubectl delete deployment nginx-deployment

ou

kubectl delete -f deployment.yaml
� Conclusão

Hoje aprendemos:

✅ O que é Deployment
✅ Como criar
✅ Como atualizar
✅ RollingUpdate vs Recreate
✅ Rollback
✅ Comandos rollout
✅ Remoção

Você agora já tem uma base sólida para trabalhar com Deployments no Kubernetes. �

⏭️ Próximo passo

Na Day-4 falaremos sobre:

ReplicaSets

DaemonSets

Fica ligado… ��

#VAIIII �


---

Se quiser deixar **nível GitHub estrela dourada ⭐**, posso:

- ✅ adicionar badges  
- ✅ adicionar diagrama visual  
- ✅ versão README premium  
- ✅ ou estilo bootcamp (sua cara �)

Só mandar: **"bora tunar o README"**
