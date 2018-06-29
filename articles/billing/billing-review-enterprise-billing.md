---
title: Inscrição de enterprise do Azure de revisão faturação dados com a REST API | Microsoft Docs
description: Saiba como utilizar as APIs REST da Azure para rever informações de faturação de inscrição empresarial.
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
ms.openlocfilehash: 046b2e31aaefa5916a42b3652f9e6a8fdceff367
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064112"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Reveja a faturação de inscrição enterprise com REST APIs

Ajuda de APIs de relatórios do Azure, reveja e gerir os custos do Azure.

Aqui, saiba obter a fatura atual associada um registo de conta de empresa.

Para obter a fatura atual:
``` http
GET https://consumption.azure.com/v2/enrollments/{enrollmentID}/usagedetails
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Criar o pedido  

O `{enrollmentID}` parâmetro é necessário e deve conter o ID de inscrição para a conta de empresa (EA).

Os cabeçalhos seguintes são necessários: 

|Cabeçalho de pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*|Necessário. Definido como `application/json`.|  
|*Autorização:*|Necessário. Definido como um `Bearer` [chave de API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Este exemplo mostra uma chamada síncrona que devolve detalhes para o ciclo de faturação atual. Por motivos de desempenho, chamadas síncronas devolvem informações para o último mês.  Também pode chamar o [API no modo assíncrono](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) devolver dados 36 meses.


## <a name="response"></a>Resposta  

Código de estado 200 (OK) é devolvido para uma resposta com êxito, que contém uma lista dos custos de detalhado para a sua conta.

``` json
{
    "id": "${id}",
    "data": [
        {
            "cost": ${cost}, 
            "departmentId": ${departmentID},
            "subscriptionGuid" : ${subscriptionGuid} 
            "date": "${date}",
            "tags": "${tags}",
            "resourceGroup": "${resourceGroup}"
        } // ...
    ],
    "nextLink": "${nextLinkURL}"
}
```  

Cada item na **dados** representa cobrada uma taxa:

|Propriedade de resposta|Descrição|
|----------------|----------|
|**Custo** | A quantidade cobrada, uma moeda adequada para a localização do Centro de dados. |
|**subscriptionGuid** | ID exclusivo global para a subscrição. | 
|**departmentId** | ID de departamento, se aplicável. |
|**Data** | Data que de encargos foi-lhe faturado. |
|**etiquetas** | Cadeia JSON que contém etiquetas associadas à subscrição. |
|**resourceGroup**|Nome do grupo de recursos que contém o objeto que tarifas o custo. |
|**nextLink**| Se estiver definida, especifica um URL para "página seguinte" dos detalhes. Em branco quando a página é o último. |  
||
  
Os IDs de departamento, grupos de recursos, etiquetas e os campos relacionados são definidos pelo administrador EA.  

Neste exemplo é abreviado; consulte [obter detalhes de utilização](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) para obter uma descrição completa de cada campo de resposta. 

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
