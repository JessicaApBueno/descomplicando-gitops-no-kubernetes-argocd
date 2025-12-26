# 🏗️ Implementação do Padrão App-of-Apps no Argo CD

Este projeto utiliza o padrão arquitetural App-of-Apps, uma estratégia avançada de GitOps que permite gerenciar múltiplas aplicações a partir de um único ponto de entrada.

## 🧐 O que é o App-of-Apps?

Em vez de configurar manualmente cada microsserviço no painel do Argo CD, criamos uma aplicação "Pai" (Root App). Esta aplicação monitora um diretório no Git que contém os manifestos de outras aplicações "Filhas".

Vantagens observadas:

    Automação Total: Ao adicionar um novo arquivo YAML na pasta monitorada, o Argo CD provisiona a nova aplicação automaticamente.

    Consistência: Garante que todo o cluster siga a mesma revisão do Git (targetRevision).

    Escalabilidade: Facilita o gerenciamento de dezenas ou centenas de aplicações sem intervenção manual na interface.

## 📁 Estrutura de Diretórios Implementada

A organização dos arquivos seguiu a hierarquia necessária para que o Argo CD mapeasse os caminhos corretamente:
Plaintext
```
└── applications/
    ├── app-of-apps.yaml      # Manifesto da aplicação "Pai"
    └── app-of-apps/          # Diretório monitorado (Caminho da verdade)
        ├── giropops-senhas.yaml
        └── random-logger.yaml
```
## ⚙️ Configuração do Manifesto RaizO
O arquivo app-of-apps.yaml foi configurado com os seguintes parâmetros críticos::

    Repo URL: https://github.com/JessicaApBueno/descomplicando-gitops-no-kubernetes-argocd

    Path: applications/app-of-apps (Caminho relativo à raiz do repositório).

    Sync Policy:

        Automated: Sincronização automática de mudanças no Git.

        Prune: Remoção de recursos deletados no repositório.

        SelfHeal: Correção automática de desvios manuais no cluster.

🚀 Como reproduzir a implantação

Para subir todo o ecossistema de aplicações de uma vez, basta aplicar o manifesto pai:
Bash

kubectl apply -f applications/app-of-apps.yaml

Após este comando, o Argo CD iniciará a cascata de sincronização:

    Sincroniza o app-of-apps.

    Identifica os arquivos na subpasta.

    Cria e sincroniza o giropops-senhas e o random-logger.

✅ Resultados Alcançados

    Cluster Saudável: Todas as aplicações atingiram o estado Healthy.

    Sincronia Garantida: O estado do cluster reflete fielmente a branch feat/day2.

    Resiliência: O sistema está configurado para auto-correção, mantendo a conformidade do ambiente.
``

<img width="1131" height="643" alt="image" src="https://github.com/user-attachments/assets/1f7322f7-5a67-4bdd-99b6-faa42b654f68" />
