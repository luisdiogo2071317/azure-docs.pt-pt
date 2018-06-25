---
title: Etiqueta entidades automaticamente com uma entidade de lista utilizando Nodejs | Microsoft Docs
description: Saiba como adicionar uma entidade de lista para ajudar a variações de etiqueta LUIS de uma palavra ou expressão.
services: cognitive-services
author: v-geberr
titleSuffix: Azure
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: e8558ecf4a64dbccef6e6367c1447bdcdb005126
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "35351776"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Utilize uma entidade de lista para aumentar a deteção de entidade 
Este tutorial demonstra a utilização de um [lista entidade](luis-concept-entity-types.md) para aumentar a deteção de entidade. Lista de entidades não precisam de ser de etiqueta conforme forem uma correspondência exata dos termos de licenciamento.  

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Criar uma entidade de lista 
* Adicionar sinónimos e valores normalizados
* Validar a identificação de entidade melhorada

## <a name="prerequisites"></a>Pré-requisitos

> [!div class="checklist"]
> * Mais recente [Node.js](https://nodejs.org)
> * [Aplicação de LUIS HomeAutomation](luis-get-started-create-app.md). Se não tiver a aplicação de home page de automatização criada, criar uma nova aplicação e adicione o domínio Prebuilt **HomeAutomation**. Dar formação e publicar a aplicação. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (se consultar demasiadas vezes), ID de aplicação, ID de versão, e [região](luis-reference-regions.md) para a aplicação de LUIS.

> [!Tip]
> Se ainda não tiver uma subscrição, pode registar-se para uma [conta gratuita](https://azure.microsoft.com/free/).

Todo o código neste tutorial de está disponível na [repositório do github LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-list-entity). 

## <a name="use-homeautomation-app"></a>Utilizar a aplicação HomeAutomation
Oferece de aplicação HomeAutomation controlo dos dispositivos como lights, sistemas de entretenimento e ambiente controla como heating e arrefecimento. Estes sistemas tem vários nomes diferentes que podem incluir nomes, nicknames, utilização de acrónimos e slang do fabricante. 

Um sistema de nomes de muitos em idiomas diferentes e a demografia é o thermostat. Um thermostat pode controlar arrefecimento e heating sistemas para um próxima ou edifício.

Idealmente, as seguintes utterances devem resolver para a entidade Prebuilt **HomeAutomation.Device**:

|#|utterance|entidade identificada|modelo de pontuação|
|--|--|--|--|
|1|Ative a ac|HomeAutomation.Device - "ac"|0.8748562|
|2|Ativar a cópia de segurança o térmico|HomeAutomation.Device - "térmico"|0.784990132|
|3|Certifique-colder|||

As duas primeiras utterances mapeiam para vários dispositivos. O terceiro utterance, "torná-lo colder", não mapear para um dispositivo, mas em vez disso, os pedidos de um resultado. LUIS não sabe que o termo "colder", significa que o thermostat o dispositivo pedido. Idealmente, LUIS deve resolver todas estas utterances ao mesmo dispositivo. 

## <a name="use-a-list-entity"></a>Utilize uma entidade de lista
A entidade de HomeAutomation.Device é excelente para um pequeno número de dispositivos ou variações alguns dos nomes. Para um edifício de escritórios ou campus, os nomes de dispositivo aumentam além a utilidade da entidade de HomeAutomation.Device. 

A **lista entidade** é uma boa opção para este cenário porque o conjunto de termos de licenciamento para um dispositivo num edifício ou campus é um conjunto conhecido, mesmo se for um grande conjunto. Ao utilizar uma entidade de lista, LUIS pode receber qualquer valor possível no conjunto para o thermostat e resolva-a para baixo para apenas o dispositivo "thermostat". 

Este tutorial vai criar uma lista de entidade com o thermostat. Os nomes alternativos para um thermostat neste tutorial são: 

|nomes alternativos para thermostat|
|--|
| AC |
| a/c|
| -c|
|heater|
|acesso frequente|
|hotter|
|frio|
|colder|

Se LUIS tem de determinar muitas vezes, uma alternativa nova, em seguida, um [lista frase](luis-concept-feature.md#how-to-use-phrase-lists) é uma melhor resposta.

## <a name="create-a-list-entity"></a>Criar uma entidade de lista
Criar um ficheiro de Node.js e copie o seguinte código para a mesma. Altere os valores authoringKey, appId, versionId e região.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

Utilize o seguinte comando para instalar as dependências NPM e executar o código para criar a entidade de lista:

```Javascript
npm install && node add-entity-list.js
```

O resultado da execução é o ID da entidade lista:

```Javascript
026e92b3-4834-484f-8608-6114a83b03a6
```
## <a name="train-the-model"></a>Dar formação sobre o modelo
Preparar LUIS por ordem para a nova lista afetar os resultados da consulta. Formação é um processo de duas partes de formação, em seguida, verificar estado se a formação é efetuada. Uma aplicação com muitos modelos pode demorar alguns momentos para preparar. O seguinte código trains a aplicação, em seguida, aguarda até que a formação é efetuada com êxito. O código utiliza uma estratégia de espera e repetição para evitar a 429 "demasiados muitos pedidos" erro. 

Criar um ficheiro de Node.js e copie o seguinte código para a mesma. Altere os valores authoringKey, appId, versionId e região.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Utilize o seguinte comando para executar o código para preparar a aplicação:

```Javascript
node train.js
```

O resultado da execução é o estado de cada iteração de formação dos modelos de LUIS. A execução seguinte necessária apenas uma verificação de formação:

```Javascript
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

```
## <a name="publish-the-model"></a>Publicar o modelo
Publica para a entidade de lista está disponível a partir do ponto final.

Criar um ficheiro de Node.js e copie o seguinte código para a mesma. Altere os valores endpointKey, appId e região. Pode utilizar o authoringKey se não pretender ligar este ficheiro além do seu limite de quota.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Utilize o seguinte comando para executar o código para a aplicação de consulta:

```Javascript
node publish.js
```

O seguinte resultado inclui o url de ponto final para todas as consultas. Resultados JSON reais inclui o appID real. 

```JSON
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'westus',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>A aplicação de consulta 
Consulta a aplicação a partir do ponto final para provar que a entidade de lista ajuda LUIS determinar o tipo de dispositivo.

Criar um ficheiro de Node.js e copie o seguinte código para a mesma. Altere os valores endpointKey, appId e região. Pode utilizar o authoringKey se não pretender ligar este ficheiro além do seu limite de quota.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Utilize o seguinte comando para executar o código e a aplicação de consulta:

```Javascript
node train.js
```

O resultado é que os resultados da consulta. Porque o código adicionado o **verboso** par nome/valor para a cadeia de consulta, o resultado inclui todos os pendentes e os respetivos pontuações:

```JSON
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

O dispositivo específico de **Thermostat** identificada com uma consulta orientado para o resultado de "ativar o térmico de cópia de segurança". Uma vez que a entidade de HomeAutomation.Device original ainda na aplicação, pode ver, bem como dos resultados. 

Tente as outros dois utterances para ver o que também são devolvidos como um thermostat. 

|#|utterance|entidade|tipo|valor|
|--|--|--|--|--|
|1|Ative a ac| AC | DevicesList | Thermostat|
|2|Ativar a cópia de segurança o térmico|térmico| DevicesList |Thermostat|
|3|Certifique-colder|colder|DevicesList|Thermostat|

## <a name="next-steps"></a>Passos Seguintes

Pode criar outra entidade de lista para expandir as localizações de dispositivo gabinetes, floors ou edifícios. 
