---
title: Criar uma aplicação de LUIS através de programação utilizando Node.js | Microsoft Docs
titleSuffix: Azure
description: Saiba como criar uma aplicação LUIS programaticamente a partir dos dados preexistentes em formato CSV através da API de criação de LUIS.
services: cognitive-services
author: DeniseMak
manager: rstand
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: 09c9d4da835b7b30fd132770f9d13b33fa80a3f5
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268331"
---
# <a name="build-a-luis-app-programmatically-using-nodejs"></a>Criar uma aplicação de LUIS através de programação utilizando Node.js

LUIS fornece uma API programática que tudo o que o [LUIS] [ LUIS] does do Web site. Isto permite poupar tempo, quando tiver dados previamente existentes e seria mais rápido para criar uma aplicação de LUIS através de programação que introduzindo informações manualmente. 

## <a name="prerequisites"></a>Pré-requisitos

* Inicie sessão no [LUIS] [ LUIS] Web site e localizar o [criação chave](luis-concept-keys.md#authoring-key) nas definições de conta. Esta chave é utilizada para chamar as APIs de criação.
* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* Este tutorial começa com um CSV para ficheiros de registo de uma empresa hipotético de pedidos de utilizador. Transferi-lo [aqui](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/IoT.csv).
* Instale o Node.js mais recente com NPM. Descarregá-lo no [aqui](https://nodejs.org/en/download/).
* **[Recomendado]**  Visual Studio Code para IntelliSense e depuração, descarregá-lo no [aqui](https://code.visualstudio.com/) gratuitamente.

## <a name="map-preexisting-data-to-intents-and-entities"></a>Mapear dados pré-existentes para entidades e pendentes
Mesmo se tiver um sistema que não foi criado com LUIS em mente, se contiver dados textual que mapeia para os utilizadores de diversos elementos quiser fazê-lo, poderá detetar um com um mapeamento das categorias existentes da entrada de utilizador para pendentes no LUIS. Se pode identificar importantes palavras ou frases reconhecíveis em que os utilizadores consiga aceder tal, estas palavras podem mapear para entidades.

Abra o ficheiro `IoT.csv`. Contém um registo de consultas de utilizador a um serviço de automatização inicial hipotético, incluindo como foram categorizados, o que o utilizador consiga aceder tal e algumas colunas com informações úteis solicitadas fora-los. 

![Ficheiro CSV](./media/luis-tutorial-node-import-utterances-csv/csv.png) 

Verá que o **RequestType** coluna pode ser pendentes e o **pedido** coluna mostra um utterance de exemplo. Os outros campos poderá ser entidades se ocorrem no utterance. Porque existem utterances de exemplo, as entidades e pendentes, tem os requisitos para uma aplicação de exemplo simples.

## <a name="steps-to-generate-a-luis-app-from-non-luis-data"></a>Passos para gerar uma aplicação LUIS não LUIS dados
Para gerar uma nova aplicação LUIS a partir do ficheiro de origem, primeiro pode analisar os dados do ficheiro CSV e converter estes dados num formato que pode carregar LUIS utilizando a API de criação. Os dados analisados, recolha informações em que as entidades e pendentes existem. Em seguida, efetuar chamadas de API para criar a aplicação e adicionar pendentes e entidades que foram recolhidas a partir dos dados analisados. Assim que tiver criado a aplicação de LUIS, pode adicionar os utterances de exemplo de dados analisados. Pode ver este fluxo na última parte do código seguinte. Copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/index.js) este código e guardá-lo no `index.js`.

   [!code-javascript[Node.js code for calling the steps to build a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/index.js)]


## <a name="parse-the-csv"></a>Analisar o CSV

As entradas de coluna que contém os utterances no CSV tem de ser analisada para um formato JSON LUIS possa compreender. Este formato JSON tem de conter um `intentName` campo que identifica a intenção de utterance. Também tem de conter um `entityLabels` campo, o que pode estar vazio se não existem não existem entidades o utterance. 

Por exemplo, a entrada para "Ativar os lights" é mapeado para este JSON:

```json
        {
            "text": "Turn on the lights",
            "intentName": "TurnOn",
            "entityLabels": [
                {
                    "entityName": "Operation",
                    "startCharIndex": 5,
                    "endCharIndex": 6
                },
                {
                    "entityName": "Device",
                    "startCharIndex": 12,
                    "endCharIndex": 17
                }
            ]
        }
```

Neste exemplo, o `intentName` é fornecido no pedido do utilizador no **pedido** no cabeçalho de coluna no ficheiro CSV e o `entityName` provém de outras colunas com informações da chave. Por exemplo, se existir uma entrada para **operação** ou **dispositivo**e que também ocorre cadeia no pedido real, em seguida, que pode ser identificado como uma entidade. O código seguinte demonstra esta análise do processo. Pode copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_parse.js) -lo e guardá-lo para `_parse.js`.

   [!code-javascript[Node.js code for parsing a CSV file to extract intents, entities, and labeled utterances](~/samples-luis/examples/build-app-programmatically-csv/_parse.js)]



## <a name="create-the-luis-app"></a>Criar a aplicação de LUIS
Assim que tiverem foi analisados os dados em JSON, adicione-a uma aplicação LUIS. O código seguinte cria a aplicação de LUIS. Copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_create.js) e guardá-lo para `_create.js`.

   [!code-javascript[Node.js code for creating a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/_create.js)]


