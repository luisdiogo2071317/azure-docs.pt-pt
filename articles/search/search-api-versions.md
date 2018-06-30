---
title: Versões de API do Azure Search | Microsoft Docs
description: Política de versão para as APIs REST do Azure Search e a biblioteca de clientes no SDK do .NET.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: brjohnst
ms.openlocfilehash: 8d1e30b0bca3c63fe4528c06e5389d8cbe27a7e6
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113610"
---
# <a name="api-versions-in-azure-search"></a>Versões de API na Azure Search
A pesquisa do Azure lança atualizações de funcionalidade regularmente. Por vezes, mas nem sempre, estas atualizações requerem uma nova versão da API para preservar a compatibilidade com versões anteriores. Publicar uma nova versão permite-lhe controlar quando e como integrar atualizações de serviço de pesquisa no seu código.

Como uma regra, a equipa da Azure Search publica novas versões apenas quando necessário, uma vez que pode envolver algum esforço para atualizar o seu código para utilizar uma nova versão de API. Uma nova versão só será necessário se algum aspeto da API foi alterado de forma de quebras de retro-compatibilidade. Essas alterações podem acontecer devido a correções a funcionalidades existentes, ou devido a novas funcionalidades que altere a área de superfície de API existente.

A mesma regra aplica-se a atualizações do SDK. O SDK de pesquisa do Azure segue a [versioning semântica](http://semver.org/) regras, o que significa que a versão tem três partes: principal, secundária e (por exemplo, 1.1.0) do número de compilação. É lançada uma nova versão principal do SDK apenas para as alterações que interromper a compatibilidade com versões anteriores. As atualizações de funcionalidade de quebra não irão incrementar a versão secundária e correções de erros só irá aumentar a versão de compilação.

> [!NOTE]
> A instância de serviço de pesquisa do Azure suporta várias versões de REST API, incluindo o mais recente. Pode continuar a utilizar uma versão quando já não é um mais recente, mas recomendamos que migre código para utilizar a versão mais recente. Quando utilizar a API REST, tem de especificar a versão da API em cada pedido através do parâmetro de versão da api. Ao utilizar o SDK .NET, a versão do SDK está a utilizar determina a versão da REST API correspondente. Se estiver a utilizar um SDK anterior, pode continuar a executar esse código sem alterações, mesmo se o serviço é atualizado para suportar uma versão mais recente da API.

## <a name="snapshot-of-current-versions"></a>Instantâneo de versões atuais
Abaixo é um instantâneo das versões atuais do todas programação interfaces para a Azure Search.

| Interfaces | Mais recente versão principal | Estado |
| --- | --- | --- |
| [SDK do .NET](https://aka.ms/search-sdk) |5.0 |Geralmente disponível, lançada Abril de 2018 |
| [Pré-visualização do SDK .NET](https://aka.ms/search-sdk-preview) |4.0.1-Preview |Pré-visualização, lançada Maio de 2017 |
| [API REST do Serviço](https://docs.microsoft.com/rest/api/searchservice/) |2017-11-11 |Geralmente disponível |
| [Serviço REST API de 2017-11-11-Preview](search-api-2017-11-11-preview.md) |2017-11-11-Preview |Pré-visualização |
| [SDK de Gestão .NET](https://aka.ms/search-mgmt-sdk) |2.0 |Geralmente disponível |
| [API REST de Gestão](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Geralmente disponível |

Para as APIs REST, incluindo o `api-version` em cada chamada é necessária. Utilizar `api-version` torna mais fácil para uma versão específica, tal como uma pré-visualização API de destino. O exemplo a seguir ilustra como o `api-version` parâmetro for especificado:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2017-11-11

> [!NOTE]
> Apesar de cada pedido tem um `api-version`, recomendamos que utilize a mesma versão para todos os pedidos de API. Isto é especialmente verdadeiro quando novas versões de API introduzem atributos ou operações que não são reconhecidas por versões anteriores. A combinação de versões de API pode ter consequências inesperadas e devem ser evitados.
>
> A API REST do serviço e a API de REST de gestão são com versão independentemente entre si. Qualquer semelhança números de versão é coincidental.

Geralmente disponível (ou GA) APIs pode ser utilizadas em produção e estão sujeitos aos contratos de nível de serviço do Azure. Versões de pré-visualização têm experimental funcionalidades que não são migradas sempre para uma versão GA. **É vivamente aconselhado Evite utilizar as APIs de pré-visualização em aplicações de produção.**

## <a name="about-preview-and-generally-available-versions"></a>Sobre as versões de pré-visualização e geralmente disponível
A pesquisa do Azure sempre previamente versões funcionalidades experimental através da API REST em primeiro lugar, em seguida, através de versões de pré-lançamento do SDK do .NET.

Funcionalidades de pré-visualização estão disponíveis para a experimentação, com o objetivo de recolha de comentários na estrutura de funcionalidade e a implementação e teste. Por este motivo, as funcionalidades de pré-visualização podem alterar ao longo do tempo, possivelmente formas que efeitos interromper a compatibilidade. Este é contrariamente funcionalidades numa versão GA, que são estável e pouco provável alterar com a exceção pequenas correções de retro-compatibilidade e melhoramentos. Além disso, as funcionalidades de pré-visualização não sempre torná-lo para uma versão GA.

Por esta razão, é recomendável relativamente ao escrever o código de produção que assume uma dependência em versões de pré-visualização. Se estiver a utilizar uma versão de pré-visualização mais antiga, recomendamos a migrar para a versão (GA) geralmente disponível.

Para o SDK .NET: orientações para a migração de código podem ser encontradas em [atualizar o SDK .NET](search-dotnet-sdk-migration.md).

Disponibilidade geral significa que da Azure Search agora sob o contrato de nível de serviço (SLA). É possível localizar o SLA [contratos de nível de serviço de pesquisa do Azure](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
