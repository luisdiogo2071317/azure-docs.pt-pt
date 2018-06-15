---
title: Implementar instâncias de contentor do Azure a partir do registo de contentor do Azure
description: Saiba como implementar contentores em instâncias de contentor Azure utilizando as imagens do contentor num registo de contentor do Azure.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 7a7d2aa61f25bc4782c6a1a6744e329935477f8c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32168120"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementar instâncias de contentor do Azure a partir do registo de contentor do Azure

O registo de contentor do Azure é um registo privado, baseado no Azure para as imagens de contentor do Docker. Este artigo descreve como implementar imagens do contentor armazenadas num registo de contentor do Azure para instâncias de contentor do Azure.

## <a name="prerequisites"></a>Pré-requisitos

**Registo de contentor do Azure**: precisará de um registo de contentor do Azure – e a imagem, pelo menos, um contentor no registo – para concluir os passos neste artigo. Se precisar de um registo, consulte [criar um registo de contentor utilizando a CLI do Azure](../container-registry/container-registry-get-started-azure-cli.md).

**CLI do Azure**: os linha de comandos exemplos deste artigo utilizar a [CLI do Azure](/cli/azure/) e são formatados para a shell de deteção. Pode [instalar a CLI do Azure](/cli/azure/install-azure-cli) localmente, ou utilize o [Shell de nuvem do Azure][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Configurar a autenticação de registo

Em qualquer cenário de produção, o acesso a um registo de contentor do Azure deve ser fornecido utilizando [principais de serviço](../container-registry/container-registry-auth-service-principal.md). Principais de serviço permitem-lhe fornecer controlo de acesso baseado em funções para as imagens de contentor. Por exemplo, pode configurar um principal de serviço com pull-only acesso a um registo.

Nesta secção, pode criar um cofre de chaves do Azure e um principal de serviço e armazenar credenciais o principal de serviço no cofre.

### <a name="create-key-vault"></a>Criar cofre de chaves

Se ainda não tiver um cofre [Cofre de chaves do Azure](/azure/key-vault/), criar uma com a CLI do Azure com os seguintes comandos.

Atualização do `RES_GROUP` variável com o nome do grupo de recursos no qual pretende criar o Cofre de chaves, e `ACR_NAME` com o nome do seu registo de contentor. Especifique um nome para o novo cofre de chaves no `AKV_NAME`. O nome do cofre, tem de ser exclusivo no Azure e tem de ser 3 a 24 carateres alfanuméricos de comprimento, começar por uma letra, terminar com uma letra ou dígito e não pode conter hífenes consecutivos.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Criar serviço principal e armazenar credenciais

Agora tem de criar um principal de serviço e armazenar as respetivas credenciais no seu Cofre de chaves.

O seguinte comando utiliza [az ad sp criar-para-rbac] [ az-ad-sp-create-for-rbac] para criar o principal de serviço e [conjunto segredo do keyvault az] [ az-keyvault-secret-set] para armazenar o um principal de serviço **palavra-passe** no cofre.

```azurecli
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

O `--role` argumento no comando anterior configura o principal de serviço com o *leitor* função, o qual concede-pull-only acesso ao registo. Para conceder ambos push e pull acesso, altere o `--role` argumento *contribuinte*.

Em seguida, armazene o principal de serviço *appId* no cofre, que é o **username** passa ao registo de contentor do Azure para a autenticação.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Já criou um cofre de chaves do Azure e armazenados dois segredos na mesma:

* `$ACR_NAME-pull-usr`: O ID de principal de serviço, para utilização como o registo de contentor **username**.
* `$ACR_NAME-pull-pwd`: O serviço principal palavra-passe, para utilização como o registo de contentor **palavra-passe**.

Agora, pode referenciar estes segredos por nome quando a ou as suas aplicações e serviços solicitar imagens a partir do registo.

## <a name="deploy-container-with-azure-cli"></a>Implementar o contentor com a CLI do Azure

Agora que as credenciais de principal de serviço são armazenadas nos segredos do Cofre de chaves do Azure, as suas aplicações e serviços podem utilizá-los para aceder ao seu registo privado.

Execute o seguinte [criar contentor de az] [ az-container-create] comando para implementar uma instância de contentor. O comando utiliza credenciais de um principal de serviço armazenadas no Cofre de chaves do Azure para autenticar para o registo de contentor e parte do princípio de que tiver feito o Push anteriormente o [aci olámundo](container-instances-quickstart.md) imagem ao seu registo. Atualização do `--image` valor se gostaria de utilizar uma imagem diferente a partir do seu registo.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_NAME.azurecr.io/aci-helloworld:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

O `--dns-name-label` valor tem de ser exclusivo no Azure, para que o comando anterior acrescenta um número aleatório a etiqueta de nome DNS do contentor. O resultado do comando apresenta o nome de domínio completamente qualificado do contentor (FQDN), por exemplo:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_NAME.azurecr.io/aci-helloworld:v1 --registry-login-server $ACR_NAME.azurecr.io --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Depois do contentor foi iniciada com êxito, pode navegar para o seu FQDN no seu browser para verificar que a aplicação é executada com êxito.

## <a name="deploy-with-azure-resource-manager-template"></a>Implementar com o modelo Azure Resource Manager

Pode especificar as propriedades do seu registo de contentor do Azure num modelo Azure Resource Manager, incluindo o `imageRegistryCredentials` propriedade na definição de grupo do contentor:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Para obter mais informações de referência segredos do Cofre de chaves do Azure num modelo do Resource Manager, consulte [Cofre de chaves do Azure de utilização para passar o valor do parâmetro seguro durante a implementação](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Implementar com o portal do Azure

Se manter a imagens de contentor no registo de contentor do Azure, pode facilmente criar um contentor em instâncias de contentor do Azure no portal do Azure.

1. No portal do Azure, navegue para o registo de contentor.

1. Selecione **repositórios**, em seguida, selecione o repositório de que pretende implementar, clique com botão direito a etiqueta para a imagem de contentor que pretende implementar e selecione **executar instância**.

    !["Executar a instância" no registo de contentor do Azure no portal do Azure][acr-runinstance-contextmenu]

1. Introduza um nome para o contentor e um nome para o grupo de recursos. Também pode alterar os valores predefinidos se desejar.

    ![Criar o menu para instâncias de contentor do Azure][acr-create-deeplink]

1. Uma vez concluída a implementação, pode navegar para o grupo de contentor do painel de notificações para localizar o respetivo endereço IP e outras propriedades.

    ![Vista de detalhes para o grupo do contentor de instâncias de contentor do Azure][aci-detailsview]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a autenticação de registo de contentor do Azure, consulte [autenticar com um registo de contentor do Azure](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-powershell]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az_container_create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set