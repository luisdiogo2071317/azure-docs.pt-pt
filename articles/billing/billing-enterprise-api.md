---
title: Azure de faturação Enterprise APIs | Microsoft Docs
description: Saiba mais sobre as APIs do Reporting Services que permitem que os clientes empresariais Azure solicitar dados de consumo através de programação.
services: ''
documentationcenter: ''
author: anandedwin
manager: aedwin
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 7ae04c7c1fdddecf40583d14f7b08dcf41941c6f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Descrição geral de APIs de relatórios para os clientes empresariais
As APIs do Reporting Services permitem que os clientes empresariais Azure programaticamente extraia consumo e dados de faturação para ferramentas de análise de dados preferencial. 

## <a name="enabling-data-access-to-the-api"></a>Ativar o acesso de dados para a API
* **Gerar ou obter a chave de API** -inicie sessão no portal da empresa e navegue até aos relatórios > Transferir utilização > chave de acesso da API para gerar ou obter a chave de API.
* **Transmissão de chaves na API** -chave de API a deve ser transmitido para cada chamada para autenticação e autorização. A seguinte propriedade tem de ser para os cabeçalhos HTTP

|Chave do cabeçalho de pedido | Valor|
|-|-|
|Autorização| Especifique o valor neste formato: **portador {API_KEY}** <br/> Exemplo: eyr de portador... 09|

## <a name="consumption-apis"></a>APIs de consumo
Um ponto final de Swagger está disponível [aqui](https://consumption.azure.com/swagger/ui/index) para as APIs descrito abaixo, que deverá ativar introspection fácil da API e a capacidade de gerar SDKs do cliente utilizando [AutoRest](https://github.com/Azure/AutoRest) ou [Swagger CodeGen](http://swagger.io/swagger-codegen/). Dados a partir de 1 de Maio de 2014 estão disponíveis através desta API. 

* **Saldo e resumo** - [saldo e API resumo](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) oferece um resumo das informações no saldos, compras novo, os encargos de serviço do Azure Marketplace, ajustes e custos excedidos mensal.

* **Detalhes de utilização** - [API de detalhes de utilização](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) oferece uma divisão diária dessas quantidades foram consumidas e custos estimados por de inscrição. O resultado também inclui informações sobre instâncias, medidores e departamentos. A API pode ser consultada por período de faturação ou por uma data de início e de fim especificada. 

* **Encargos de arquivo de Marketplace** - [API de encargos de arquivo de Marketplace](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) devolve a repartição de encargos de marketplace baseada na utilização por dia para o período especificado de faturação ou datas de início e fim (taxas de uma vez não são incluídas).

* **Folha de preços** - [API de folha de preços](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) fornece a taxa de aplicável para cada medidor para a inscrição e a faturação período indicado. 

## <a name="data-freshness"></a>Atualização de Dados
Os etags são serão devolvidos na resposta da API acima. Uma alteração na Etag indica que os dados tem sido atualizados.  Em subsequentes chamadas à API do mesma utilizando os mesmos parâmetros, passe o Etag capturada com a chave "If-None-Match" no cabeçalho de pedido de http. O código de estado de resposta seria "NotModified" se os dados não tem sido atualizados mais e não serão possível devolvidos dados. API irá devolver o conjunto de dados completo para o período de necessário sempre que exista uma alteração de etag.

## <a name="helper-apis"></a>APIs de programa auxiliar
 **Lista de períodos de faturação** - [API de períodos de faturação](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) devolve uma lista de períodos que tenham dados de consumo para a inscrição especificada por ordem cronológica inversa de faturação. Cada período contém uma propriedade de apontar para a rota de API para os quatro conjuntos de dados - BalanceSummary, UsageDetails, os encargos de Marketplace e folha de preços.


## <a name="api-response-codes"></a>Códigos de resposta de API  
|Código de estado de resposta|Mensagem|Descrição|
|-|-|-|
|200| OK|Nenhum erro|
|401| Não autorizado| Chave de API não foi encontrada, inválido, etc expirou.|
|404| Indisponível| Ponto final de relatório não foi encontrado|
|400| Pedido Incorreto| Parâmetros inválidos – intervalos de datas, números EA etc.|
|500| Erro do Servidor| Pedido de processamento de erros de Unexoected| 









