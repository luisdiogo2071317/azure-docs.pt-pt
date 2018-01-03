---
title: "Implementar em instâncias de contentor do Azure a partir do registo de contentor do Azure"
description: "Implementar instâncias de contentor do Azure no registo de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4205b47dc67920021812c1e573a98de64ad198ec
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Implementar em instâncias de contentor do Azure a partir do registo de contentor do Azure

O registo de contentor do Azure é um registo privado baseado no Azure, para imagens de contentor do Docker. Este artigo abrange como implementar imagens do contentor armazenadas no registo de contentor do Azure para instâncias de contentor do Azure.

## <a name="deploy-with-azure-cli"></a>Implementar com o CLI do Azure

A CLI do Azure inclui comandos para criar e gerir contentores em instâncias de contentor do Azure. Se especificar uma imagem de privada no [criar contentor de az] [ az-container-create] comando, também pode especificar o registo imagem palavra-passe necessária para autenticar com o registo de contentor.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

O [criar contentor de az] [ az-container-create] comando também suporta a especificação de `--registry-login-server` e `--registry-username`. No entanto, o servidor de início de sessão para o registo de contentor do Azure está sempre *registryname*. azurecr.io e o nome de utilizador predefinido é *registryname*, por isso, estes valores são inferidos a partir do nome da imagem se não explicitamente fornecido.

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

Para evitar a armazenar a palavra-passe de registo do contentor diretamente no modelo, recomendamos que armazene-lo como um segredo no [Cofre de chaves do Azure](../key-vault/key-vault-manage-with-cli2.md) e referencie-o em modelo, utilizando o [integração nativa entre o Azure Gestor de recursos e o Cofre de chaves](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Implementar com o portal do Azure

Se manter a imagens de contentor no registo de contentor do Azure, pode facilmente criar um contentor em instâncias de contentor do Azure no portal do Azure.

1. No portal do Azure, navegue para o registo de contentor.

2. Selecione **repositórios**, em seguida, selecione o repositório de que pretende implementar, clique com botão direito a etiqueta para a imagem de contentor que pretende implementar e selecione **executar instância**.

    !["Executar a instância" no registo de contentor do Azure no portal do Azure][acr-runinstance-contextmenu]

3. Introduza um nome para o contentor e um nome para o grupo de recursos. Também pode alterar os valores predefinidos se desejar.

    ![Criar o menu para instâncias de contentor do Azure][acr-create-deeplink]

4. Uma vez concluída a implementação, pode navegar para o grupo de contentor do painel de notificações para localizar o respetivo endereço IP e outras propriedades.

    ![Vista de detalhes para o grupo do contentor de instâncias de contentor do Azure][aci-detailsview]

## <a name="next-steps"></a>Passos Seguintes

Saiba como criar contentores, emiti-las para um registo de contentor privada e implementá-las para instâncias de contentor do Azure por [concluir o tutorial](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create