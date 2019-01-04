---
title: Associar um recurso dos serviços cognitivos com um conjunto de capacidades no Azure Search
description: Instruções para anexar uma subscrição de tudo-em-um dos serviços cognitivos para um conjunto de capacidades cognitivo no Azure Search
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 52efa685bba330879365f56e547881d62a52a185
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000160"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Associar um recurso dos serviços cognitivos com um conjunto de capacidades no Azure Search 

Pesquisa cognitiva extrai e otimiza os dados para torná-lo pesquisáveis no Azure Search. Chamamos passos de extração e enriquecimento *capacidades cognitivas*. O conjunto de habilidades chamado durante a indexação de conteúdo são definidos num *conjunto de capacidades*. Pode utilizar um conjunto de capacidades [predefinidos habilidades](cognitive-search-predefined-skills.md) ou competências personalizadas. Para obter mais informações, consulte [exemplo: criar uma habilidade personalizada](cognitive-search-create-custom-skill-example.md).

Neste artigo, saiba como associar um [serviços cognitivos do Azure ](https://azure.microsoft.com/services/cognitive-services/) recurso com o conjunto de capacidades cognitivo.

O recurso dos serviços cognitivos que selecionar serão power as capacidades cognitivas incorporadas. Este recurso também será utilizado para fins de faturação. Será cobrado qualquer possível que efetuar ao utilizar as capacidades cognitivas incorporadas contra o recurso dos serviços cognitivos que selecionar. Eles são cobrados à mesma tarifa como se tivesse efetuadas a mesma tarefa com um recurso dos serviços cognitivos. Ver [preços dos serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/).

> [!NOTE]
> A partir de 21 de Dezembro de 2018, pode associar um recurso dos serviços cognitivos com um conjunto de capacidades do Azure Search. Isto permite-nos ser cobrada a execução do conjunto de capacidades. Esta data, também Começámos a cobrar para extração de imagem como parte da fase de aberturas de documentos. Extração de texto de documentos continuam a ser disponibilizado sem custos adicionais.
>
> A execução das habilidades internas é cobrada a [dos serviços cognitivos pay as you go preço](https://azure.microsoft.com/pricing/details/cognitive-services/). Preços de extração de imagem é cobrado a preços de pré-visualização e está descrito na [página de preços do Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Limites de quando não é selecionado nenhum recurso dos serviços cognitivos
A partir de 1 de Fevereiro de 2019, se não associar uma subscrição de serviços cognitivos com as suas competências, poderá enriquecer somente um pequeno número de documentos para gratuita (20 documentos por dia). 

## <a name="associate-a-cognitive-services-resource-with-a-new-skillset"></a>Associar um recurso dos serviços cognitivos com um novo conjunto de capacidades

1. Como parte do [importar dados](search-import-data-portal.md) assistente, depois de ligar à origem de dados, vá para o **adicionar a pesquisa cognitiva** passo opcional. Esta é a segunda etapa do assistente.

1. Expanda a **anexar serviços cognitivos** secção. Este passo mostra-lhe todos os recursos dos serviços cognitivos que tiver nas mesmas regiões que o serviço Azure Search.

   ![Expandir a secção de anexar serviços cognitivos](./media/cognitive-search-attach-cognitive-services/attach1.png "expandido anexar serviços cognitivos")

1. Selecione um recurso de serviços cognitivos existente ou selecione **criar novo recurso de serviços cognitivos**. Se selecionou **gratuito (limitado possível)**, será possível enriquecer somente um pequeno número de documentos para gratuita (20 documentos por dia). Se selecionou **criar novo recurso de serviços cognitivos**, abrirá um novo separador onde pode criar o recurso. 

1. Se tiver criado um novo recurso, selecione **atualizar** para atualizar a lista de recursos dos serviços cognitivos e, em seguida, selecione o recurso. 

   ![Selecionar recurso dos serviços cognitivos](./media/cognitive-search-attach-cognitive-services/attach2.png "selecionado recurso de serviço dos serviços cognitivos")

1. Expanda a **possível adicionar** secção para selecionar as capacidades cognitivas específicas que pretende executar com os seus dados e prosseguir com o resto do fluxo.

## <a name="associate-a-cognitive-services-resource-with-an-existing-skillset"></a>Associar um recurso dos serviços cognitivos com um conjunto de competências existente

1. Sobre o **descrição geral do serviço** página, selecione a **conjuntos de competências** separador.

   ![Separador conjuntos de competências](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "separador conjuntos de competências")

1. Selecione o conjunto de capacidades que pretende modificar. Este passo é aberto um painel onde pode editar um conjunto de capacidades.

1. Selecione um recurso de serviços cognitivos existente ou selecione **criar novo recurso de serviços cognitivos**. Se selecionou **gratuito (limitado possível)**, será possível enriquecer somente um pequeno número de documentos para gratuita (20 documentos por dia). Se selecionou **criar novo recurso de serviços cognitivos**, abrirá um novo separador onde pode criar o recurso.

   <n/> 
   <img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Se tiver criado um novo recurso, selecione **atualizar** para atualizar a lista de recursos dos serviços cognitivos e, em seguida, selecione o recurso.

1. Selecione **OK** para confirmar as alterações.

## <a name="associate-a-cognitive-services-resource-programmatically"></a>Associar um recurso dos serviços cognitivos através de programação

Quando está a definir o conjunto de capacidades programaticamente, adicione um `cognitiveServices` secção. Na secção, inclua a chave do recurso dos serviços cognitivos que pretende associar o conjunto de capacidades. Também incluem `@odata.type`e defina-o como `#Microsoft.Azure.Search.CognitiveServicesByKey`. O exemplo seguinte mostra este padrão.

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
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimate-the-cost-of-document-cracking-and-enrichment"></a>Exemplo: Calcular o custo de aberturas de documentos e enriquecimento
Pode querer fazer uma estimativa quanto custa para enriquecer um tipo de documento. O exercício seguinte é apenas um exemplo, mas poderá ser útil para.

Imagine que tenha 1.000 PDFs. Estimar o que em média, cada um desses documentos tem 6 páginas. Cada página tem 1 imagem. Em média, há cerca de 3000 carateres por página. 

Agora, suponha que deseja realizar os passos seguintes como parte do pipeline enriquecimento:
1. Como parte de aberturas de documentos, extraia as imagens e o conteúdo do documento.
1. Como parte do melhoramento, utilize o reconhecimento ótico de carateres (OCR) para cada uma das páginas extraídas, combinar o texto para todas as páginas e, em seguida, extraia cada uma das organizações no texto combinado de todas as imagens.

Vamos fazer uma estimativa de quanto iria custar para ingerir os 1000 documentos, passo a passo:

1. Para aberturas de documentos, teria de extrair um número combinado de 6.000 imagens. Partindo do princípio de US $1 para cada 1000 imagens extraídas, que iria custar a US $6.00.

2. Poderia extrair o texto de cada uma dessas 6.000 imagens. Em inglês, a habilidade de cognitiva OCR utiliza o melhor algoritmo (DescribeText). Supondo que um custo de US $2.50 por 1000 imagens a ser analisados, pagaria us $15.00 para este passo.

3. Para extração de entidades, teria um total de 3 registos de texto por página. (Cada registo é de 1000 carateres.) Três registos de texto por página * 6.000 páginas = 18,000 registos de texto. Partindo do princípio de US $2,00 por 1000 registos de texto, este passo iria custar us $36.00.

Juntando as peças, pagaria us $57.00 ingerir 1000 documentos PDF dessa natureza com o conjunto de capacidades descrito. Neste exercício, assumimos que o preço mais caro por transação. Poderia ter sido inferior devido a graduando preços. Ver [preços dos serviços cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Passos Seguintes
+ [O Azure Search a página de preços](https://azure.microsoft.com/pricing/details/search/)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
+ [Criar conjunto de capacidades (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Como mapear campos plena](cognitive-search-output-field-mapping.md)
