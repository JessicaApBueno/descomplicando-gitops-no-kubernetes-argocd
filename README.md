# 🚀 Descomplicando GitOps - Day 3: Argo CD ApplicationSet

Este módulo foca na automação de deployments em escala utilizando o **Argo CD ApplicationSet**. O objetivo foi gerenciar múltiplos microserviços (`giropops-senhas` e `random-logger`) a partir de um único manifesto, seguindo as melhores práticas de GitOps.

## 🛠️ O que é um ApplicationSet?
O ApplicationSet é um controlador que estende as capacidades do Argo CD, permitindo o uso de **Generators**. No nosso caso, utilizamos o **Git Generator**, que escaneia pastas dentro de um repositório Git e cria automaticamente uma `Application` para cada diretório encontrado.

## 📂 Estrutura do Repositório
A estrutura de pastas utilizada para este projeto é a seguinte:
```
applicationset/
├── apps/
│   ├── giropops-senhas/
│   │   └── helm/             <-- Contém o Helm Chart
│   └── random-logger/
│       └── helm/             <-- Contém o Helm Chart
└── manifests/
    └── applicationset.yaml   <-- Manifesto principal
```

## 🔍 Desafio Técnico: Objeto vs. String (GoTemplate)

Durante a implementação, habilitamos a opção goTemplate: true no manifesto. Isso alterou o comportamento das variáveis de ambiente do Argo CD.

### O Problema

Ao utilizar a variável {{.path}} para definir o caminho do Helm, o Argo CD retornava um erro de ComparisonError. O log indicava que o caminho estava sendo interpretado como um objeto (map) e não como texto puro:
```
    rpc error: code = Unknown desc = map[basename:giropops-senhas ...]/helm: app path does not exist.
```

### A Solução

Para corrigir isso, ajustamos o template para acessar a propriedade específica de texto dentro do objeto de metadados do Generator.

    Incorreto: path: '{{.path}}/helm' (Resultava no objeto mapeado).

    Correto: path: '{{.path.path}}/helm' (Extrai apenas a string do caminho).

## 🔐 Configuração de Autenticação

Como o repositório é privado ou requer acesso via API, foi necessário configurar as credenciais no Argo CD:

    Conexão via HTTPS.

    Uso de Personal Access Token (PAT) do GitHub para autorizar o argocd-repo-server a listar os diretórios.

    Status da conexão validado como Successful nas configurações do painel.

## 💻 Comandos Úteis (Troubleshooting)

Durante o "Day 3", utilizamos os seguintes comandos para depuração no ambiente Linux Mint:
Bash

### Aplicar o manifesto do ApplicationSet
```
kubectl apply -f manifests/applicationset.yaml -n argocd
```

### Reiniciar o servidor de repositórios para limpar cache de manifestos
```
kubectl delete pod -n argocd -l app.kubernetes.io/name=argocd-repo-server
```

### Verificar logs e eventos do ApplicationSet
```
kubectl describe applicationset meu-primeiro-app-set -n argocd
```
<img width="1366" height="690" alt="image" src="https://github.com/user-attachments/assets/5beaf314-2359-4dfe-9fa9-c832761fec1e" />

<img width="1357" height="685" alt="image" src="https://github.com/user-attachments/assets/5b273490-8b63-4e33-b44f-09ef50c04659" />

<img width="1357" height="684" alt="image" src="https://github.com/user-attachments/assets/b0c557e1-f69f-4235-9f93-dcfcded64969" />


**Documentação gerada por Jéssica Aparecida Bueno durante o treinamento "Descomplicando GitOps no Kubernetes com ArgoCD".**
