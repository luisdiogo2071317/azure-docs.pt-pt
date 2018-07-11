---
title: Tutorial do Kubernetes no Azure – Implementar Cluster
description: Tutorial de AKS – Implementar Cluster
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8698f16138e9baeb9c9c1142a5d0c8937a69d1b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341404"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Tutorial: Implementar um cluster do Serviço Kubernetes do Azure (AKS)

O Kubernetes dispõe de uma plataforma distribuída para aplicações em contentores. Com o AKS, pode aprovisionar rapidamente um cluster de Kubernetes pronto para produção. Neste tutorial, parte três de sete, é implementado um cluster do Kubernetes no AKS. Os passos concluídos incluem:

> [!div class="checklist"]
> * Criação de um principal de serviço para interações de recursos
> * Implementar um cluster do Kubernetes AKS
> * Instalação da CLI do Kubernetes (kubectl)
> * Configuração de kubectl

Nos tutoriais seguintes, a aplicação Azure Vote é implementada no cluster, dimensionada e atualizada.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi criada e carregada uma imagem de contentor para uma instância do Azure Container Registry. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="create-a-service-principal"></a>Criar um principal de serviço

Para permitir que um cluster do AKS interaja com outros recursos do Azure, é utilizado um principal de serviço do Azure Active Directory. Este principal de serviço pode ser criado automaticamente com a CLI do Azure ou o portal, ou pode criar previamente um e atribuir permissões adicionais. Neste tutorial, vai criar um principal de serviço, conceder acesso à instância do Azure Container Registry (ACR) criado no tutorial anterior e, em seguida, criar um cluster do AKS.

Criar um principal de serviço com [az ad sp create-for-rbac][]. O parâmetro `--skip-assignment` limita a atribuição de permissões adicionais.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

O resultado é semelhante ao seguinte exemplo:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Anote o *appId* e a *palavra-passe*. São utilizados os seguintes valores nos passos seguintes.

## <a name="configure-acr-authentication"></a>Configurar a autenticação do ACR

Para aceder a imagens armazenadas no ACR, tem de conceder os direitos corretos ao principal de serviço do AKS para solicitar imagens do ACR.

Primeiro, obtenha o ID do recurso do ACR com [az acr show][]. Atualize o nome do registo `<acrName>` para o nome da sua instância do ACR e o grupo de recursos onde está localizada a instância do ACR.

```azurecli
az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv
```

Para conceder o acesso correto ao cluster do AKS para este utilizar imagens armazenadas no ACR, crie uma atribuição de função com [az role assignment create][]. Substitua `<appId`> e `<acrId>` pelos valores recolhidos nos dois passos anteriores.

```azurecli
az role assignment create --assignee <appId> --role Reader --scope <acrId>
```

## <a name="create-kubernetes-cluster"></a>Criar cluster do Kubernetes

Agora, crie um cluster do AKS com [az aks create][]. O exemplo seguinte cria um cluster com o nome *myAKSCluster* num grupo de recursos com o nome *myResourceGroup*. Este grupo de recursos foi criado no [tutorial anterior][aks-tutorial-prepare-acr]. Indique o seu próprio `<appId>` e `<password>` do passo anterior, onde criou o principal de serviço.

```azurecli
az aks create \
    --name myAKSCluster \
    --resource-group myResourceGroup \
    --node-count 1 \
    --generate-ssh-keys \
    --service-principal <appId> \
    --client-secret <password>
```

Ao fim de vários minutos, a implementação é concluída e devolve informações sobre a implementação do AKS no formato JSON.

## <a name="install-the-kubectl-cli"></a>Instalar a CLI do kubectl

Para ligar ao cluster de Kubernetes a partir do computador cliente, utilize [kubectl][kubectl], o cliente de linha de comandos do Kubernetes.

Se utilizar o Azure Cloud Shell, o kubectl já está instalado. Também pode instalá-lo localmente com [az aks install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Ligar-se com kubectl

Utilize [az aks get-credentials][] para configurar o kubectl para ligar ao cluster Kubernetes. O exemplo seguinte obtém credenciais para o nome de cluster do AKS *myAKSCluster* no *myResourceGroup*:

```azurecli
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

Para verificar a ligação ao cluster, execute o comando [kubectl get nodes][kubectl-get].

```azurecli
kubectl get nodes
```

Saída:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.6
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, um cluster do cluster do Kubernetes foi implementado no AKS. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Criou um principal de serviço para interações de recursos
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
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
