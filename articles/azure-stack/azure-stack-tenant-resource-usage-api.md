---
title: API de utilização de recursos de inquilino | Documentos da Microsoft
description: Referência de API, a utilização de recursos que obter informações de utilização do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ab5dad550e590cd70f54ad5c8d4727d0f6370190
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379717"
---
# <a name="tenant-resource-usage-api"></a>API de utilização de recursos de inquilino

Um inquilino pode utilizar a API de inquilino para ver os dados de utilização de recursos do inquilino. Esta API é consistente com a API de utilização do Azure (atualmente em pré-visualização privada).

Pode utilizar o cmdlet do Windows PowerShell **Get-UsageAggregates** para obter dados de utilização, como no Azure.

## <a name="api-call"></a>Chamada à API
### <a name="request"></a>Pedir
O pedido obtém os detalhes de consumo para as subscrições de pedido e para o intervalo de tempo pedido. Não existe nenhum corpo de pedido.

| **Método** | **URI do pedido** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumentos
| **Argumento** | **Descrição** |
| --- | --- |
| *armendpoint* |Gestor de recursos ponto final do Azure do seu ambiente do Azure Stack. A Convenção do Azure Stack é o nome do ponto final do Gestor de recursos do Azure está no formato `https://management.{domain-name}`. Por exemplo, crie uma para o kit de desenvolvimento, o nome de domínio é local.azurestack.external, em seguida, o ponto de final do Gestor de recursos é `https://management.local.azurestack.external`. |
| *subId* |ID de subscrição do utilizador que está fazendo a chamada. Pode utilizar esta API apenas a consulta para utilização de uma única subscrição. Fornecedores podem utilizar a API de utilização do fornecedor de recursos para utilização da consulta para todos os inquilinos. |
| *reportedStartTime* |Hora de início da consulta. O valor para *DateTime* deve estar em UTC e no início da hora, por exemplo, 13:00. Para agregação diária, defina este valor como meia-noite de UTC. O formato é *caracteres de escape* ISO 8601, por exemplo, 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, onde a vírgula é escape para % 3a e além de escape para % 2b para que seja amigável de URI. |
| *reportedEndTime* |Hora de fim da consulta. As restrições que se aplicam à *reportedStartTime* também se aplicam a este argumento. O valor para *reportedEndTime* não pode ser no futuro. |
| *aggregationGranularity* |O parâmetro opcional que tem dois valores possíveis discretas: diários e hora a hora. Como os valores sugerem, um devolve os dados na granularidade diária e o outro é uma resolução por hora. A opção diária é a predefinição. |
| *api-version* |Versão do protocolo que é utilizado para efetuar este pedido. Tem de utilizar 2015-06-01-preview. |
| *continuationToken* |Token obtido a partir da última chamada para o fornecedor de API de utilização. Este token é necessário quando uma resposta for superior a 1000 linhas e atua como um indicador de progresso. Se não estiver presente, os dados são recuperados desde o início do dia ou hora, com base na granularidade passado. |

### <a name="response"></a>Resposta
GET /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&api-version=1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",
"name": "sub1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Detalhes da resposta
| **Argumento** | **Descrição** |
| --- | --- |
| *id* |ID exclusivo do agregado de utilização |
| *name* |Nome do agregado de utilização |
| *tipo* |Definição do recurso |
| *subscriptionId* |Identificador de subscrição do utilizador do Azure |
| *usageStartTime* |Hora de registo de utilização a que pertence este agregado de utilização de início de UTC |
| *usageEndTime* |Hora de fim de UTC de registo de utilização a que pertence este agregado de utilização |
| *instanceData* |Pares de chave-valor dos detalhes de instância (num novo formato):<br>  *resourceUri*: completamente qualificado do ID de recurso, incluindo grupos de recursos e o nome da instância <br>  *localização*: região em que este serviço foi executado <br>  *etiquetas*: etiquetas de recursos especificadas pelo utilizador <br>  *additionalInfo*: obter mais detalhes sobre o recurso que foi consumido, por exemplo, tipo de imagem ou de versão do SO |
| *quantity* |Quantidade de consumo de recursos que ocorreram neste período de tempo |
| *meterId* |ID exclusivo para o recurso que foi consumido (também denominado *ResourceID*) |


## <a name="next-steps"></a>Passos Seguintes
[API de utilização do recurso do fornecedor](azure-stack-provider-resource-api.md)

[Perguntas Freqüentes relacionadas com a utilização](azure-stack-usage-related-faq.md)

