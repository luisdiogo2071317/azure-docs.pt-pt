---
title: Os limites de pedido do Azure Resource Manager | Microsoft Docs
description: Descreve como utilizar a limitação com pedidos de Gestor de recursos do Azure quando atingiu limites de subscrição.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2018
ms.author: tomfitz
ms.openlocfilehash: f3dcb0c5036b2cfc38ef2a6a16269a8697bbd9e6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358868"
---
# <a name="throttling-resource-manager-requests"></a>Limitação de pedidos de Gestor de recursos
Para cada subscrição e de inquilino, limites de Gestor de recursos pedidos para 15 000 por hora para ler e escrever pedidos 1,200 por hora. Aplicam estes limites para cada instância do Azure Resource Manager. Existem várias instâncias em cada região do Azure e do Azure Resource Manager é implementado para todas as regiões do Azure.  Para que, na prática, os limites de forma eficaz muito superiores a estes limites, como utilizador pedidos são normalmente servidos por várias instâncias diferentes.

Se a aplicação ou script atinge estes limites, é necessário limitar os pedidos. Este artigo mostra como determinar os pedidos restantes que tiver antes de atingir o limite e como responder quando atingiu o limite.

Quando atingir o limite, receberá o código de estado HTTP **429 demasiados pedidos muitos**.

O número de pedidos é confinado à sua subscrição ou o inquilino. Se tiver vários, aplicações em simultâneo efetuar pedidos na sua subscrição, os pedidos das aplicações são adicionados em conjunto para determinar o número de pedidos restantes.

Pedidos de subscrição no âmbito são aqueles ID involve passar na sua subscrição, tais como obter os grupos de recursos na sua subscrição. Pedidos de um âmbito de inquilino não incluem o ID de subscrição, tais como obter as localizações do Azure válidas.

## <a name="remaining-requests"></a>Pedidos restantes
Pode determinar o número de pedidos restantes, examinando os cabeçalhos de resposta. Cada pedido inclui valores para o número de pedidos de escrita e leitura restantes. A tabela seguinte descreve os cabeçalhos de resposta, pode examinar para esses valores:

| Cabeçalho de resposta | Descrição |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |Um âmbito de subscrição lê restantes. Este valor é devolvido em operações de leitura. |
| x-ms-ratelimit-remaining-subscription-writes |Um âmbito de subscrição escreve restantes. Este valor é devolvido nas operações de escrita. |
| x-ms-ratelimit-remaining-tenant-reads |Um âmbito de inquilino lê restante |
| x-ms-ratelimit-remaining-tenant-writes |Um âmbito de inquilino escreve restante |
| x-ms-ratelimit-remaining-subscription-resource-requests |Subscrição confinadas pedidos de tipo de recurso dos restantes.<br /><br />Este valor de cabeçalho é devolvido apenas se um serviço substituiu o limite predefinido. Gestor de recursos adiciona este valor em vez da subscrição leituras ou escritas. |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |Subscrição de um âmbito restante de pedidos de recolha de tipo de recurso.<br /><br />Este valor de cabeçalho é devolvido apenas se um serviço substituiu o limite predefinido. Este valor fornece o número de pedidos de recolha de restantes (recursos de lista). |
| x-ms-ratelimit-remaining-tenant-resource-requests |Pedidos de tipo de recurso dos restantes de âmbito de inquilino.<br /><br />Este cabeçalho é adicionado apenas para pedidos de nível de inquilino e apenas se um serviço substituiu o limite predefinido. Gestor de recursos adiciona este valor em vez do inquilino leituras ou escritas. |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |Pedidos de recolha de tipo de recurso dos restantes de âmbito de inquilino.<br /><br />Este cabeçalho é adicionado apenas para pedidos de nível de inquilino e apenas se um serviço substituiu o limite predefinido. |

## <a name="retrieving-the-header-values"></a>Obter os valores de cabeçalho
A obter estes valores de cabeçalho no seu código ou script é não diferem obter qualquer valor de cabeçalho. 

Por exemplo, no **c#**, é possível obter o valor do cabeçalho de uma **HttpWebResponse** objeto com o nome **resposta** com o seguinte código:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

No **PowerShell**, é possível obter o valor do cabeçalho de uma operação de Invoke WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Ou, se pretender ver os restantes pedidos para depuração, pode fornecer o **-Debug** parâmetro no seu **PowerShell** cmdlet.

```powershell
Get-AzureRmResourceGroup -Debug
```

Que devolve vários valores, incluindo o seguinte valor de resposta:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
```

Para obter os limites de escrita, utilize uma operação de escrita: 

```powershell
New-AzureRmResourceGroup -Name myresourcegroup -Location westus -Debug
```

Que devolve vários valores, incluindo os seguintes valores:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

No **CLI do Azure**, é possível obter o valor do cabeçalho utilizando a opção mais verbosa.

```azurecli
az group list --verbose --debug
```

Que devolve vários valores, incluindo os seguintes valores:

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '14998'
```

Para obter os limites de escrita, utilize uma operação de escrita: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Que devolve vários valores, incluindo os seguintes valores:

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>A aguardar antes de enviar o pedido seguinte
Quando atingir o limite do pedido, o Gestor de recursos devolve o **429** código de estado HTTP e um **depois de repetir** valor no cabeçalho. O **depois de repetir** valor Especifica o número de segundos deve aguardar a sua aplicação (ou suspensão) antes de enviar o pedido seguinte. Se enviar um pedido antes do valor de repetição expirou, o pedido não é processado e é devolvido um novo valor de repetição.

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre limites e quotas, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
* Para saber mais sobre o processamento assíncronos pedidos REST, consulte [controlar as operações do Azure assíncronas](resource-manager-async-operations.md).
