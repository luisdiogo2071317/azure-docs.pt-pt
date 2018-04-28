---
title: moreLikeThis na pesquisa do Azure (pré-visualização) | Microsoft Docs
description: Preliminar documentação para a funcionalidade de moreLikeThis (pré-visualização), exposta na API de REST da Azure Search.
authors: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 29d9a478ca2e91e658d7d0f52e7a193ba694bc16
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis na pesquisa do Azure (pré-visualização)

`moreLikeThis=[key]` é um parâmetro de consulta no [Search API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Ao especificar o `moreLikeThis` parâmetro numa consulta de pesquisa, pode encontrar documentos que são semelhantes ao documento especificado pela chave do documento. Quando é efetuado um pedido de pesquisa com `moreLikeThis`, é gerada uma consulta com os termos da procura extraídos do documento fornecido que descrevem melhor esse documento. A consulta gerada, em seguida, é utilizada para efetuar o pedido de pesquisa. Por predefinição, o conteúdo de todos os `searchable` campos são considerados a menos que o `searchFields` parâmetro é utilizado para restringir os campos. O `moreLikeThis` parâmetro não pode ser utilizado com o parâmetro de pesquisa, `search=[string]`.

## <a name="examples"></a>Exemplos 

Segue-se um exemplo de uma consulta de moreLikeThis. A consulta localizar documentos cujos campos de descrição são mais semelhantes para o campo do documento de origem, tal como especificado pelo `moreLikeThis` parâmetro.

```  
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```  

```  
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {  
      "moreLikeThis": "1002",  
      "searchFields": "description"  
    }  
```  

## <a name="feature-availability"></a>Disponibilidade de funcionalidades

A funcionalidade de moreLikeThis está atualmente em pré-visualização e só são suportados na pré-visualização api-versions, `2015-02-28-Preview` e `2016-09-01-Preview`. Porque a versão da API é especificada no pedido, é possível combinar geralmente disponível (GA) e APIs de pré-visualização na mesma aplicação. No entanto, as APIs não estão sob o SLA e funcionalidades de pré-visualização pode alterar, pelo que recomendamos que não utilizá-los em aplicações de produção.