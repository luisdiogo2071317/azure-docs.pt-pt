---
title: Atualizar para a versão mais recente da API de REST do serviço de pesquisa do Azure | Microsoft Docs
description: Atualizar para a versão mais recente da API de REST do serviço de pesquisa do Azure
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 3848f317fd6d760961756f132edf9cbcb5f431ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181975"
---
# <a name="upgrading-to-the-latest-azure-search-service-rest-api-version"></a>Atualizar para a versão mais recente da API de REST do serviço de pesquisa do Azure
Se estiver a utilizar uma versão anterior do [API de REST do serviço de pesquisa do Azure](https://docs.microsoft.com/rest/api/searchservice/), este artigo irá ajudá-lo a atualizar a sua aplicação para utilizar a versão mais recente versão de API geralmente disponível, 2017-11-11.

Versão de 2017-11-11 da REST API contém algumas alterações de versões anteriores. Estes são principalmente retrocompatível, para que alterar o seu código deverá ser preciso apenas esforço, consoante a versão que estava a utilizar antes. Consulte [passos para atualizar](#UpgradeSteps) para obter instruções sobre como alterar o código para utilizar a nova versão de API.

> [!NOTE]
> A instância de serviço de pesquisa do Azure suporta várias versões de REST API, incluindo o mais recente. Pode continuar a utilizar uma versão quando já não é um mais recente, mas recomendamos que migre código para utilizar a versão mais recente.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Novidades na versão de 2017-11-11
Versão de 2017-11-11 é o mais recente versão geralmente disponível da API de REST do serviço de pesquisa do Azure. Novas funcionalidades nesta versão de API incluem:

* [Sinónimos](search-synonyms.md). A nova funcionalidade de sinónimos permite-lhe definir termos equivalentes e expande o âmbito da consulta.
* [Suporte de forma eficiente indexação de blobs de texto](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). A nova `text` análise significativamente o modo de indexadores de Blob do Azure melhora o desempenho de indexação.
* [Estatísticas de API do serviço](https://aka.ms/azure-search-stats). Ver a utilização atual e os limites de recursos na Azure Search com esta nova API.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Passos para atualizar
Se estiver a atualizar a partir de uma versão do DG, 2015-02-28 ou 2016-09-01, provavelmente, não terá efetue as alterações ao código, diferente de alterar o número de versão. As únicas situações em que poderá ter de alterar o código de são quando:

* O código de falha quando uma resposta de API, são devolvidas propriedades não reconhecidas. Por predefinição, a aplicação deve ignorar propriedades que não compreender.
* O código de persistir pedidos de API e tenta reenviá-los para a nova versão de API. Por exemplo, isto pode acontecer se a aplicação persistir tokens de continuação devolvidos a partir da API de pesquisa (para obter mais informações, procure `@search.nextPageParameters` no [referência da API de pesquisa](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Se qualquer uma destas situações se aplicar ao utilizador, poderá ter de alterar o código em conformidade. Caso contrário, não existem alterações devem ser necessárias a menos que pretenda começar a utilizar o [novas funcionalidades](#WhatsNew) da versão de 2017-11-11.

Se estiver a atualizar a partir de uma versão de api de pré-visualização, acima também se aplicam, mas deve também estar ciente de que algumas funcionalidades de pré-visualização não estão disponíveis na versão de 2017-11-11:

* Suporte do indexador de armazenamento de Blobs do Azure para blobs que contém matrizes JSON e de ficheiros CSV.
* Consultas de "Mais como"

Se o seu código utiliza estas funcionalidades, não poderá atualizar para 2017-11-11 sem remover a sua utilização dos mesmos.

> [!IMPORTANT]
> . Lembre-se, pré-visualizar APIs foram concebidas para avaliação e de teste e não deve ser utilizadas em ambientes de produção.
> 
> 

## <a name="conclusion"></a>Conclusão
Se precisar de mais detalhes sobre como utilizar a API de REST do serviço de pesquisa do Azure, consulte recentemente atualizado [referência da API](https://docs.microsoft.com/rest/api/searchservice/) no MSDN.

Apreciamos os seus comentários na Azure Search. Se tiver problemas, não hesite e peça-nos para obter ajuda no [fórum MSDN de pesquisa do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) ou [StackOverflow](http://stackoverflow.com/). Se está a solicitar uma pergunta sobre a Azure Search no StackOverflow, certifique-se a etiqueta com `azure-search`.

Obrigado por utilizar a pesquisa do Azure!

