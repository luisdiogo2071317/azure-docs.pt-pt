---
title: Principais de serviço do Azure Kubernetes Services (AKS)
description: Criar e gerir um principal de serviço do Azure Active Directory para um cluster no Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: ef3139c4b3f06644b219e177fad0c094ed600fb6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394595"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Principais de serviço com o Azure Kubernetes Service (AKS)

Para interagir com as APIs do Azure, os clusters do AKS requerem um [principal de serviço do Azure Active Directory (AD)][aad-service-principal]. O principal de serviço é necessário para criar e gerir dinamicamente outros recursos do Azure como um balanceador de carga ou registo de contentor do Azure (ACR).

Este artigo mostra como criar e utilizar um principal de serviço para os seus clusters do AKS.

## <a name="before-you-begin"></a>Antes de começar

Para criar um principal de serviço do Azure AD, tem de ter permissões para registar uma aplicação no seu inquilino do Azure AD e para atribuir a aplicação a uma função na sua subscrição. Se não tiver as permissões necessárias, poderá ter de pedir ao administrador do Microsoft Azure AD ou da subscrição para atribuir as permissões necessárias ou pré-criar um principal de serviço para utilizar com o cluster do AKS.

Precisa também da versão 2.0.46 ou posterior da CLI do Azure instalada e configurada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Criar e utilizar um principal de serviço automaticamente

Quando criar um cluster do AKS no portal do Azure ou com o comando [az aks create][az-aks-create], o Azure pode gerar automaticamente um principal de serviço.

No exemplo seguinte da CLI do Azure, não está especificado nenhum principal de serviço. Neste cenário, a CLI do Azure cria um principal de serviço para o cluster do AKS. Para concluir esta operação com êxito, a sua conta do Azure tem de ter os direitos adequados para criar um principal de serviço.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Criar um principal de serviço manualmente

Para criar manualmente um principal de serviço com a CLI do Azure, utilize o comando [az ad sp create-for-rbac][az-ad-sp-create]. No exemplo a seguir, o parâmetro `--skip-assignment` impede que sejam atribuídas atribuições predefinidas adicionais:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

O resultado será semelhante ao seguinte exemplo. Tome nota do seu `appId` e `password`. Estes valores serão utilizados quando criar um cluster do AKS na secção seguinte.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Especificar um principal de serviço para um cluster do AKS

Para utilizar um principal de serviço existente quando criar um cluster do AKS com o comando [az aks create][az-aks-create], utilize os parâmetros `--service-principal` e `--client-secret` para especificar `appId` e `password` no resultado do comando [az ad sp create-for-rbac][az-ad-sp-create]:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Se implementar um cluster do AKS com o portal do Azure, na página *Autenticação* da caixa de diálogo **Criar cluster do Kubernetes**, opte por **Configurar principal de serviço**. Selecione **Utilizar existente** e especifique os seguintes valores:

- O **ID de cliente do principal de serviço** é o seu *appId*
- O **Segredo do cliente do principal de serviço** é o valor da *palavra-passe*

![Imagem de navegação para o Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="additional-considerations"></a>Considerações adicionais

Quando utilizar principais de serviço do AKS e do Microsoft Azure AD, tenha em atenção as seguintes considerações.

- O principal de serviço para Kubernetes faz parte da configuração do cluster. No entanto, não utilize a identidade para implementar o cluster.
- Cada principal de serviço está associado a uma aplicação do Azure AD. O principal de serviço de um cluster do Kubernetes pode ser associado a qualquer nome de aplicação do Microsoft Azure AD válido (por exemplo: *https://www.contoso.org/example*). O URL para a aplicação não tem de ser um ponto final real.
- Quando especificar o **ID de Cliente** do principal de serviço, utilize o valor de `appId`.
- Nas VMs de nós e principais no cluster do Kubernetes, as credenciais do principal de serviço são armazenadas no ficheiro `/etc/kubernetes/azure.json`
- Quando utilizar o comando [az aks create][az-aks-create] para gerar automaticamente o principal de serviço, as credenciais do principal de serviço são escritas no ficheiro `~/.azure/aksServicePrincipal.json` no computador utilizado para executar o comando.
- Ao eliminar um cluster do AKS que tenha sido criado pelo [az aks create][az-aks-create], o principal de serviço que foi criado automaticamente não é eliminado.
    - Para eliminar o principal de serviço, obtenha primeiro o ID do mesmo com [az ad app list][az-ad-app-list]. O exemplo seguinte consulta o cluster denominado *myAKSCluster* e, em seguida, elimina o ID de aplicação com [az ad app delete][az-ad-app-delete]. Substitua estes nomes pelos próprios valores:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre os principais de serviço do Azure Active Directory, veja [Objetos de aplicação e de principal de serviço][service-principal]

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
[az-aks-create]: /cli/azure/aks#az-aks-create
