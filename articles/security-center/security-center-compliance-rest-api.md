---
title: Monitoramento de conformidade de centro de segurança do Azure com a API REST | Documentos da Microsoft
description: Rever os resultados das verificações de conformidade automatizada usando a API de REST do Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 651d7737258f1b1b17c8392a09882388ddf95635
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819881"
---
# <a name="review-security-center-compliance-results-using-the-azure-rest-apis"></a>Reveja os resultados de compatibilidade do Centro de segurança com as APIs de REST do Azure

Neste artigo, vai aprender a obter as informações de conformidade de segurança para obter uma lista de subscrições com as APIs de REST do Azure.

## <a name="review-compliance-for-each-subscription"></a>Rever a conformidade para cada subscrição

O exemplo abaixo obtém informações de avaliação de segurança para um determinado conformidade e a subscrição a utilizar o [regulamentares obter](/rest/api/securitycenter/compliances/get) operação.

```http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/compliances/{complianceName}?api-version=2017-08-01-preview
```

O `{complianceName}` parâmetro é obrigatório e deve conter o nome da conformidade avaliada para `{subscription-id}`. Saída irá conter os resultados da avaliação, tais como:

```json
{
...
  "properties": {
    "assessmentResult": [
      {
        "segmentType": "Compliant",
        "percentage": 77.777777777777786
      }
    ],
    "resourceCount": 18,
    "assessmentTimestampUtcDate": "2018-01-01T00:00:00Z"
  }
}
```

## <a name="review-compliance-for-multiple-subscriptions"></a>Rever a conformidade para várias subscrições

Para obter dados de várias subscrições, fazer a chamada abaixo primeiro obter uma lista das inscrições com o [subscrições de listas de](/rest/api/resources/subscriptions/list) operação. Invoke acima [regulamentares obter](/rest/api/securitycenter/compliances/get) para cada um dos IDs de subscrição retornado.

A chamada de API é:

```http
GET https://management.azure.com/subscriptions?api-version=2016-06-01
```

Que retorna uma matriz de com valores como o seguinte. Utilize os valores de subscriptionId na chamada acima para rever as informações de conformidade para todas as subscrições.

```json
"value": [
    {
      "id": "/subscriptions/{subscription-id}",
      "subscriptionId": "{subscription-id}",
      "displayName": "Demo subscription",
      ...
    }
```






