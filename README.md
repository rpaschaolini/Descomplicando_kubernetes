# Descomplicando Kubernetes — Day 1 �

Repositório criado como parte do curso **Descomplicando Kubernetes** da linuxtips.io.

## Estrutura

day-1/
├── pod.yaml
└── kind/
└── kind-cluster.yaml


## Conteúdo

### pod.yaml
Define um Pod simples com NGINX:
- 1 container
- Porta 80
- RestartPolicy Always

### kind-cluster.yaml
Cria um cluster local com Kind:
- 1 control-plane
- 2 workers

## Como usar

Criar o cluster:
```bash
kind create cluster --config day-1/kind/kind-cluster.yaml

kubectl apply -f day-1/pod.yaml


---

## � PASSO 5 — Inicializar o Git (agora vai liso)

```bash
git init
git branch -m main
