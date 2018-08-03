---
title: Implementar no Azure Container Instances do registo de contentor do Azure
description: Saiba como implementar contentores no Azure Container Instances com imagens de contentor num Azure container registry.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 5c3cf162caf5cf9aa88b012257d4caab37b7893c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424216"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementar no Azure Container Instances do registo de contentor do Azure

O Azure Container Registry é um registo privado e baseado no Azure para as imagens de contentor do Docker. Este artigo descreve como implementar imagens de contentor armazenadas num Azure container registry no Azure Container Instances.

## <a name="prerequisites"></a>Pré-requisitos

**O Azure Container Registry**: precisa de um registo de contentor do Azure – e a imagem de contentor, pelo menos, um registro – para concluir os passos neste artigo. Se precisar de um registo, veja [criar um registo de contentor com a CLI do Azure](../container-registry/container-registry-get-started-azure-cli.md).

**CLI do Azure**: os linha de comando exemplos neste artigo utilizam o [CLI do Azure](/cli/azure/) e são formatados para o shell de Bash. Pode [instalar a CLI do Azure](/cli/azure/install-azure-cli) localmente, ou utilize o [Azure Cloud Shell][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Configurar a autenticação do registo

Em qualquer cenário de produção, o acesso a um Azure container registry deve ser fornecido com o [principais de serviço](../container-registry/container-registry-auth-service-principal.md). Os principais de serviço permitem-lhe ter controlo de acesso baseado em funções sobre as imagens de contentor. Por exemplo, pode configurar um principal de serviço com acesso a um registo apenas por pedido.

Nesta secção, crie um cofre de chave do Azure e um principal de serviço e armazenar credenciais do principal de serviço no cofre.

### <a name="create-key-vault"></a>Criar o cofre de chaves

Se ainda não tiver um cofre no [Azure Key Vault](/azure/key-vault/), crie um com a CLI do Azure através dos comandos seguintes.

Atualização do `RES_GROUP` variável com o nome do grupo de recursos no qual pretende criar o Cofre de chaves, e `ACR_NAME` com o nome do seu registo de contentor. Especifique um nome para o novo cofre de chaves no `AKV_NAME`. O nome do cofre tem de ser exclusivo no Azure e tem de ser 3 e 24 carateres de alfanuméricos de comprimento, começar com uma letra, terminar com uma letra ou dígito e não pode conter hífenes consecutivos.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Criar o principal de serviço e armazenar as credenciais

Agora tem de criar um principal de serviço e armazenar as credenciais no cofre de chaves.

O seguinte comando utiliza [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] para criar o principal de serviço, e [conjunto segredo do keyvault az] [ az-keyvault-secret-set] para armazenar o principal de serviço **palavra-passe** no cofre.

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

O argumento `--role` no comando anterior configura o principal de serviço com a função de *leitor*, a qual concede acesso ao registo apenas por pedido. Para conceder acesso por pedido e por envio, altere o argumento `--role` para *contribuinte*.

Em seguida, armazenar o principal de serviço *appId* no cofre, que é o **username** passar para o Azure Container Registry para autenticação.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Acabou de criar um Azure Key Vault e de armazenar dois segredos nele:

* `$ACR_NAME-pull-usr`: o ID do principal de serviço, para utilização como o **nome de utilizador** do registo de contentor.
* `$ACR_NAME-pull-pwd`: a palavra-passe do principal de serviço, para utilização como a **palavra-passe** do registo de contentor.

Agora, pode referenciar estes segredos por nome quando você ou as suas aplicações e serviços solicitarem imagens do registo.

## <a name="deploy-container-with-azure-cli"></a>Implementar o contentor com a CLI do Azure

Agora que as credenciais do principal de serviço são armazenadas em segredos do Cofre de chaves do Azure, as aplicações e serviços podem utilizá-los para aceder ao seu registo privado.

Execute o comando seguinte [az container create][az-container-create] para implementar uma instância de contentor. O comando utiliza credenciais do principal de serviço armazenadas no Azure Key Vault para autenticar para o seu registo de contentor e parte do princípio de que anteriormente enviou a [aci-helloworld](container-instances-quickstart.md) imagem para o seu registo. Atualização do `--image` valor se gostaria de utilizar uma imagem diferente a partir do registo.

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

O `--dns-name-label` valor tem de ser exclusivo no Azure, para que o comando anterior acrescenta um número aleatório, a etiqueta de nome DNS do contentor. O resultado do comando apresenta o nome de domínio completamente qualificado (FQDN) do contentor, por exemplo:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_NAME.azurecr.io/aci-helloworld:v1 --registry-login-server $ACR_NAME.azurecr.io --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Depois do contentor foi iniciado com êxito, pode navegar para o seu FQDN no seu browser para verificar que a aplicação está em execução com êxito.

## <a name="deploy-with-azure-resource-manager-template"></a>Implementar com o modelo Azure Resource Manager

Pode especificar as propriedades do seu registo de contentor do Azure num modelo do Azure Resource Manager, incluindo o `imageRegistryCredentials` propriedade na definição de grupo do contentor:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Para obter detalhes sobre a referenciar segredos do Cofre de chaves do Azure num modelo do Resource Manager, consulte [do Azure com o Key Vault para transmitir o valor do parâmetro segura durante a implementação](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Implementar com o portal do Azure

Se manter imagens de contentor no Azure Container Registry, pode facilmente criar um contentor no Azure Container Instances com o portal do Azure.

1. No portal do Azure, navegue para o seu registo de contentor.

1. Selecione **repositórios**, em seguida, selecione o repositório que pretende implementar a partir de, com o botão direito a etiqueta da imagem de contentor que pretende implementar e selecione **executar instância**.

    !["Executar instância" no Azure Container Registry no portal do Azure][acr-runinstance-contextmenu]

1. Introduza um nome para o contentor e um nome para o grupo de recursos. Também pode alterar os valores predefinidos se desejar.

    ![Criar o menu do Azure Container Instances][acr-create-deeplink]

1. Depois de concluída a implementação, pode navegar para o grupo de contentores a partir do painel de notificações para localizar o respetivo endereço IP e outras propriedades.

    ![Vista de detalhes para o grupo de contentores do Azure Container Instances][aci-detailsview]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a autenticação do Azure Container Registry, veja [autenticar com um Azure container registry](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-powershell]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set