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
ms.openlocfilehash: 5e8eaca7f759842a8097184dafc1f3ea183b898c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993192"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric Mesh
A CLI do Service Fabric Mesh é necessária para implementar e gerir recursos no Service Fabric Mesh. 

Para a pré-visualização, a CLI do Azure Service Fabric Mesh é escrita como uma extensão da CLI do Azure. Pode instalá-la no Azure Cloud Shell ou numa instalação local da CLI do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-service-fabric-mesh-cli-locally"></a>Instalar a CLI do Service Fabric Mesh localmente
Se optar por instalar e utilizar a CLI localmente, tem de instalar a versão 2.0.43 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Para instalar ou atualizar para a versão mais recente da CLI, veja [Instalar a CLI do Azure][azure-cli-install].

Instale o módulo de extensão da CLI do Azure Service Fabric Mesh através do seguinte comando. 

```azurecli-interactive
az extension add --name mesh
```

Para atualizar um módulo de CLI do Azure Service Fabric Mesh existente, execute o seguinte comando.

```azurecli-interactive
az extension update --name mesh
```

Pode também configurar o seu [ambiente de desenvolvimento no Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli
