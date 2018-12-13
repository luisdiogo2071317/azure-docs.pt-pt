---
title: Anexar serviços cognitivos para o conjunto de capacidades - Azure Search
description: Instruções para anexar uma subscrição de tudo-em-um dos serviços cognitivos para um conjunto de capacidades cognitivas
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7f604d1b94e51db11e6d6992b7f070d64ae869dd
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317241"
---
# <a name="associate-cognitive-services-resource-with-a-skillset"></a>Associar o recurso dos serviços cognitivos com um conjunto de capacidades 

> [!NOTE]
> A partir de 21 de Dezembro de 2018, poderá associar um recurso dos serviços cognitivos com um conjunto de capacidades do Azure Search. Isso permitirá que comecem a cobrança de consumo para a execução do conjunto de capacidades. Esta data, podemos também começar a cobrar extração de imagem como parte da fase de aberturas de documentos. Continuaremos a oferecer a extração de texto de documentos sem custos adicionais.
>
> A execução das habilidades internas será cobrada existente [dos serviços cognitivos pay as you go preço](https://azure.microsoft.com/pricing/details/cognitive-services/) . Preços de extração de imagem são cobrados ao preço de pré-visualização e está descrito na [página de preços do Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).


Pesquisa cognitiva extrai e otimiza os dados para torná-lo pesquisáveis no Azure Search. Chamamos passos de extração e enriquecimento *capacidades cognitivas*. O conjunto de habilidades chamado durante a indexação de conteúdo são definidos num *conjunto de capacidades*. Pode utilizar um conjunto de capacidades [predefinidos habilidades](cognitive-search-predefined-skills.md) ou competências personalizadas (consulte [exemplo: criar uma habilidade personalizada](cognitive-search-create-custom-skill-example.md) para obter mais informações).

Neste artigo, irá aprender a associar a [dos serviços cognitivos ](https://azure.microsoft.com/services/cognitive-services/) recurso para o conjunto de capacidades cognitivo.

O recurso dos serviços cognitivos que selecionar serão power as capacidades cognitivas incorporadas. Este recurso também será utilizado para fins de faturação. Qualquer possível que realizar a utilizar as capacidades cognitivas incorporadas é cobradas contra o recurso dos serviços cognitivos que selecionar. Eles são cobrados à mesma tarifa como se tivesse executado a mesma tarefa através de um recurso de serviços cognitivos. Ver [preços dos serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Limites de quando não é selecionado nenhum recurso dos serviços cognitivos
A partir de 1 de Fevereiro de 2019, se não associe uma subscrição de serviços cognitivos com as suas competências, apenas poderá enriquecer um pequeno número de documentos para gratuitos (20 os documentos por dia). 

## <a name="associating-a-cognitive-services-resource-with-a-new-skillset"></a>Associar um recurso de serviços cognitivos com um novo conjunto de capacidades

1. Como parte do *importar dados* experiência, depois de ligar à origem de dados irá navegar para o *adicionar a pesquisa cognitiva* passo opcional. 

1. Expanda a *anexar serviços cognitivos* secção. Isto irá mostrar quaisquer recursos de serviço cognitivo que tiver nas mesmas regiões que o seu serviço de pesquisa. 
![Expandido anexar serviço cognitivo](./media/cognitive-search-attach-cognitive-services/attach1.png "expandido anexar serviços cognitivos")

1. Selecione um recurso existente de serviços cognitivos, ou *criar um novo recurso de serviços cognitivos*. Se selecionar a *gratuito (limitado possível) recursos*, apenas será possível enriquecer um pequeno número de documentos para gratuitos (20 os documentos por dia). Se clicar em *criar um novo recurso de serviços cognitivos*, um novo separador aberto que permite-lhe criar o recurso dos serviços cognitivos. 

1. Se tiver criado um novo recurso, clique em *atualizar* para atualizar a lista de recursos dos serviços cognitivos e selecione o recurso. 
![Selecionou o recurso de serviço cognitivo](./media/cognitive-search-attach-cognitive-services/attach2.png "selecionado o recurso de serviço cognitivo")

1. Assim que tiver feito isso, pode expandir a *possível adicionar* secção para selecionar as capacidades cognitivas específicas que pretende executar com os seus dados e continue com o resto do fluxo.

## <a name="associating-a-cognitive-services-resource-with-an-existing-skillset"></a>Associar um recurso de serviços cognitivos com um conjunto de competências existente

1. Na página de descrição geral do serviço, selecione o *conjuntos de competências* separador. ![Separador conjuntos de competências](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "separador conjuntos de competências")

1. *Clique em* no conjunto de capacidades que pretende modificar. Esta ação irá abrir um painel que lhe permite editar um conjunto de capacidades.

1. Selecione um recurso existente de serviços cognitivos ou *criar um novo recurso de serviços cognitivos*. Se selecionar a *gratuito (limitado possível) recursos*, apenas será possível enriquecer um pequeno número de documentos para gratuitos (20 os documentos por dia). Se clicar em *criar um novo recurso de serviços cognitivos*, um novo separador aberto que permite-lhe criar o recurso dos serviços cognitivos. <n/> 
<img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Se tiver criado um novo recurso, clique em *atualizar* para atualizar a lista de recursos dos serviços cognitivos e selecione o recurso.
1. Clique em *OK* para confirmar as alterações

## <a name="associating-a-cognitive-services-resource-programmatically"></a>Associar um recurso de serviços cognitivos através de programação

Ao definir o conjunto de capacidades programaticamente, adicione um `cognitiveServices` secção. A secção deve incluir a chave do recurso dos serviços cognitivos que pretende associar o conjunto de capacidades, bem como o @odata.type, que deve ser definido como "#Microsoft.Azure.Search.CognitiveServicesByKey". Este padrão é mostrado no exemplo abaixo.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey"
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimating-the-cost-of-document-cracking-and-enrichment"></a>Exemplo: Estimar o custo de aberturas de documentos e enriquecimento
Pode querer fazer uma estimativa quanto custa para enriquecer um determinado tipo de documento. O exercício abaixo é apenas um exemplo, mas poderá ser útil para.

Imagine que temos 1000 PDFs e podemos estimar que em média, cada um desses documentos ter 6 páginas cada um. Digamos que cada um deles tem uma imagem por página para este exercício. Vamos também imaginar que em média, há cerca de 3000 carateres por página. 

Agora, vamos supor que queremos realizar os passos seguintes como parte do pipeline enriquecimento:
1. Como parte de aberturas de documentos, extraia as imagens e o conteúdo do documento.
1. Como parte do melhoramento, extraídos de OCR, cada uma das páginas, combinar o texto para todas as páginas e, em seguida, extraia cada uma das organizações no texto combinado de todas as imagens.

Vamos fazer uma estimativa de quanto iria custar a ingerir esses documentos, passo a passo.

Para os documentos de 1000:

1. Documente a Decodificação, seria extraímos um número combinado de 6.000 imagens. Partindo do princípio de US $1 para cada 1000 imagens extraídas, que iria custar-us $6.00.

2. Seria extraímos o texto de cada uma dessas 6.000 imagens. Em inglês, a habilidade de cognitiva OCR utiliza o melhor algoritmo (DescribeText). Supondo que um custo de US $2.50 por 1000 imagens a ser analisados, podemos pagaria us $15.00 para este passo.

3. Para extração de entidades, teríamos um total de 3 registos de texto por página (cada registo é de 1000 carateres). registos/página de texto 3 * 6.000 páginas = 18,000 registos de texto. Partindo do princípio de US $2,00 / registos de texto de 1000, este passo iria custar us $36.00.

Juntando as peças, podemos pagaria us $57.00 para ingerir 1000 documentos em pdf dessa natureza com o conjunto de capacidades descrito.  Neste exercício, supúnhamos que o preço mais caro de transação, poderia ter sido inferior devido a graduando preços. Ver [preços dos serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Passos Seguintes
+ [Página de preços de pesquisa do Azure](https://azure.microsoft.com/pricing/details/search/)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
+ [Criar conjunto de capacidades (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Como mapear campos plena](cognitive-search-output-field-mapping.md)
