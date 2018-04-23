---
title: Tutorial do Kubernetes no Azure – Implementar Cluster
description: Tutorial de AKS – Implementar Cluster
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/24/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 99b65ba5e5f0be40e464fd6565db40fda4170a0f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-deploy-an-azure-container-service-aks-cluster"></a>Tutorial: Implementar um cluster do Azure Container Service (AKS)

O Kubernetes dispõe de uma plataforma distribuída para aplicações em contentores. Com o AKS, o aprovisionamento de um cluster do Kubernetes pronto para produção é simples e rápido. Neste tutorial, parte três de oito, é implementado um cluster do Kubernetes no AKS. Os passos concluídos incluem:

> [!div class="checklist"]
> * Implementar um cluster do Kubernetes AKS
> * Instalação da CLI do Kubernetes (kubectl)
> * Configuração de kubectl

Nos tutoriais subsequentes, a aplicação Azure Vote é implementada no cluster, dimensionada, atualizada e o Log Analytics é configurado para monitorizar o cluster de Kubernetes.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi criada e carregada uma imagem de contentor para uma instância do Azure Container Registry. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="enable-aks-preview"></a>Ativar a pré-visualização do AKS

Enquanto o AKS está em pré-visualização, a criação de novos clusters exige um sinalizador de funcionalidade na sua subscrição. Pode pedir esta funcionalidade para qualquer número de subscrições que pretende utilizar. Utilize o comando `az provider register` para registar o fornecedor do AKS:

```azurecli
az provider register -n Microsoft.ContainerService
```

Depois de registar, está pronto para criar um cluster do Kubernetes com o AKS.

## <a name="create-kubernetes-cluster"></a>Criar cluster do Kubernetes

O exemplo seguinte cria um cluster com o nome `myAKSCluster` num Grupo de Recursos chamado `myResourceGroup`. Este Grupo de Recursos foi criado no [tutorial anterior][aks-tutorial-prepare-acr].

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

Ao fim de vários minutos, a implementação é concluída e devolve informações sobre a implementação do AKS no formato json.

## <a name="install-the-kubectl-cli"></a>Instalar a CLI do kubectl

Para ligar ao cluster de Kubernetes a partir do computador cliente, utilize [kubectl][kubectl], o cliente de linha de comandos do Kubernetes.

Se estiver a utilizar o Azure CloudShell, kubectl já está instalado. Se pretender instalá-lo localmente, execute o seguinte comando:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Ligar-se com kubectl

Para configurar o kubectl para se ligar ao seu cluster do Kubernetes, execute o comando seguinte:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para verificar a ligação ao cluster, execute o comando [kubectl get nodes][kubectl-get].

```azurecli
kubectl get nodes
```

Saída:

```
NAME                          STATUS    AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     49m       v1.7.9
```

Ao concluir o tutorial, tem um cluster do AKS pronto para cargas de trabalho. Nos tutoriais subsequentes, é implementada uma aplicação com vários contentores neste cluster, ampliada, atualizada e monitorizada.

## <a name="configure-acr-authentication"></a>Configurar a autenticação do ACR

A autenticação tem de ser configurada entre o cluster AKS e registo ACR. Isto implica conceder à identidade do AKS os direitos adequados para solicitar imagens do registo ACR.

Em primeiro lugar, obtenha o ID do principal de serviço configurado para o AKS. Atualize o nome do grupo de recursos e o nome do cluster AKS para corresponderem ao seu ambiente.

```azurecli
CLIENT_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId" --output tsv)
```

Obtenha o id de recurso do registo ACR. Atualize o nome do registo para o nome do seu registo ACR e o grupo de recursos para o grupo de recursos onde está localizado o registo ACR.

```azurecli
ACR_ID=$(az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv)
```

Crie a atribuição de função, que concede o acesso adequado.

```azurecli
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, um cluster do cluster do Kubernetes foi implementado no AKS. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Implementou um cluster do Kubernetes AKS
> * Instalou a CLI do Kubernetes (kubectl)
> * Configurar kubectl

Avance para o próximo tutorial para saber mais sobre a execução da aplicação no cluster.

> [!div class="nextstepaction"]
> [Implementar aplicação no Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
