---
title: Configurar a CLI do Azure Service Fabric Mesh | Microsoft Docs
description: Saiba como configurar a CLI do Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 567f2afdea44f439779212c61fb3a129f4f979be
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281577"
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
az extension add --source https://meshcli.blob.core.windows.net/cli/mesh-0.9.1-py2.py3-none-any.whl
```

Pode também configurar o seu [ambiente de desenvolvimento no Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli