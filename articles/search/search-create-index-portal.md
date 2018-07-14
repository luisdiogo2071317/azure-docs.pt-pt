---
title: Criar um índice da Azure Search no portal do | Documentos da Microsoft
description: Saiba como criar um índice para o Azure Search com designers de índice de portal internos.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: bb1ba5e860dab237b3f6e16205b5e4cbad45e6e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990851"
---
# <a name="how-to-create-an-azure-search-index-using-the-azure-portal"></a>Como criar um índice da Azure Search no portal do Azure

O Azure Search inclui um índice internos designer no portal do útil para protótipos ou criar um [índice de pesquisa](search-what-is-an-index.md) alojados no seu serviço Azure Search. A ferramenta é utilizada para a construção de esquema. Ao guardar a definição, um índice vazio torna-se totalmente expresso no Azure Search. Como carregá-lo com dados pesquisáveis cabe a.

O estruturador de índice é apenas uma abordagem para criar um índice. Por meio de programação, pode criar um índice com o [.NET](search-create-index-dotnet.md) ou [REST](search-create-index-rest-api.md) APIs.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo parte do princípio de que existe uma [subscrição do Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) e o [serviço Azure Search](search-create-service-portal.md).

## <a name="open-index-designer-and-name-an-index"></a>Abra o estruturador de índices e nome de um índice

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e abra o dashboard de serviço. Pode clicar em **Todos os serviços** na barra de atalhos para procurar os “serviços de pesquisa” existentes na subscrição atual. 

2.  Clique no botão **Adicionar índice** na barra de comando na parte superior da página.

3. Atribua um nome ao índice do Azure Search. Nomes de índice são referenciados nas operações de indexação e consulta. O nome do índice passa a fazer parte do URL do ponto final utilizado nas ligações ao índice e para enviar pedidos HTTP na API REST do Azure Search.

   * Comece com uma letra.
   * Utilize apenas letras minúsculas, números ou hífenes ("-").
   * Limite o nome a 60 carateres.

## <a name="define-the-fields-of-your-index"></a>Definir os campos do índice

A composição do índice inclui uma *coleção de Campos* que define os dados pesquisáveis no índice. Em conjunto, a coleção de campos Especifica a estrutura de documentos que carrega separadamente. Uma coleção de campos inclui campos obrigatórios e opcionais, com o nome e escritos, com atributos de índice que determinam como o campo pode ser utilizado.

1. No painel **Adicionar Índice**, clique em **Campos >** para abrir gradualmente o painel de definição de campos. 

