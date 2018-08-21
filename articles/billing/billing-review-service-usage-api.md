---
title: Reveja a utilização de recursos de serviço do Azure com a REST API | Documentos da Microsoft
description: Saiba como utilizar APIs REST do Azure para rever a utilização de recursos de serviço do Azure.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alleonar
ms.openlocfilehash: 1b7b1455413fb4886b317d468e6d278111c094b1
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40225961"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>Reveja a utilização de recursos do Azure com a API REST


Azure [APIs de consumo](https://docs.microsoft.com/rest/api/consumption/) ajuda examinar os dados de custos e utilização para os seus recursos do Azure.

Neste artigo, irá aprender a obter e agregar informações de utilização de recursos para os recursos num grupo de recursos do Azure, bem como a forma como filtrar esses resultados com base na [etiquetas do Gestor de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

## <a name="get-usage-for-a-resource-group"></a>Obter a utilização de um grupo de recursos

Para obter a utilização de recursos de computação, base de dados e outros recursos num grupo de recursos, utilize o `usageDetails` operação REST e filtrar os resultados por grupo de recursos.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30&filter=properties/resourceGroup eq '{resource-group}]
Content-Type: application/json   
Authorization: Bearer
```

O `{subscription-id}` parâmetro é obrigatório e deve conter um ID de subscrição que pode acessar o grupo de recursos de {resource-group} com uma função de leitor. 

Os seguintes cabeçalhos são necessários: 

|Cabeçalho do pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*| Necessário. Definido como `application/json`. |  
|*Autorização:*| Necessário. Definido como válido `Bearer` token. |

### <a name="response"></a>Resposta  

É devolvido o código de estado 200 (OK) para uma resposta com êxito, que contém uma lista de estatísticas de utilização para cada recurso do Azure no grupo de recursos com o ID de subscriptipon `00000000-0000-0000-0000-000000000000`.

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    } ] }
```

## <a name="get-usage-for-tagged-resources"></a>Obter a utilização de recursos com etiquetas

Para obter a utilização de recursos para recursos no organizados por etiquetas, utilizam o `usageDetails` operação REST e filtrar os resultados com o nome de etiqueta a `$filter` parâmetro de consulta.

```http
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Consumption/usageDetails?$filter=tags eq 'tag1'&api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

O `{subscription-id}` parâmetro é obrigatório e deve conter um ID de subscrição que pode aceder os recursos com etiquetas.


### <a name="response"></a>Resposta  

É devolvido o código de estado 200 (OK) para uma resposta com êxito, que contém uma lista de estatísticas de utilização para cada recurso do Azure no grupo de recursos com o ID de subscriptipon `00000000-0000-0000-0000-000000000000` e é par de Cofre de chaves de nome de etiqueta `dev` e `tools`. 

Resposta de exemplo:

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "tags": {
        "dev": "tools"
      },
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes
- [Introdução à API REST do Azure](https://docs.microsoft.com/rest/api/azure/)   
