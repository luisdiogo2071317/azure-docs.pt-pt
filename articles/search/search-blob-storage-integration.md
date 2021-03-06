---
title: Adicionar a pesquisa em texto completo para o armazenamento de Blobs do Azure - Azure Search
description: Conteúdo de texto de pesquisa no armazenamento de Blobs do Azure para a indexação de pesquisa do Azure, no código utilizando a API de REST de HTTP.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: 7bd7bcf66f0d91a87519a5d5bff7df7c73e2603f
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310883"
---
# <a name="searching-blob-storage-with-azure-search"></a>Pesquisar o armazenamento de Blobs com o Azure Search

A pesquisa em toda a variedade de tipos de conteúdo armazenados no armazenamento de Blobs do Azure pode ser um problema difícil de resolver. No entanto, pode indexar e procure o conteúdo dos Blobs em apenas alguns cliques, utilizando o Azure Search. Procurar no armazenamento de BLOBs requer o aprovisionamento de um serviço Azure Search. Os vários limites de serviço e os escalões de preços do Azure Search podem ser encontrados no [página de preços](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>O que é o Azure Search?
[O Azure Search](https://aka.ms/whatisazsearch) é um serviço de pesquisa que torna mais fácil para os desenvolvedores adicionar experiências de pesquisa de texto completo robusta para aplicações web e móveis. Como um serviço, o Azure Search remove a necessidade de gerir qualquer infraestrutura de pesquisa ao oferecer um [tempo de atividade de 99,9% SLA](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Formatos de documento de enterprise do índice e pesquisa
Com suporte para [documentar extração](https://aka.ms/azsblobindexer) no armazenamento de Blobs do Azure, permite-lhe indexar o seguinte conteúdo:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Extraindo texto e os metadados destes tipos de ficheiro, pode pesquisar em vários formatos de arquivo com uma única consulta. 

## <a name="search-through-your-blob-metadata"></a>Pesquisar por meio de seus metadados do blob
É um cenário comum que torna mais fácil ordenar por meio de blobs de qualquer tipo de conteúdo indexar metadados personalizados e as propriedades do sistema para cada blob. Dessa forma, informações para todos os blobs são indexadas, independentemente do tipo de documento. Em seguida, pode avançar para ordenar, filtrar e faceta em todos os conteúdos de armazenamento de Blobs.

[Saiba que mais sobre indexação os metadados do blob.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Pesquisa de imagens
Pesquisa em texto completo, navegação por facetas e capacidades de classificação do Azure Search agora podem ser aplicadas aos metadados de imagens armazenados em blobs.

Se estas imagens previamente são processadas utilizando o [API de imagem digitalizada](https://www.microsoft.com/cognitive-services/computer-vision-api) partir dos serviços cognitivos da Microsoft, em seguida, é possível indexar o conteúdo visual encontrado em cada imagem, incluindo o reconhecimento de OCR e de escrita manual. Estamos a trabalhar na adição de OCR e outros recursos de processamento de imagem diretamente para o Azure Search, se estiver interessado nestas capacidades, submeter um pedido no nosso [UserVoice](https://aka.ms/azsuv) ou [envie-num e-mail](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Índice e a pesquisa por meio de JSON blobs
O Azure Search pode ser configurado para extrair conteúdo estruturado encontrado nos blobs que contêm o JSON. O Azure Search pode ler JSON blobs e analisar o conteúdo estruturado nos campos apropriados de um documento do Azure Search. O Azure Search também pode tirar blobs que contém uma matriz de objetos JSON e mapeiam cada elemento para um documento separado do Azure Search.

Análise de JSON não é atualmente configurável através do portal. [Saiba mais sobre análise de JSON no Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Início rápido
O Azure Search pode ser adicionado para blobs diretamente a partir da página de portal de armazenamento de Blobs.

![](./media/search-blob-storage-integration/blob-blade.png)

Clique em **adicionar Azure Search** para iniciar um fluxo, onde pode selecionar um serviço de pesquisa do Azure existente ou criar um novo serviço. Se criar um novo serviço, são navegar para fora da experiência do portal da conta de armazenamento. Pode navegar de volta para a página de portal de armazenamento e a selecionar os **adicionar Azure Search** opção, onde pode selecionar o serviço existente.

### <a name="next-steps"></a>Próximos Passos
Saiba mais sobre o indexador de Blobs do Azure Search em todo o [documentação](https://aka.ms/azsblobindexer).
