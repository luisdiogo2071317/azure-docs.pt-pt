---
title: Autenticar com o registo de contentor do Azure de instâncias de contentor do Azure
description: Saiba como fornecer acesso a imagens no seu registo de contentor privada das instâncias de contentor do Azure através da utilização de um principal de serviço do Azure Active Directory.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: marsma
ms.openlocfilehash: c68516b46831168bd754fda06ce153f4d79ca741
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Autenticar com o registo de contentor do Azure de instâncias de contentor do Azure

Pode utilizar um principal de serviço do Azure Active Directory (Azure AD) para fornecer acesso aos seus registos do contentor privada no registo de contentor do Azure.

Neste artigo, saiba criar e configurar um principal de serviço do Azure AD com *solicitação* permissões para o registo. Em seguida, inicie um contentor no Azure contentor instâncias (ACI) que obtém a imagem a partir do seu registo privado, o principal de serviço a utilizar para autenticação.

## <a name="when-to-use-a-service-principal"></a>Quando utilizar um principal de serviço

Deve utilizar um principal de serviço para a autenticação de ACI no **cenários sem interface**, tal como em aplicações ou serviços que criar instâncias do contentor de forma automática ou de outra forma automática.

Por exemplo, se tiver um script automatizado que é executada durante a noite e cria um [instância baseada em tarefas contentor](../container-instances/container-instances-restart-policy.md) para processar alguns dados, pode utilizar um principal de serviço com pull-only permissões de (leitor) para autenticar no registo. Em seguida, pode rodar credenciais o principal de serviço ou revogar o acesso totalmente sem afetar outros serviços e aplicações.

Principais de serviço também devem ser utilizado quando o registo [utilizador de admin](container-registry-authentication.md#admin-account) está desativada.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Autenticar com o principal de serviço

Para iniciar um contentor em instâncias de contentor Azure utilizando um principal de serviço, especifique o respetivo ID para `--registry-username`e a palavra-passe para `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Scripts de exemplo

Pode encontrar os scripts de exemplo anterior para a CLI do Azure no GitHub, como bem versões para o Azure PowerShell:

* [CLI do Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Passos Seguintes

Os seguintes artigos contém detalhes adicionais sobre como trabalhar com os principais de serviço e ACR:

* [Autenticação de registo de contentor do Azure com os principais de serviço](container-registry-auth-service-principal.md)
* [Autenticar com o registo de contentor do Azure do serviço de contentor do Azure (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
