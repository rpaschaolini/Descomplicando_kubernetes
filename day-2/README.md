# � Descomplicando o Kubernetes — Expert Mode

## � DAY-2

---

# � Índice

- O que iremos ver hoje?
- O que é um Pod?
- Criando um Pod
- Criando um Pod via YAML
- Pod com múltiplos containers
- Limites de CPU e memória
- Volume EmptyDir

---

# � Início da aula do Day-2

## � O que iremos ver hoje?

Hoje vamos mergulhar no **menor objeto do Kubernetes: o Pod**.

Você vai aprender:

- Criar Pods simples  
- Criar Pods multicontainer  
- Trabalhar com volumes  
- Limitar CPU e memória  
- Inspecionar Pods em execução  
- Brincar bastante com YAML �  

---

# � O que é um Pod?

� O **Pod** é a menor unidade dentro de um cluster Kubernetes.

Pense nele como:

> � Uma caixinha que contém um ou mais containers

## � Características importantes

Containers dentro do mesmo Pod compartilham:

- IP  
- Network namespace  
- Volumes  
- Recursos  

✅ Um Pod pode ter **um ou vários containers**

---

# � Criando um Pod (via comando)

```bash
kubectl run giropops --image=nginx --port=80

Isso cria:

Pod: giropops

Imagem: nginx

Porta: 80

� Listar Pods
kubectl get pods
� Ver Pods em todos os namespaces
kubectl get pods -A

ou

kubectl get pods --all-namespaces
� Ver namespace específica
kubectl get pods -n kube-system
� Inspecionando um Pod
YAML
kubectl get pods giropops -o yaml
JSON
kubectl get pods giropops -o json
Wide
kubectl get pods giropops -o wide
� Describe (MUUUITO usado)
kubectl describe pods giropops

Mostra:

eventos

node

container

status

�️ Remover Pod
kubectl delete pods giropops
� Criando Pod via YAML

Arquivo: pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
    - name: giropops
      image: nginx
      ports:
        - containerPort: 80
� Aplicar
kubectl apply -f pod.yaml
� apply vs create

apply

cria

atualiza se existir ✅

create

cria

falha se existir ❌

� Por isso usamos mais o apply.

� Ver logs
kubectl logs giropops
Em tempo real
kubectl logs -f giropops
� Pod com múltiplos containers

Arquivo: pod-multi-container.yaml

apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
    - name: girus
      image: nginx
      ports:
        - containerPort: 80

    - name: strigus
      image: alpine
      args:
        - sleep
        - "1800"
� Criar
kubectl apply -f pod-multi-container.yaml
� attach vs exec
attach

Conecta ao processo principal:

kubectl attach giropops -c strigus

⚠️ Não cria novo processo.

exec (mais usado �)

Executa comandos dentro do container:

kubectl exec giropops -c strigus -- ls
Shell interativo
kubectl exec -it giropops -c strigus -- sh

� O -it cria terminal interativo.

⚙️ Limites de CPU e memória

Arquivo: pod-limitado.yaml

apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
    - name: girus
      image: nginx
      ports:
        - containerPort: 80
      resources:
        limits:
          memory: "128Mi"
          cpu: "0.5"
        requests:
          memory: "64Mi"
          cpu: "0.3"
� Conceitos
requests

� mínimo garantido

limits

� máximo permitido

� CPU em Kubernetes

1 CPU = 1000m

500m = 50% CPU

250m = 25% CPU

� Criar
kubectl create -f pod-limitado.yaml
� Testando com stress

Pod Ubuntu limitado:

apiVersion: v1
kind: Pod
metadata:
  name: giropops
spec:
  containers:
    - name: girus
      image: ubuntu
      args:
        - sleep
        - infinity
      resources:
        limits:
          memory: "128Mi"
          cpu: "0.5"
        requests:
          memory: "64Mi"
          cpu: "0.3"
Entrar no container
kubectl exec -it giropops -- bash

Instalar stress:

apt update
apt install -y stress
Testar memória
stress --vm 1 --vm-bytes 100M

� Para quebrar:

stress --vm 1 --vm-bytes 200M
� Volume EmptyDir
� O que é?

Volume temporário que:

nasce com o Pod

morre com o Pod

Muito usado para:

� compartilhar dados entre containers

� Manifesto

Arquivo: pod-emptydir.yaml

apiVersion: v1
kind: Pod
metadata:
  name: giropops
spec:
  containers:
    - name: girus
      image: ubuntu
      args:
        - sleep
        - infinity
      volumeMounts:
        - name: primeiro-emptydir
          mountPath: /giropops
  volumes:
    - name: primeiro-emptydir
      emptyDir:
        sizeLimit: 256Mi
� Criar
kubectl create -f pod-emptydir.yaml
� Testar

Entrar no container:

kubectl exec -it giropops -- bash

Criar arquivo:

touch /giropops/FUNCIONAAAAAA

Ver mounts:

mount
� Conclusão

Hoje você aprendeu:

✅ O que é Pod
✅ Criar Pods
✅ YAML na prática
✅ Multicontainers
✅ attach vs exec
✅ Limites de recursos
✅ Volume EmptyDir

Você agora domina o básico de Pods no Kubernetes. �

⏭️ Próximo passo

No Day-3 entramos em:

Deployments

ReplicaSets

Estratégias de rollout

Segura que o nível vai subir… ��

#VAIIII �


---

Se quiser deixar **padrão repo profissional do Romulinho™**, posso:

- ⭐ adicionar badges  
- � adicionar diagramas  
- � criar índice clicável  
- � ou versão “modo prova CKA”

Só falar:

> **“README modo insano”** ��
