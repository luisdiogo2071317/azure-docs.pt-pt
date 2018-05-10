---
title: Criar Skillset (REST api-version = 2017-11-11-pré-visualização)-a Azure Search | Microsoft Docs
description: Um skillset é uma coleção de competências cognitivos que compõem um pipeline sem causa.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a5277d4137ede5fe6dacf993413eefd7c9e46ad4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="create-skillset-api-version2017-11-11-preview"></a>Criar Skillset (api-version = 2017-11-11-pré-visualização)

**Aplica-se a:** api-version-2017-11-11-Preview

Um skillset é uma coleção de competências cognitivos utilizado para processamento de linguagem natural e outras transformações. As competências incluem extração entidade com nome, extração de expressão de chave, texto de agrupamento para páginas lógicas, entre outros.

Para utilizar o skillset, referencie-o num indexador de Azure Search e, em seguida, executar o indexador para importar dados, invocar transformações e sem causa e mapear os campos de saída para um índice. Um skillset é o recurso de alto nível, mas está operacional apenas dentro do processamento do indexador. Como um recurso de alto nível, pode conceber um skillset uma vez e, em seguida, referencie-o em vários indexadores. 

Um skillset é expresso na Azure Search através de um HTTP PUT ou um pedido POST. Para PUT, o corpo do pedido é um esquema JSON que especifica quais as competências são invocadas. As competências estão encadeadas em conjunto através de associações de entrada-saída, onde o resultado de uma transformação torna-se a entrada para outro.

Um skillset tem de ter pelo menos um skill. Não existe nenhum limite teórico do número máximo de competências, mas três a cinco é uma configuração comum.  


> [!NOTE]
> A Pesquisa Cognitiva encontra-se em pré-visualização pública e a execução do conjunto de competências é, de momento, disponibilizada gratuitamente. Posteriormente, iremos anunciar os preços desta capacidade.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```  

## <a name="request"></a>Pedir  
 HTTPS é necessário para todos os pedidos de serviço. O **criar Skillset** pedido pode ser construído utilizando um método PUT, com o nome de skillset como parte do URL. Se o skillset não existir, é criado. Se já existir, é atualizado para a definição de novo. Tenha em atenção que só pode colocar um skillset cada vez.  

 O nome de skillset tem de cumprir os seguintes requisitos:

- Estar em minúsculas
- Iniciar e terminar com uma letra ou um número
- Não ter barras nem pontos
- Tiver menos de 128 carateres 

Depois de iniciar o nome de skillset com uma letra ou número, o resto do nome pode incluir qualquer letra, o número e travessões, desde que os traços não são consecutivos.  

O **api-version** é necessário o parâmetro. A versão disponível apenas é `2017-11-11-Preview`. Consulte [versões de API na Azure Search](https://go.microsoft.com/fwlink/?linkid=834796) para obter uma lista de todas as versões. 


### <a name="request-headers"></a>Cabeçalhos do pedido  

 A tabela seguinte descreve os cabeçalhos de pedido necessários e opcionais.  

|Cabeçalho de pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*|Necessário. Defina esta opção para `application/json`|  
|*chave de API:*|Necessário. O `api-key` é utilizado para autenticar o pedido para o serviço de pesquisa. É um valor de cadeia, exclusivo para o seu serviço. O **criar Skillset** pedido tem de incluir um `api-key` cabeçalho definido para a chave de administrador (por oposição a uma chave de consulta).|  

Também tem o nome do serviço para construir o URL do pedido. Pode obter o nome do serviço e `api-key` a partir do seu dashboard de serviço no portal do Azure. Consulte [criar um serviço da Azure Search no portal do](search-create-service-portal.md) para obter ajuda na navegação página.  

### <a name="request-body-syntax"></a>Sintaxe de corpo do pedido  

O corpo do pedido contém a definição de skillset, constituída por um ou mais especificado totalmente competências, bem como parâmetros de nome e uma descrição opcionais.  

Segue-se a sintaxe para structuring o payload de pedido. Um exemplo de pedido é fornecido neste artigo bem como nas [como definir um skillset](cognitive-search-defining-skillset.md).  

```
{   
    "name" : "Required for POST, optional for PUT. Friendly name of the skillset",  
    "description" : "Optional. Anything you want, or null",  
    "Skills" : "Required. An array of skills. Each skill has an odata.type, name, input and output parameters",  
}  
```

### <a name="request-example"></a>Exemplo de pedido
 O exemplo seguinte cria um skillset utilizado para enriquecer uma coleção de documentos financeiros.

```http
PUT https://[servicename].search.windows.net/skillsets/financedocenricher?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

O corpo do pedido é um documento JSON. Este skillset específico utiliza dois competências substance de processamento de forma assíncrona, independentemente de `contents` como dois transformações diferentes. Em alternativa, pode direcionar o resultado de uma transformação seja a entrada de outro. Para obter mais informações, consulte [como definir um skillset](cognitive-search-defining-skillset.md).

```json
{
  "name": "financedocenricher",
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
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
  ]
}
```

## <a name="response"></a>Resposta  

 Para um pedido com êxito, deverá ver o código de estado "criado 201".  

 Por predefinição, o corpo da resposta irá conter o JSON para a definição de skillset que foi criada. No entanto, se o cabeçalho do pedido Prefer estiver definido como devolver = mínimo, o corpo da resposta estará vazio, e o código de estado de êxito serão "204 nenhum conteúdo" em vez de "criado 201". Isto acontece independentemente se o PUT ou POST é utilizado para criar o skillset.   

## <a name="see-also"></a>Consulte também

+ [Descrição geral de pesquisa cognitivos](cognitive-search-concept-intro.md)
+ [Início rápido: Tente cognitivos pesquisa](cognitive-search-quickstart-blob.md)
+ [Tutorial: Enriquecidos a indexação de blobs do Azure](cognitive-search-tutorial-blob.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Como mapear os campos](cognitive-search-output-field-mapping.md)
+ [Como definir uma interface personalizada](cognitive-search-custom-skill-interface.md)
+ [Exemplo: criar um skill personalizado](cognitive-search-create-custom-skill-example.md)
+ [Sklls predefinidas](cognitive-search-predefined-skills.md)