2. Aceite o campo de *chave* gerado do tipo Edm.String. Por predefinição, o campo tem o nome *id*, mas pode alterá-lo, desde que a cadeia satisfaça as [regras de nomenclatura](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Um campo de chave é obrigatório para todos os índices do Azure Search e tem de ser uma cadeia.

3. Adicione campos para especificar completamente os documentos que irá carregar. Se os documentos forem compostos por um *id*, *nome de hotel*, *endereço*, *cidade* e *região*, crie um campo correspondente para cada um no índice. Reveja a [documentação de orientação na secção abaixo](#design) para obter ajuda na definição dos atributos.

4. Opcionalmente, adicione quaisquer campos utilizados internamente em expressões de filtro. Os atributos no campo podem ser definidos para excluir campos das operações de pesquisa.

5. Quando terminar, clique em **OK** para guardar e criar o índice.

## <a name="tips-for-adding-fields"></a>Sugestões para adicionar campos

Criar um índice no portal é exigente em termos de teclado. Minimize os passos ao seguir este fluxo de trabalho:

1. Primeiro, crie a lista de campos ao introduzir os nomes e ao definir os tipos de dados.

2. Em seguida, utilize as caixas de verificação na parte superior de cada atributo para permitir a definição em massa para todos os campos e desmarque seletivamente as caixas para os poucos campos que não o requerem. Por exemplo, os campos de cadeia são normalmente pesquisáveis. Como tal, pode clicar em **Recuperável** e **Pesquisável** para devolver os valores do campo nos resultados da pesquisa, bem como permitir a pesquisa em texto completo no campo. 

<a name="design"></a>

## <a name="design-guidance-for-setting-attributes"></a>Documentação de orientação para definição de atributos

Embora possa adicionar novos campos em qualquer altura, as definições de campos existentes estão bloqueadas durante o ciclo de vida do índice. Por este motivo, os programadores utilizam normalmente o portal para criar índices simples, testar ideias ou utilizar as páginas do portal para procurar uma definição. A iteração frequente através de uma estrutura de índice é mais eficiente se seguir uma abordagem baseada em código de modo a poder reconstruir o índice facilmente.

O analisadores e os sugestores estão associados a campos antes de o índice ser guardado. Certifique-se de que clica em cada página com separadores para adicionar sugestores ou analisadores de idioma à definição do índice.

Os campos de cadeia são frequentemente marcados como **Pesquisáveis** e **Recuperáveis**.

Os campos utilizados para restringir os resultados da pesquisa incluem as definições **Ordenável**, **Filtrável** e **Facetável**.

Os atributos de campo determinam como um campo é utilizado, por exemplo, se é utilizado na pesquisa em texto completo, navegação por facetas, operações de ordenação e assim sucessivamente. A tabela seguinte descreve cada atributo.

|Atributo|Descrição|  
|---------------|-----------------|  
|**pesquisável**|Texto completo pesquisável, sujeito a análise lexical, como separação de palavras durante a indexação. Se definir um campo pesquisável para um valor como "sunny day", será dividido internamente nos tokens individuais "sunny" e "day". Para obter detalhes, veja [Como funciona a pesquisa em texto completo](search-lucene-query-architecture.md).|  
|**filtrável**|Referenciado na consulta **$filter**. Os campos filtráveis do tipo `Edm.String` ou `Collection(Edm.String)` não são submetidos a separação de palavras, pelo que as comparações destinam-se apenas a correspondências exatas. Por exemplo, se definir um campo f para "sunny day", `$filter=f eq 'sunny'` não encontrará correspondências, mas `$filter=f eq 'sunny day'` sim. |  
|**ordenável**|Por predefinição, o sistema ordena os resultados por classificação, mas pode configurar a ordenação com base nos campos nos documentos. Os campos do tipo `Collection(Edm.String)` não podem ser **ordenáveis**. |  
|**facetável**|Normalmente, é utilizado numa apresentação de resultados de pesquisa que inclui uma contagem de resultados por categoria (por exemplo, hotéis numa cidade específica). Esta opção não pode ser utilizada com campos do tipo `Edm.GeographyPoint`. Os campos do tipo `Edm.String` que sejam **filtráveis**, **ordenáveis** ou **facetáveis** podem ter um máximo de 32 kilobytes. Para mais detalhes, veja [Criar um Índice (API REST)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**chave**|Identificador exclusivo de documentos no índice. Deve ser selecionado exatamente um campo como o campo de chave e tem de ser do tipo `Edm.String`.|  
|**recuperável**|Determina se o campo pode ser devolvido num resultado da pesquisa. Isto é útil quando quiser utilizar um campo (como *margem de lucro*) como mecanismo de filtro, ordenação ou classificação, mas não quer que o campo esteja visível para o utilizador final. Este atributo tem de ser `true` para campos `key`.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Criar o índice de hotéis utilizado nas secções de API de exemplo

A documentação da API do Azure Search inclui exemplos de código com um índice de *hotéis* simples. Nas capturas de ecrã abaixo, pode ver a definição do índice, incluindo o analisador de idioma francês especificado durante a definição do índice, que pode recriar como um exercício prático no portal.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Passos Seguintes

Depois de criar um índice do Azure Search, pode avançar para o próximo passo: [carregar dados pesquisáveis para o índice](search-what-is-data-import.md).

Em alternativa, também pode analisar os índices mais detalhadamente. Além da coleção de Campos, um índice também especifica analisadores, sugestores, perfis de classificação e definições CORS. O portal fornece páginas com separadores para definir os elementos mais comuns: campos, analisadores e sugestores. Para criar ou modificar outros elementos, pode utilizar a API REST ou o SDK .NET.

## <a name="see-also"></a>Consulte também

 [Como funciona a pesquisa em texto completo](search-lucene-query-architecture.md)  
 [API REST do serviço de pesquisa](https://docs.microsoft.com/rest/api/searchservice/) [SDK .NET](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

