---
title: Reveja os dados de faturação da subscrição do Azure com a REST API | Microsoft Docs
description: Saiba como utilizar as APIs REST da Azure para rever detalhes de faturação da subscrição.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: cc29d1f613af67604d50654be794cc90080098bb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063923"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Reveja a faturação da subscrição através de REST APIs

Ajuda de APIs de relatórios do Azure, reveja e gerir os custos do Azure.  

Filtros de ajudam a personalizar os resultados para as suas necessidades.

Aqui, que aprender a utilizar uma API REST para devolver os detalhes de faturação da subscrição para um intervalo de datas especificado.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Criar o pedido  

O `{subscriptionID}` parâmetro é necessário e identifica a subscrição de destino.

O `{billingPeriod}` parâmetro é necessário e especifica um atual [período faturação](https://docs.microsoft.com/rest/api/billing/billingperiods/get#billingperiod).

O `${startDate}` e `${endDate}` parâmetros são necessários para este exemplo, mas opcional para o ponto final.  Se especificarem o intervalo de datas como cadeias no formato AAAA-MM-DD (exemplos: `'20180501'` e `'20180615'`). 

Os cabeçalhos seguintes são necessários: 

|Cabeçalho de pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*|Necessário. Definido como `application/json`.|  
|*Autorização:*|Necessário. Definido como um `Bearer` [token de acesso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Resposta  

Código de estado 200 (OK) é devolvido para uma resposta com êxito, que contém uma lista dos custos de detalhado para a sua conta.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": ${usageQuantity},
        "billableQuantity": ${billableQuantity},
        "pretaxCost": ${cost},
        "meterId": "${meterID}",
        "meterDetails": ${meterDetails}
      }
    }
    ],
    "nextLink": "${nextLinkURL}"
} 
```  

Cada item na **valor** representa um detalhes sobre a utilização de um serviço:

|Propriedade de resposta|Descrição|
|----------------|----------|
|**subscriptionGuid** | ID exclusivo global para a subscrição. | 
|**startDate** | Data a utilização iniciada. |
|**endDate** | Data a utilização terminada. |
|**useageQuantity** | Quantidade utilizada. | 
|**billableQuantity** | Quantidade realmente cobrada. |
|**pretaxCost** | Custo invoiced, antes de taxas aplicáveis. | 
|**meterDetails** | Obter informações detalhadas sobre a utilização. |
|**nextLink**| Se estiver definida, especifica um URL para "página seguinte" dos detalhes. Em branco quando a página é o último. |  
||
  
Neste exemplo é abreviado; consulte [lista detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails/listbybillingperiod#usagedetailslistresult) para obter uma descrição completa de cada campo de resposta. 

Outros códigos de estado indicam condições de erro. Nestes casos, o objeto de resposta explica por que motivo o pedido falhou.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Passos Seguintes 
- Reveja [Enterprise reporting descrição geral](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Investigar [Enterprise REST API de faturação](https://docs.microsoft.com/rest/api/billing/)   
- [Introdução à API REST do Azure](https://docs.microsoft.com/rest/api/azure/)   
