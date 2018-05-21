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
ms.openlocfilehash: e5cd7fcd0c853f03dbafb4d95b8459dcc83aecdf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>API de REST do serviço de pesquisa do Azure: Versão 2016-09-01-Preview
Este artigo apresenta uma lista de funcionalidades de pré-visualização `api-version=2016-09-01-Preview`. Esta pré-visualização expande a versão anterior do geralmente disponível, [api-version = 2016-09-01](https://docs.microsoft.com/rest/api/searchservice), fornecendo as seguintes funcionalidades experimental:

* [`moreLikeThis` parâmetro de consulta](search-more-like-this.md) localizar documentos que são relevantes para um documento específico. Esta funcionalidade tem sido pré-visualizações anteriores. Se estiver a chamar esta API com uma versão de api anterior, pode continuar a utilizar essa versão.


## <a name="how-to-call-a-preview-api"></a>Como chamar uma API de pré-visualização

O exemplo a seguir ilustra como a pré-visualização versão da api é especificada em efetuar uma consulta mais-como-este.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Funcionalidades de pré-visualização estão disponíveis para testar e experiência com o objetivo de recolha de comentários e estão sujeitos a alterações. **Aconselhamos vivamente contra a utilização de APIs de pré-visualização em aplicações de produção.**

Serviço de pesquisa do Azure está disponível em múltiplas versões. Consulte [controlo de versões de serviço de pesquisa](https://docs.microsoft.com/azure/search/search-api-versions) para obter mais detalhes.
