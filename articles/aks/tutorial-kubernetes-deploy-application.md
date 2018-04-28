---
title: Tutorial do Kubernetes no Azure – Implementar Aplicação
description: Tutorial de AKS – Implementar Aplicação
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 79fc716f70fe0fb3273badd485e4c01ea5d04e1d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-run-applications-in-azure-container-service-aks"></a>Tutorial: Executar aplicações no Azure Container Service (AKS)

Neste tutorial, parte quatro de oito, a aplicação de exemplo é implementada num cluster do Kubernetes. Os passos concluídos incluem:

> [!div class="checklist"]
> * Atualizar ficheiros de manifesto Kubernetes
> * Executar uma aplicação no Kubernetes
> * Testar a aplicação

Nos tutoriais subsequentes, esta aplicação é aumentada horizontalmente, atualizada e o Log Analytics é configurado para monitorizar o cluster de Kubernetes.

Este tutorial pressupõe um conhecimento básico dos conceitos do Kubernetes. Para obter informações detalhadas sobre o Kubernetes, veja a [documentação do Kubernetes][kubernetes-documentation].

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, a imagem foi carregada para o Azure Container Registry e foi criado um cluster de Kubernetes. 

Para concluir este tutorial, precisa do ficheiro de manifesto previamente criado do Kubernetes `azure-vote-all-in-one-redis.yaml`. Este ficheiro foi transferido com o código de origem da aplicação num tutorial anterior. Certifique-se de que tem um clone do repositório e que foram alterados diretórios no repositório clonado.

Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="update-manifest-file"></a>Atualizar ficheiro de manifesto

Neste tutorial, o Azure Container Registry (ACR) serviu para armazenar uma imagem de contentor. Antes de executar a aplicação, o nome de servidor de início de sessão ACR tem de ser atualizado no ficheiro de manifesto Kubernetes.

Obtenha o nome do servidor de início de sessão ACR com o comando [az acr list][az-acr-list].

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

O ficheiro de manifesto foi previamente criado com um nome de servidor de início de sessão de `microsoft`. Abra o ficheiro com qualquer editor de texto. Neste exemplo, o ficheiro está aberto com `nano`.

```console
nano azure-vote-all-in-one-redis.yaml
```

Substitua `microsoft` pelo nome do servidor de início de sessão do ACR. Este valor foi encontrado na linha **47** do ficheiro de manifesto.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

O código acima passa a apresentar o seguinte.

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Guarde e feche o ficheiro.

## <a name="deploy-application"></a>Implementar aplicação

Utilize o comando [kubectl create][kubectl-create] para executar a aplicação. Este comando analisa o ficheiro de manifesto e cria os objetos de Kubernetes definidos.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yaml
```

Saída:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testar aplicação

O [serviço do Kubernetes][kubernetes-service] é criado, o que expõe a aplicação na Internet. Este processo pode demorar alguns minutos. 

Para monitorizar o progresso, utilize o comando [kubectl get service][kubectl-get] com o argumento `--watch`.

```azurecli
kubectl get service azure-vote-front --watch
```

Inicialmente, o *EXTERNAL-IP* do serviço *azure-vote-front* aparece como *pendente*.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Quando o endereço *EXTERNAL-IP* mudar de *pendente* para *Endereço IP*, utilize `CTRL-C` para parar o processo de observação do kubectl. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Para ver a aplicação, navegue para o endereço IP externo.

![Imagem do cluster do Kubernetes no Azure no Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

Se o carregamento da aplicação tiver falhado, poderá dever-se a um problema de autorização com o registo de imagem.

Siga estes passos para [permitir o acesso através de um segredo do Kubernetes](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, a aplicação do Azure vote foi implementada num cluster de Kubernetes do AKS. As tarefas concluídas incluem:  

> [!div class="checklist"]
> * Transferir ficheiros de manifesto Kubernetes
> * Executar a aplicação no Kubernetes
> * Testou a aplicação

Avance para o próximo tutorial para saber mais sobre o dimensionamento de uma aplicação Kubernetes e a infraestrutura de Kubernetes subjacente. 

> [!div class="nextstepaction"]
> [Dimensionar a aplicação e infraestrutura do Kubernetes][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
