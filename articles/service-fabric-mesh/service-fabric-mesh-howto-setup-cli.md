---
title: Configurar a CLI do Azure Service Fabric Mesh | Microsoft Docs
description: Saiba como configurar a CLI do Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: cf899e236b7fa21a90a0784e1e2b4fce0feae055
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213440"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric Mesh
A CLI do Service Fabric Mesh é necessária para implementar e gerir recursos no Service Fabric Mesh. 

Para a pré-visualização, a CLI do Azure Service Fabric Mesh é escrita como uma extensão da CLI do Azure. Pode instalá-la no Azure Cloud Shell ou numa instalação local da CLI do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Se optar por instalar e utilizar a CLI localmente, tem de instalar a versão 2.0.35 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Para instalar ou atualizar a versão mais recente da CLI, veja [Instalar a CLI 2.0 do Azure][azure-cli-install].

Remova qualquer instalação anterior do módulo da CLI do Azure Service Fabric Mesh.

```azurecli-interactive
az extension remove --name mesh
```

Instale o módulo de extensão da CLI do Azure Service Fabric Mesh através do seguinte comando. 

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

Pode também configurar o seu [ambiente de desenvolvimento no Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli