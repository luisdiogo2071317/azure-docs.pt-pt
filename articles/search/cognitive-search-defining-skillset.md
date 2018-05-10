---
title: Criar um skillset num pipeline pesquisa cognitivos (Azure Search) | Microsoft Docs
description: Definir a extração de dados, linguagem natural a processar, ou passos de análise de imagem para enriqueça e extrair informações estruturadas dos seus dados para utilizam na Azure Search.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 3ab35cfd8ce5cf54a68473736fe05b78d26850de
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Como criar um skillset um pipeline sem causa

Pesquisa cognitivos extrai e otimiza os dados para tomar pesquisável na Azure Search. Chamamos passos de extração e sem causa *competências cognitivos*, combinada para um *skillset* referenciado durante a indexação. Pode utilizar um skillset [predefinidas competências](cognitive-search-predefined-skills.md) ou competências personalizadas (consulte [exemplo: criar um skill personalizado](cognitive-search-create-custom-skill-example.md) para obter mais informações).

Neste artigo, irá aprender a criar um pipeline sem causa para as capacidades que pretende utilizar. Um skillset está ligado a um Azure Search [indexador](search-indexer-overview.md). Uma parte da estrutura de pipeline, abordada neste artigo, é construir skillset próprio. 

> [!NOTE]
> Outra parte do design do pipeline está especificando um indexador, abrangido o [passo seguinte](#next-step). Uma definição de indexador inclui uma referência a skillset Plus mapeamentos campo utilizados para ligar entradas para saídas no índice de destino.

Chave aponta para Lembre-se:

+ Só pode ter um skillset por indexador.
+ Um skillset tem de ter pelo menos um skill.
+ Pode criar várias competências do mesmo tipo (por exemplo, variantes skill de análise uma imagem), mas cada skill só pode ser utilizado uma vez dentro do mesmo skillset.

## <a name="begin-with-the-end-in-mind"></a>Começar com o fim em mente

Um passo recomendado inicial é decidir quais os dados para extrair dados não processados e a forma como pretende utilizar esses dados numa solução de pesquisa. Criar uma ilustração do pipeline completo sem causa pode ajudar a identificar os passos necessários.

Suponha que está interessado no processamento de um conjunto de comentários do analista financeiros. Para cada ficheiro que pretende extrair os nomes de empresa e o sentimento geral dos comentários. Pode também querer escrever um enricher personalizada que utiliza o serviço de pesquisa do Bing entidade para encontrar informações adicionais sobre a empresa, tais como o tipo de negócio da empresa é parte da. Essencialmente, que pretende extrair informações semelhante ao seguinte, indexado para cada documento:

| texto de registo | Empresas | Sentiment | descrições da empresa |
|--------|-----|-----|-----|
|registo de exemplo| ["Microsoft", "LinkedIn"] | 0,99 | ["Microsoft Corporation é uma empresa de tecnologia multinational American …", "LinkedIn é um e sua admissão-orientados para negócios redes sociais de rede..."]

O diagrama seguinte ilustra um pipeline sem hipotético causa:

![Um pipeline sem causa hipotético](media/cognitive-search-defining-skillset/sample-skillset.png "um pipeline sem causa hipotético")


Assim que tiver ideia justa de que pretende no pipeline, pode express skillset que fornece estes passos. Funcionalmente, o skillset é expresso ao carregar a definição de indexador para a Azure Search. Para obter mais informações sobre como carregar o indexador, consulte o [indexador documentação](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


No diagrama, a *cracking documento* passo ocorre automaticamente. Essencialmente, a Azure Search sabe como abrir ficheiros bem conhecidos e cria um *conteúdo* campo que contém o texto extraído de cada documento. As caixas em brancos são enrichers incorporadas e a caixa "Bing pesquisa de entidade" delimitada por pontos representa um enricher personalizada que está a criar. Conforme ilustrado, o skillset contém três competências.

## <a name="skillset-definition-in-rest"></a>Definição de Skillset no REST

Um skillset está definido como uma matriz de competências. Cada skill define a origem das respetivas entradas e o nome das saídas produzidos. Utilizar o [criar API REST do Skillset](ref-create-skillset.md), pode definir um skillset que corresponde ao diagrama anterior: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar",
      },
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Criar um skillset

Ao criar um skillset, pode fornecer uma descrição para tornar o skillset Self-documentação. Uma descrição é opcional, mas útil para controlar o que faz uma skillset. Como skillset é um documento JSON, que permite que os comentários, tem de utilizar um `description` elemento para este.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

A seguinte informação no skillset é uma matriz de competências. Pode considerar cada skill como uma primitiva de sem causa. Cada skill executa uma tarefa de pequena neste pipeline sem causa. Cada um deles tem uma entrada (ou um conjunto de entradas) e devolve algumas saídas. As secções seguintes alguns focar-se sobre como especificar competências predefinidas e personalizadas, encadeamento competências em conjunto através de referências de entrada e de saída. Entradas podem ter de dados de origem ou de outro skill. Saídas podem ser mapeadas para um campo no índice de pesquisa ou utilizadas como uma entrada para um skill a jusante.

## <a name="add-predefined-skills"></a>Adicionar competências predefinidas

Vamos ver skill primeiro, o que é o predefinido [denominado skill de reconhecimento de entidade](cognitive-search-skill-named-entity-recognition.md):

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Cada skill predefinida tem `odata.type`, `input`, e `output` propriedades. Propriedades específicas do skill fornecem informações adicionais aplicáveis a esse skill. Para reconhecimento de entidade, `categories` é uma entidade entre um conjunto de tipos de entidade que consegue reconhecer o modelo pretrained fixo.

* Cada skill deve ter um ```"context"```. O contexto representa o nível no qual as operações ocorrer. No skill acima, o contexto é todo o documento, que significa que o skill de reconhecimento de entidade com o nome é chamado uma vez por documento. Saídas também são produzidas nesse nível. Mais especificamente, ```"organizations"``` são gerados como membro do ```"/document"```. A jusante competências, pode fazer referência a este recém-criado informações como ```"/document/organizations"```.  Se o ```"context"``` campo não for explicitamente definido, o contexto predefinido é o documento.

* O skill tem uma entrada chamada "text", com um conjunto de entrada de origem para ```"/document/content"```. Skill (com o nome reconhecimento de entidade) funciona com o *conteúdo* campo de cada documento, o que é um campo padrão criados pelo indexador de Blobs do Azure. 

* O skill tem uma saída chamada ```"organizations"```. Saídas existem apenas durante o processamento. Encadear este resultado para a entrada de um skill a jusante, referenciar o resultado como ```"/document/organizations"```.

* Para um determinado documento, o valor de ```"/document/organizations"``` é uma matriz das organizações extraídos a partir do texto. Por exemplo:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Algumas situações chamada para referenciar a cada elemento de uma matriz separadamente. Por exemplo, suponha que pretende passar cada elemento da ```"/document/organizations"``` separadamente para outro skill (por exemplo, o enricher pesquisa de entidade da Bing personalizado). Pode consultar a cada elemento da matriz ao adicionar um asterisco ao caminho: ```"/document/organizations/*"``` 

O segundo skill para extração de dados de sentimento acompanha o mesmo padrão, como o primeiro enricher. Demora ```"/document/content"``` como entrada e devolve um sentimento Pontuar para cada instância de conteúdo. Uma vez que não tenha definido a ```"context"``` campo explicitamente, a saída (mySentiment) agora é um elemento subordinado ```"/document"```.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Adicionar um skill personalizado

Recuperar a estrutura de enricher de pesquisa de entidade de Bing personalizado:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar",
      }
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Esta definição é uma skill personalizado que chama uma API web como parte do processo de sem causa. Para cada organização identificada por reconhecimento de entidade com nome, este skill chama uma API web do encontrar a descrição da organização. A orquestração de quando a chamada da API web e como as informações que recebeu do fluxo é processada internamente pelo motor de sem causa. No entanto, a inicialização necessária para chamar esta API personalizada tem de ser fornecida no JSON (por exemplo, uri, httpHeaders e as entradas esperadas). Para obter orientações sobre como criar uma API de web personalizado para o pipeline sem causa, consulte [como definir uma interface personalizada](cognitive-search-custom-skill-interface.md).

