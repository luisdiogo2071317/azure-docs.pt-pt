---
title: Tutorial do Azure Container Service – Implementar Cluster
description: Tutorial do Azure Container Service – Implementar Cluster
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8403e5d8dd3bad07e412b08709dcb8c28201bcdf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434429"
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Implementar um cluster do Kubernetes no Azure Container Service

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

O Kubernetes dispõe de uma plataforma distribuída para aplicações em contentores. Com o Azure Container Service, o aprovisionamento de um cluster Kubernetes pronto para produção é simples e rápido. Neste tutorial, parte 3 de 7, é implementado num cluster de Kubernetes do Azure Container Service. Os passos concluídos incluem:

> [!div class="checklist"]
> * Implementar um cluster do Kubernetes ACS
> * Instalação da CLI do Kubernetes (kubectl)
> * Configuração de kubectl

Nos tutoriais subsequentes, a aplicação Azure Vote é implementada no cluster, dimensionada, atualizada e o Log Analytics é configurado para monitorizar o cluster de Kubernetes.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi criada e carregada uma imagem de contentor para uma instância do Azure Container Registry. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Criar cluster do Kubernetes

Crie um cluster do Kubernetes no Azure Container Service com o comando [az acs create](/cli/azure/acs#az-acs-create). 

O exemplo seguinte cria um cluster com o nome `myK8sCluster` num Grupo de Recursos chamado `myResourceGroup`. Este Grupo de Recursos foi criado no [tutorial anterior](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

Em alguns casos, como numa versão de avaliação limitada, uma subscrição do Azure tem acesso limitado aos recursos do Azure. Caso a implementação falhe devido a um número limitado de núcleos disponíveis, reduza a contagem de agentes pré-definida ao adicionar `--agent-count 1` ao comando [az acs create](/cli/azure/acs#az-acs-create). 

Ao fim de vários minutos, a implementação é concluída e devolve informações sobre a implementação do ACS no formato json.

## <a name="install-the-kubectl-cli"></a>Instalar a CLI do kubectl

Para ligar ao cluster de Kubernetes a partir do computador cliente, utilize [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), o cliente de linha de comandos do Kubernetes. 

Se estiver a utilizar o Azure CloudShell, kubectl já está instalado. Se pretender instalá-lo localmente, utilize o comando [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Se executar no Linux ou no macOS, poderá ter de executar com sudo. No Windows, certifique-se de que a shell foi executada como administrador.

```azurecli-interactive 
az acs kubernetes install-cli 
```

No Windows, a instalação predefinida é *c:\program files (x86)\kubectl.exe*. Terá de adicionar este ficheiro ao caminho do Windows. 

## <a name="connect-with-kubectl"></a>Ligar-se com kubectl

Para configurar kubectl para se ligar ao seu cluster do Kubernetes, execute o comando [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Para verificar a ligação ao cluster, execute o comando [kubectl get nodes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get).

```azurecli-interactive
kubectl get nodes
```

Saída:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Ao concluir o tutorial, tem um cluster de ACS Kubernetes pronto para cargas de trabalho. Nos tutoriais subsequentes, é implementada uma aplicação com vários contentores neste cluster, ampliada, atualizada e monitorizada.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, foi implementado um cluster de Kubernetes do Azure Container Service. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Implementou um cluster do Kubernetes ACS
> * Instalou a CLI do Kubernetes (kubectl)
> * Configurar kubectl

Avance para o próximo tutorial para saber mais sobre a execução da aplicação no cluster.

> [!div class="nextstepaction"]
> [Implementar aplicação no Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
