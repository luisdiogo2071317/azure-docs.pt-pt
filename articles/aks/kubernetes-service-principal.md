---
title: Principal de serviço do cluster de Kubernetes do Azure
description: Criar e gerir um principal de serviço do Azure Active Directory para um cluster de Kubernetes no AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: get-started-article
ms.date: 04/19/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4ad0fc3fdb7d5b7c14f13fd6c279915974558dc9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578801"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Principais de serviço com o Serviço Kubernetes do Azure (AKS)

Um cluster do AKS requer um [principal de serviço do Azure Active Directory][aad-service-principal] para interagir com APIs do Azure. O principal de serviço tem de criar e gerir dinamicamente recursos como o [Balanceador de Carga do Azure ][azure-load-balancer-overview].

Este artigo mostra as diferentes opções para configurar um principal de serviço para o cluster de Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar


Para criar um principal de serviço do Azure AD, tem de ter permissões para registar uma aplicação no seu inquilino do Azure AD e para atribuir a aplicação a uma função na sua subscrição. Se não tiver as permissões necessárias, poderá ter de pedir ao administrador do Azure AD ou da subscrição para atribuir as permissões necessárias ou pré-criar um principal de serviço para o cluster de Kubernetes.

Precisa também da versão 2.0.27 da CLI do Azure ou posterior instalada e configurada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="create-sp-with-aks-cluster"></a>Criar o SP com o cluster do AKS

Quando implementar um cluster do AKS com o comando `az aks create`, tem a opção de gerar automaticamente um principal de serviço.

No exemplo seguinte, é criado um cluster do AKS e, por não existir um principal de serviço especificado, é criado um principal de serviço para o cluster. Para concluir esta operação, a sua conta tem de ter os direitos adequados para criar um principal de serviço.

```azurecli-interactive
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Utilizar um SP existente

Pode ser utilizado ou pré-criado um principal de serviço existente do Azure AD para utilização num cluster do AKS. Isto é útil ao implementar um cluster a partir do Portal do Azure, onde tem de fornecer as informações do principal de serviço. Quando utilizar um principal de serviço existente, o segredo do cliente tem de ser configurado como uma palavra-passe.

## <a name="pre-create-a-new-sp"></a>Pré-criar um novo SP

Para criar o principal de serviço com a CLI do Azure, utilize o comando [az ad sp create-for-rbac][az-ad-sp-create].

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

O resultado é semelhante ao seguinte. Tome nota do `appId` e da `password`. Estes valores são utilizados quando criar um cluster do AKS.

```json
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Utilizar um SP existente

Quando utilizar um principal de serviço pré-criado, forneça o `appId` e a `password` como valores de argumento para o comando `az aks create`.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --service-principal <appId> --client-secret <password>
```

Se estiver a implementar um cluster AKS utilizando o Portal do Azure, introduza o `appId` valor no campo **ID de cliente do principal de serviço** e o campo `password` valor o **Segredo de cliente do principal de serviço** no formulário de configuração do cluster AKS.

![Imagem de navegação para o Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Considerações adicionais

Quando utilizar principais de serviço do AKS e do Azure AD, tenha em atenção o seguinte.

* O principal de serviço para Kubernetes faz parte da configuração do cluster. No entanto, não utilize a identidade para implementar o cluster.
* Cada principal de serviço está associado a uma aplicação do Azure AD. O principal de serviço para um cluster de Kubernetes pode ser associado a qualquer nome de aplicação do Azure AD válido (por exemplo: `https://www.contoso.org/example`). O URL para a aplicação não tem de ser um ponto final real.
* Quando especificar o **ID de Cliente**, do principal de serviço, utilize o valor de `appId`.
* Em VMs do nó e mestras no cluster de Kubernetes, as credenciais do principal de serviço são armazenadas no ficheiro `/etc/kubernetes/azure.json`.
* Quando utilizar o comando `az aks create` para gerar automaticamente o principal de serviço, as credenciais do principal de serviço são escritas no ficheiro `~/.azure/aksServicePrincipal.json` na máquina utilizada para executar o comando.
* Ao eliminar um cluster do AKS que tenha sido criado pelo `az aks create`, o principal de serviço que foi criado automaticamente não é eliminado. Para eliminar o principal de serviço, obtenha primeiro o ID do mesmo com [az ad app list][az-ad-app-list]. O exemplo seguinte consulta o cluster denominado *myAKSCluster* e, em seguida, elimina o ID de aplicação com [az ad app delete][az-ad-app-delete]. Substitua estes nomes pelos próprios valores:

    ```azurecli-interactive
    az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
    az ad app delete --id <appId>
    ```

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre principais de serviço do Azure Active Directory, veja a documentação de aplicações do Azure AD.

> [!div class="nextstepaction"]
> [Objetos de aplicação e de principal de serviço][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