## <a name="add-intents"></a>Adicionar intenções
Assim que tiver uma aplicação, terá pendentes ao mesmo. O código seguinte cria a aplicação de LUIS. Copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_intents.js) e guardá-lo para `_intents.js`.

   [!code-javascript[Node.js code for creating a series of intents](~/samples-luis/examples/build-app-programmatically-csv/_intents.js)]


## <a name="add-entities"></a>Adicionar entidades
O seguinte código adiciona as entidades para a aplicação de LUIS. Copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_entities.js) e guardá-lo para `_entities.js`.

   [!code-javascript[Node.js code for creating entities](~/samples-luis/examples/build-app-programmatically-csv/_entities.js)]
   


## <a name="add-utterances"></a>Adicionar expressões
Depois das entidades e pendentes terem sido definidas na aplicação LUIS, pode adicionar os utterances. O seguinte código utiliza o [Utterances_AddBatch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) API, o que lhe permite adicionar até 100 utterances cada vez.  Copiar ou [transferir](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_upload.js) e guardá-lo para `_upload.js`.

   [!code-javascript[Node.js code for adding utterances](~/samples-luis/examples/build-app-programmatically-csv/_upload.js)]


## <a name="run-the-code"></a>Executar o código


### <a name="install-nodejs-dependencies"></a>Instalar Node.js dependências
Instale as dependências do Node.js a partir de NPM na linha de comando/terminal.

````
> npm install
````

### <a name="change-configuration-settings"></a>Alterar as definições de configuração
Para utilizar esta aplicação, terá de alterar os valores no ficheiro index.js a sua própria chave de subscrição e forneça o nome que pretende que a aplicação ter. Também pode definir o idioma da aplicação ou alterar o número de versão.

Abra o ficheiro de index.js e alterar estes valores na parte superior do ficheiro.


````JavaScript
// Change these values
const LUIS_programmaticKey = "YOUR_PROGRAMMATIC_KEY";
const LUIS_appName = "Sample App";
const LUIS_appCulture = "en-us"; 
const LUIS_versionId = "0.1";
````
### <a name="run-the-script"></a>Execute o script
Execute o script a partir de uma linha de comando/terminal com o Node.js.

````
> node index.js
````
ou
````
> npm start
````

### <a name="application-progress"></a>Progresso da aplicação
Enquanto a aplicação está em execução, a linha de comandos mostra o progresso. A saída da linha de comando inclui o formato de respostas de LUIS.

````
> node index.js
intents: ["TurnOn","TurnOff","Dim","Other"]
entities: ["Operation","Device","Room"]
parse done
JSON file should contain utterances. Next step is to create an app with the intents and entities it found.
Called createApp, created app with ID 314b306c-0033-4e09-92ab-94fe5ed158a2
Called addIntents for intent named TurnOn.
Called addIntents for intent named TurnOff.
Called addIntents for intent named Dim.
Called addIntents for intent named Other.
Results of all calls to addIntent = [{"response":"e7eaf224-8c61-44ed-a6b0-2ab4dc56f1d0"},{"response":"a8a17efd-f01c-488d-ad44-a31a818cf7d7"},{"response":"bc7c32fc-14a0-4b72-bad4-d345d807f965"},{"response":"727a8d73-cd3b-4096-bc8d-d7cfba12eb44"}]
called addEntity for entity named Operation.
called addEntity for entity named Device.
called addEntity for entity named Room.
Results of all calls to addEntity= [{"response":"6a7e914f-911d-4c6c-a5bc-377afdce4390"},{"response":"56c35237-593d-47f6-9d01-2912fa488760"},{"response":"f1dd440c-2ce3-4a20-a817-a57273f169f3"}]
retrying add examples...

Results of add utterances = [{"response":[{"value":{"UtteranceText":"turn on the lights","ExampleId":-67649},"hasError":false},{"value":{"UtteranceText":"turn the heat on","ExampleId":-69067},"hasError":false},{"value":{"UtteranceText":"switch on the kitchen fan","ExampleId":-3395901},"hasError":false},{"value":{"UtteranceText":"turn off bedroom lights","ExampleId":-85402},"hasError":false},{"value":{"UtteranceText":"turn off air conditioning","ExampleId":-8991572},"hasError":false},{"value":{"UtteranceText":"kill the lights","ExampleId":-70124},"hasError":false},{"value":{"UtteranceText":"dim the lights","ExampleId":-174358},"hasError":false},{"value":{"UtteranceText":"hi how are you","ExampleId":-143722},"hasError":false},{"value":{"UtteranceText":"answer the phone","ExampleId":-69939},"hasError":false},{"value":{"UtteranceText":"are you there","ExampleId":-149588},"hasError":false},{"value":{"UtteranceText":"help","ExampleId":-81949},"hasError":false},{"value":{"UtteranceText":"testing the circuit","ExampleId":-11548708},"hasError":false}]}]
upload done
````




## <a name="open-the-luis-app"></a>Abra a aplicação de LUIS
Após a conclusão do script, pode iniciar sessão [LUIS] [ LUIS] e ver a aplicação de LUIS que criou em **aplicações My**. Deve ser capaz de ver utterances adicionado sob o **TurnOn**, **TurnOff**, e **nenhum** pendentes.

![Intenção de TurnOn](./media/luis-tutorial-node-import-utterances-csv/imported-utterances-661.png)


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Testar e preparar a sua aplicação no Web site de LUIS](interactive-test.md)

## <a name="additional-resources"></a>Recursos adicionais

Esta aplicação de exemplo utiliza os APIs de LUIS seguintes:
- [Criar aplicação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)
- [Adicionar pendentes](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0c)
- [Adicionar entidades](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0e) 
- [Adicionar utterances](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

