---
title: Adicionar analisadores de idiomas - Azure Search
description: Análise de texto de léxicos em vários idiomas para consultas de inglês e índices no Azure Search.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Yahnoosh
ms.author: jlembicz
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: b5c562994c169a8c5d51ee31a9606c5c40162603
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008336"
---
# <a name="add-language-analyzers-to-an-azure-search-index"></a>Adicionar analisadores de idiomas para um índice da Azure Search

R *analisador de idioma* é um componente específico de um [mecanismo de pesquisa de texto completo](https://docs.microsoft.com/azure/search/search-lucene-query-architecture) que realiza a análise lexical, usando as regras de idioma de destino. Cada campo pesquisável tem um `analyzer` propriedade. Se o índice contém as cadeias traduzidas, tais como campos separados para texto em inglês e o chinês, poderia especificar analisadores de idiomas em cada campo para aceder às funcionalidades avançadas linguísticos os analisadores.  

O Azure Search oferece suporte a 35 analisadores alicerçados Lucene e 50 analisadores apoiados por proprietária Microsoft processamento de linguagem natural tecnologia usada no Office e o Bing.

## <a name="compare-language-analyzer-types"></a>Comparar tipos de analisador de idioma 

Alguns desenvolvedores preferem a solução mais familiar, simples e código-fonte aberto de Lucene. Analisadores de idiomas de Lucene são mais rápidos, mas os analisadores de Microsoft avançaram capacidades, como a lematização word decompounding (em linguagens como alemão, dinamarquês, Holandês, sueco, norueguês, Estoniano, Finish, húngaro, Eslovaco) e para a entidade reconhecimento (URLs, emails, datas e números). Se possível, deve executar comparações de analisadores da Microsoft e de Lucene para decidir qual delas é a melhor opção. 

A indexação com analisadores de Microsoft é em média duas a três vezes mais lenta do que seus equivalentes de Lucene, dependendo do idioma. Desempenho de pesquisa não deve ser significativamente afetado para consultas de tamanho médio. 

### <a name="english-analyzers"></a>Analisadores em inglês

O analisador padrão é Lucene padrão, o que funciona bem para inglês, mas talvez não, bem como analisador em inglês da Lucene ou analisador em inglês da Microsoft. 
 
+ Analisador de em inglês do Lucene estende o analisador padrão. Remove possessives (do à direita) de palavras, aplica-se a lematização de acordo com o algoritmo Porter Lematização e remove palavras de paragem em inglês.  

+ Analisador de em inglês da Microsoft realiza Lematização em vez de Lematização. Isso significa que ele pode lidar com formas de palavras de inflected e irregulares muito melhor o que resulta em resultados de pesquisa mais relevantes 

 > [!Tip]
 > O [demonstração do Search Analyzer](https://alice.unearth.ai/) fornece comparação lado a lado dos resultados produzidos pelo analisador de Lucene padrão, o analisador de idioma inglês do Lucene e o processador de idioma natural da Microsoft. Para cada entrada de pesquisa é fornecer, resultados de cada analisador são apresentados nos painéis adjacentes.

## <a name="analyzer-configuration"></a>Configuração de analisador

Para cada campo na definição do índice, pode definir o `analyzer` propriedade a um nome de analisador que especifica qual linguagem e o fornecedor. O analisador mesmo será aplicado quando indexação e a pesquisa para esse campo. Por exemplo, pode ter campos separados para inglês, francês e espanhol descrições de hotel que existem lado a lado no mesmo índice.  

Utilize o **searchFields** parâmetro para especificar o campo de idioma específico para pesquisar em suas consultas de consulta. Pode rever os exemplos de consulta que incluem a propriedade de analisador em documentos de pesquisa. 

Para obter mais informações sobre as propriedades de índice, consulte [Create Index &#40;API de REST do serviço de pesquisa do Azure&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index). Para obter mais informações sobre a análise do Azure Search, consulte [analisadores no Azure Search](https://docs.microsoft.com/azure/search/search-analyzers).

## <a name="analyzer-list"></a>Lista de analisador  
 Segue-se a lista de idiomas suportados, juntamente com os nomes de analisador do Lucene e da Microsoft.  

|Idioma|Nome da Microsoft Analyzer|Nome do analisador de Lucene|  
|--------------|-----------------------------|--------------------------|  
|Árabe|ar.microsoft|ar.lucene|  
|Arménio||hy.lucene|  
|Bangla|bn.microsoft||  
|Basco (Basco)||eu.lucene|  
|Búlgaro|bg.microsoft|bg.lucene|  
|Catalão|ca.microsoft|ca.lucene|  
|Chinês Simplificado|zh-Hans.microsoft|zh-Hans.lucene|  
|Chinês Tradicional|zh-Hant.microsoft|zh-Hant.lucene|  
|Croata|hr.microsoft||  
|Checo|cs.microsoft|cs.lucene|  
|Dinamarquês|da.microsoft|da.lucene|  
|Neerlandês|nl.microsoft|nl.lucene|  
|Português|en.microsoft|en.lucene|  
|Estónio|et.microsoft||  
|Finlandês|fi.microsoft|fi.lucene|  
|Francês|fr.microsoft|fr.lucene|  
|Galego||gl.lucene|  
|Alemão|de.microsoft|de.lucene|  
|Grego|el.microsoft|el.lucene|  
|Guzarate|gu.microsoft||  
|Hebraico|he.microsoft||  
|Hindi|hi.microsoft|Hi.lucene|  
|Húngaro|hu.microsoft|hu.lucene|  
|Islandês|is.microsoft||  
|Indonésio (Bahasa)|id.microsoft|id.lucene|  
|Irlandês||ga.lucene|  
|Italiano|it.microsoft|it.lucene|  
|Japonês|ja.microsoft|ja.lucene|  
|Canarim|ka.microsoft||  
|Coreano|ko.microsoft|ko.lucene|  
|Letão|lv.microsoft|lv.lucene|  
|Lituano|lt.microsoft||  
|Malayalam|ml.microsoft||  
|Malaio (Latim)|ms.microsoft||  
|Marata|mr.microsoft||  
|Norueguês|nb.microsoft|no.lucene|  
|Persa||fa.lucene|  
|Polaco|pl.microsoft|pl.lucene|  
|Português (Brasil)|pt-Br.microsoft|pt-Br.lucene|  
|Português (Portugal)|pt-Pt.microsoft|pt-Pt.lucene|  
|Punjabi|pa.microsoft||  
|Romeno|ro.microsoft|ro.lucene|  
|Russo|ru.microsoft|ru.lucene|  
|Sérvio (Cirílico)|sr-cyrillic.microsoft||  
|Sérvio (Latim)|sr-latin.microsoft||  
|Eslovaco|sk.microsoft||  
|Esloveno|sl.microsoft||  
|Espanhol|es.microsoft|es.lucene|  
|Sueco|sv.microsoft|sv.lucene|  
|Tamil|ta.microsoft||  
|Télego|te.microsoft||  
|Tailandês|th.microsoft|th.lucene|  
|Turco|tr.microsoft|tr.lucene|  
|Ucraniano|uk.microsoft||  
|Urdu|ur.microsoft||  
|Vietnamita|vi.microsoft||  

 Todos os analisadores com nomes anotados com **Lucene** têm a tecnologia [analisadores de idiomas do Apache Lucene](https://lucene.apache.org/core/4_9_0/core/overview-summary.html ).

## <a name="see-also"></a>Consulte também  
 [Criar índice &#40;API de REST do serviço Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
 [Classe de AnalyzerName](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  
 [Vídeo: o módulo 7 da apresentação de pesquisa do Azure da MVA](https://channel9.msdn.com/Series/Adding-Microsoft-Azure-Search-to-Your-Websites-and-Apps/07).  

