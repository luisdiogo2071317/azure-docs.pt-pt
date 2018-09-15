---
title: Utilize a voz c# SDK com os LUIS
titleSuffix: Azure Cognitive Services
description: O serviço de voz permite-lhe utilizar uma única solicitação receber áudio e retornar a predição de LUIS objetos JSON. Neste artigo, baixe e use um projeto c# no Visual Studio para falar de uma expressão num microfone e receber informações de predição de LUIS. O projeto utiliza o pacote NuGet de voz, já incluído como referência.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8eff6ff3d0263708158f2fea82380e88ba4638ad
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633632"
---
# <a name="integrate-speech-service"></a>Integre o serviço de voz
O [serviço de voz](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) permite-lhe utilizar uma única solicitação receber áudio e retornar a predição de LUIS objetos JSON. Neste artigo, baixe e use um projeto c# no Visual Studio para falar de uma expressão num microfone e receber informações de predição de LUIS. O projeto utiliza a voz [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) pacote, já incluída como referência. 

Neste artigo, terá um livre [LUIS] [ LUIS] conta de Web site para importar o aplicativo.

## <a name="create-luis-endpoint-key"></a>Criar uma chave de ponto final de LUIS
No portal do Azure, [crie](luis-how-to-azure-subscription.md#create-luis-endpoint-key) um **compreensão de idiomas** chave (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importar recursos humanos LUIS aplicação
Os objetivos e expressões com para este artigo são a partir da aplicação de recursos humanos LUIS disponível a partir da [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) repositório do Github. Transfira o [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) ficheiro, guarde-o com a extensão *. JSON, e [importar](luis-how-to-start-new-app.md#import-new-app) -lo para o LUIS. 

Esta aplicação tem intenções, entidades e expressões com relacionados com o domínio de recursos humanos. Expressões com de exemplo incluem:

|Expressões de exemplo|
|--|
|Quem é o Gestor de John Smith?|
|Que faça a gestão de John Smith?|
|Onde está o formulário 123456?|
|É necessário sempre que paga?|


## <a name="add-keyphrase-prebuilt-entity"></a>Adicionar KeyPhrase pré-criados de entidade
Depois de importar a aplicação, selecione **entidades**, em seguida, **gerir entidades pré-concebidas**. Adicionar a **KeyPhrase** entidade. A entidade de KeyPhrase extrai a chave de assunto da expressão.

## <a name="train-and-publish-the-app"></a>Preparar e publicar a aplicação
1. Na barra de navegação superior, certo, selecione o **treinar** botão para preparar a aplicação do LUIS.

2. Selecione **Manage** no canto superior direito da barra, em seguida, selecione **chaves e os pontos finais** na navegação à esquerda. 

3. Na **chaves e os pontos finais** página, atribua a chave de LUIS criada no [chave de ponto final de criar o LUIS](#create-luis-endpoint-key) secção.

  Nesta página, coletar o ID da aplicação, publicar a região e ID de subscrição da chave do LUIS criado no [chave de ponto final de criar o LUIS](#create-luis-endpoint-key) secção. Terá de modificar o código para utilizar estes valores, mais adiante neste artigo. 
  
  Fazer **não** usam a chave de iniciante gratuita para este exercício. Apenas um **compreensão de idiomas** chave criada no portal do Azure irá funcionar para este exercício. 

  https://**região**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? chave de subscrição =**LUISKEY**& q =


4. Publicar a aplicação do LUIS, selecionando o **publicar** botão no canto superior direito da barra. 

## <a name="audio-device"></a>Dispositivo de áudio
Este artigo utiliza o dispositivo de áudio no seu computador. Isso pode ser um headset com microfone ou um dispositivo de áudio incorporado. Verifique os níveis de entrada de áudio para ver se deve falar louder do que normalmente faria com que a sua voz detetado por parte do dispositivo de áudio. 

## <a name="download-the-luis-sample-project"></a>Transfira o projeto de exemplo do LUIS
 Clonar ou transferir os [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) repositório. Abra o [conversão de voz em intenção projeto](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) com o Visual Studio e restaurar os pacotes de NuGet. O ficheiro de solução de VS é.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

O SDK de voz já está incluído como referência. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Pacote de Microsoft.CognitiveServices.Speech NuGet mostrando de captura de ecrã do Visual Studio 2017")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Modificar o código do c#
Abra o **LUIS_samples.cs** de ficheiros e alterar as variáveis seguintes:

|Nome da variável|Objetivo|
|--|--|
|luisSubscriptionKey|Corresponde ao valor de chave de subscrição de URL de ponto final da página de publicação|
|luisRegion|Corresponde ao subdomínio do URL de ponto final de primeiro|
|luisAppId|Corresponde a rota de URL de ponto final a seguir **aplicações /**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Captura de ecrã do Visual Studio 2017 exibindo LUIS_samples.cs variáveis")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

O ficheiro já tiver dos objetivos de recursos humanos mapeados.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Captura de ecrã do Visual Studio 2017 exibindo LUIS_samples.cs intenções")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Crie e execute a aplicação. 

## <a name="test-code-with-utterance"></a>Código de teste com a expressão
Selecione **1** e fala sobre o microfone "Quem é o gerente do João Silva".

```cmd
1. Speech recognition of LUIS intent.
0. Stop.
Your choice: 1
LUIS...
Say something...
ResultId:cc83cebc9d6040d5956880bcdc5f5a98 Status:Recognized IntentId:<GetEmployeeOrgChart> Recognized text:<Who is the manager of John Smith?> Recognized Json:{"DisplayText":"Who is the manager of John Smith?","Duration":25700000,"Offset":9200000,"RecognitionStatus":"Success"}. LanguageUnderstandingJson:{
  "query": "Who is the manager of John Smith?",
  "topScoringIntent": {
    "intent": "GetEmployeeOrgChart",
    "score": 0.617331
  },
  "entities": [
    {
      "entity": "manager of john smith",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 31
    }
  ]
}

Recognition done. Your Choice:

```

A intenção correta, **GetEmployeeOrgChart**, foi encontrado com uma confiança de 61%. A entidade de keyPhrase foi devolvida. 

O SDK de voz devolve a resposta inteira do LUIS. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, elimine a aplicação do LUIS RecursosHumanos. Para tal, selecione o botão de reticências (***…***) à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

Não se esqueça de eliminar o diretório de exemplos de LUIS quando tiver terminado com o código de exemplo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Integrar o LUIS com um BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website