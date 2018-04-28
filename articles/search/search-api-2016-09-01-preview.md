---
title: A pesquisa do Azure serviço REST versão da API 2016-09-01-pré-visualização | Microsoft Docs
description: Azure pesquisa serviço REST versão da API 2016-09-01-Preview inclui funcionalidades experimental como moreLikeThis pesquisas.
author: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: reference
ms.date: 04/18/2018
ms.author: nateko
ms.openlocfilehash: 8eae54c912711a11c015737903b6898b98fd5159
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>API de REST do serviço de pesquisa do Azure: Versão 2016-09-01-Preview
Este artigo é a documentação de referência para `api-version=2016-09-01-Preview`. Esta pré-visualização expande a versão atual do geralmente disponível, [api-version = 2016-09-01](https://docs.microsoft.com/rest/api/searchservice), fornecendo as seguintes funcionalidades experimental:

* [`moreLikeThis` parâmetro de consulta](search-more-like-this.md) localizar documentos que são relevantes para um documento específico.

Certifique-se para a versão de pré-visualização API de destino `api-version=2016-09-01-Preview` para experimentar estas funcionalidades experimental. O exemplo a seguir ilustra como a pré-visualização versão da api é especificada em efetuar uma consulta mais-como-este.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Funcionalidades de pré-visualização estão disponíveis para testar e experiência com o objetivo de recolha de comentários e estão sujeitos a alterações. **Aconselhamos vivamente contra a utilização de APIs de pré-visualização em aplicações de produção.**

Serviço de pesquisa do Azure está disponível em múltiplas versões. Consulte [controlo de versões de serviço de pesquisa](https://docs.microsoft.com/azure/search/search-api-versions) para obter mais detalhes.