Tenha em atenção que o campo "contexto" está definido como ```"/document/content/organizations/*"``` com um asterisco, o que significa que o passo de sem causa é chamado *para cada* organização em ```"/document/content/organizations"```. 

Saída, neste caso, uma descrição da empresa, é gerado para cada organização identificada. Ao fazer referência a descrição num passo (por exemplo, na extração da expressão de chave) a jusante, teria de utilizar o caminho ```"/document/content/organizations/*/description"``` para fazê-lo. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Enrichments criar a estrutura de informações não estruturadas

O skillset gera estruturadas informações fora de dados não estruturados. Considere o exemplo seguinte:

*"No respetivo quarto trimestre, Microsoft registado $ milhões de 1.1 no receitas de LinkedIn, a empresa de redes social comprou a último ano. A aquisição permite Microsoft combinar as capacidades de LinkedIn seu CRM e capacidades do Office. Stockholders são entusiasmados com o progresso até ao momento."*

Um resultado provável seria uma estrutura gerada semelhante à ilustração seguinte:

![Estrutura de saída de exemplo](media/cognitive-search-defining-skillset/enriched-doc.png "estrutura de saída de exemplo")

Recuperar a que esta estrutura é interna. Na verdade, não é possível obter este gráfico no código.

<a name="next-step"></a>
## <a name="next-steps"></a>Passos Seguintes

Agora que já está familiarizado com o pipeline de sem causa e skillsets, prossiga com [como fazer referência anotações de um skillset](cognitive-search-concept-annotations-syntax.md) ou [como mapear saídas para campos num índice](cognitive-search-output-field-mapping.md). 