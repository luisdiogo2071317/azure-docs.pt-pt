---
title: "Principal de serviço do cluster de Kubernetes do Azure"
description: "Criar e gerir um principal de serviço do Azure Active Directory para um cluster de Kubernetes no AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9814dca53f1a410f4d1e95cc18b98373f27f9802
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Principais de serviço com o Azure Container Service (AKS)

Um cluster do AKS requer um [principal de serviço do Azure Active Directory][aad-service-principal] para interagir com APIs do Azure. O principal de serviço tem de gerir dinamicamente recursos, como [rotas definidas pelo utilizador][user-defined-routes] e o [Balanceador de Carga do Azure de Camada 4][azure-load-balancer-overview].

Este artigo mostra as diferentes opções para configurar um principal de serviço para o cluster de Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar


Para criar um principal de serviço do Azure AD, tem de ter permissões para registar uma aplicação no seu inquilino do Azure AD e para atribuir a aplicação a uma função na sua subscrição. Se não tiver as permissões necessárias, poderá ter de pedir ao administrador do Azure AD ou da subscrição para atribuir as permissões necessárias ou pré-criar um principal de serviço para o cluster de Kubernetes.

Precisa também da versão 2.0.21 da CLI do Azure ou posterior instalada e configurada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="create-sp-with-aks-cluster"></a>Criar o SP com o cluster do AKS

Quando implementar um cluster do AKS com o comando `az aks create`, tem a opção de gerar automaticamente um principal de serviço.

No exemplo seguinte, é criado um cluster do AKS e, por não existir um principal de serviço especificado, é criado um principal de serviço para o cluster. Para concluir esta operação, a sua conta tem de ter os direitos adequados para criar um principal de serviço.

```azurecli
az aks create --name myK8SCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Utilizar um SP existente

Pode ser utilizado ou pré-criado um principal de serviço existente do Azure AD para utilização num cluster do AKS. Isto é útil ao implementar um cluster a partir do portal do Azure, onde tem de fornecer as informações do principal de serviço. Quando utilizar um principal de serviço existente, o segredo do cliente tem de ser configurado como uma palavra-passe.

## <a name="pre-create-a-new-sp"></a>Pré-criar um novo SP

Para criar o principal de serviço com a CLI do Azure, utilize o comando [az ad sp create-for-rbac][az-ad-sp-create].

```azurecli
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
az aks create --resource-group myResourceGroup --name myK8SCluster --service-principal <appId> --client-secret <password>
```

Se estiver a implementar um cluster AKS utilizando o portal do Azure, introduza o `appId` valor no campo **ID de cliente do principal de serviço** e o campo `password` valor o **Segredo de cliente do principal de serviço** no formulário de configuração do cluster AKS.

![Imagem de navegação para o Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Considerações adicionais

Quando utilizar principais de serviço do AKS e do Azure AD, tenha em atenção o seguinte.

* O principal de serviço para Kubernetes faz parte da configuração do cluster. No entanto, não utilize a identidade para implementar o cluster.
* Cada principal de serviço está associado a uma aplicação do Azure AD. O principal de serviço para um cluster de Kubernetes pode ser associado a qualquer nome de aplicação do Azure AD válido (por exemplo: `https://www.contoso.org/example`). O URL para a aplicação não tem de ser um ponto final real.
* Quando especificar o principal de serviço **ID de Cliente**, pode utilizar o valor do `appId` (como mostrado neste artigo) ou o principal de serviço correspondente `name` (por exemplo, `https://www.contoso.org/example`).
* Em VMs do nó e mestras no cluster de Kubernetes, as credenciais do principal de serviço são armazenadas no ficheiro `/etc/kubernetes/azure.json`.
* Quando utilizar o comando `az aks create` para gerar automaticamente o principal de serviço, as credenciais do principal de serviço são escritas no ficheiro `~/.azure/acsServicePrincipal.json` na máquina utilizada para executar o comando.
* Quando utiliza o comando `az aks create` para gerar automaticamente o principal de serviço, o principal de serviço também pode autenticar com um [Azure container registry][acr-intro] criado na mesma subscrição.
* Ao eliminar um cluster do AKS que tenha sido criado pelo `az aks create`, o principal de serviço que foi criado automaticamente não é eliminado. Pode utilizar `az ad sp delete --id $clientID` para eliminá-lo.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre principais de serviço do Azure Active Directory, veja a documentação de aplicações do Azure AD.

> [!div class="nextstepaction"]
> [Objetos de aplicação e de principal de serviço][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]: ../active-directory/develop/active-directory-application-objects.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]: ../active-directory/develop/active-directory-application-objects.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md