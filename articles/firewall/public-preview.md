---
title: Ativar a pré-visualização pública do Firewall do Azure
description: Utilizar o Azure PowerShell para ativar a pré-visualização pública do Firewall do Azure
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 263b16a419b5ff20a9b6d62860385f92c2a18f9c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991454"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Ativar a pré-visualização pública do Firewall do Azure

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Ativar com o Azure PowerShell

Para ativar a pré-visualização pública do Firewall do Azure, utilize os seguintes comandos do Azure PowerShell:

```PowerShell
Register-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Register-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Demora até 30 minutos para o registo de funcionalidade concluir. Pode verificar o estado de registo ao executar os seguintes comandos do Azure PowerShell:

```PowerShell

Get-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Após o registo estiver concluído, execute o seguinte comando:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Passos Seguintes

- [Tutorial: Implementar e configurar a Firewall do Azure no portal do Azure](tutorial-firewall-deploy-portal.md)

