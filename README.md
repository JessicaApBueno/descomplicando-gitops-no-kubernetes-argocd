# Descomplicando GitOps no Kubernetes com Argo CD 🚀

Este repositório contém os exercícios e manifestos práticos realizados durante o estudo de GitOps, utilizando o Argo CD para gerenciar o ciclo de vida de aplicações em um cluster Kind.

## 🛠️ Tecnologias Utilizadas

    Kubernetes (Kind): Cluster local rodando em containers Docker.

    Argo CD: Ferramenta de Continuous Delivery declarativa para Kubernetes.

    Git: Fonte da verdade para as configurações do cluster.

    Helm: Gerenciador de pacotes para Kubernetes.

## 📋 Jornada de Implementação
1. Configuração do Ambiente

O cluster foi criado utilizando o Kind. Para conectar o terminal ao cluster, exportamos a configuração necessária:
Bash

kind export kubeconfig --name kind

2. Instalação do Argo CD

Criamos um namespace dedicado e aplicamos os manifestos oficiais:
Bash

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

3. Acesso ao Painel e Autenticação

Para acessar a interface Web localmente, utilizamos o redirecionamento de porta:
Bash

kubectl port-forward svc/argocd-server -n argocd 8080:443

    Usuário: admin

    Senha: Recuperada via Secret do Kubernetes e decodificada de Base64.

4. Estratégia de Branching

Trabalhamos com o conceito de branches para novas funcionalidades:

    Criada a branch feat/day2 para o desenvolvimento da aplicação de senhas.

    Utilizado Personal Access Token (PAT) do GitHub para autenticação segura entre o Argo CD e este repositório privado/público.

## 🚀 Aplicação: Giropops-Senhas

A aplicação principal foi implantada utilizando um arquivo de definição do Argo CD (Application).

Destaques do Manifesto:

    Namespace Automático: Configurado CreateNamespace=true nas syncOptions.

    Self-Healing: Ativado para garantir que mudanças manuais no cluster sejam sobrescritas pelo que está no Git.

    Prune: Ativado para remover recursos do cluster que foram deletados no repositório.

Como visualizar os recursos:
Bash

kubectl get all -n giropops-senhas

📸 Evidências

O sistema está operando com sucesso, com todos os Pods (Redis e App) em estado Running e sincronizados via Helm.
<img width="1359" height="688" alt="image" src="https://github.com/user-attachments/assets/b0e569de-1860-45b6-b3d7-7e95b90d6d08" />
<img width="1342" height="688" alt="image" src="https://github.com/user-attachments/assets/24ae9c35-4650-4d57-927f-132835d3b92a" />
