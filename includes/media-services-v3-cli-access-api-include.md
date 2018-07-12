---
title: incluir ficheiro
description: incluir ficheiro
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/29/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 7454e96a2a05bf89a0455674a4f144534c374c71
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733298"
---
## <a name="access-the-media-services-api"></a>Aceder à API dos Serviços de Multimédia

Para ligar às APIs dos Serviços de Multimédia do Azure, utilize a autenticação do principal de serviço do Azure AD. O comando seguinte cria uma aplicação do Azure AD e anexa um principal de serviço à conta. Deve utilizar os valores devolvidos para configurar a aplicação.

Antes de executar o script, pode substituir o `amsaccount` e o `amsResourceGroup` pelos nomes que escolheu ao criar estes recursos. `amsaccount` é o nome da conta dos Serviços de Multimédia do Azure à qual o principal de serviço será anexado.

O seguinte comando devolve uma saída `json`:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Este comando produz uma resposta semelhante à seguinte:

```json
{
  "AadClientId": "00000000-4cdd-418a-8a72-0755ace03de5",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-02f5-4bf2-9057-1c4f7baff155",
  "AadTenantId": "00000000-86f1-41af-91ab-2d7cd011db47",
  "AccountName": "amsaccount22",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup2",
  "SubscriptionId": "00000000-6753-4ca2-b1ae-193798e2c9d8"
}
```

Se pretender obter `xml` na resposta, utilize o seguinte comando:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
