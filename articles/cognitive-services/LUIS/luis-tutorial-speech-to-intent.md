---
title: Utilizar o reconhecimento de voz c# SDK com LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Utilize o exemplo de reconhecimento de voz c# SDK enunciar para microfone e obter predições de intenção e entidades do LUIS devolvidas.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/26/2018
ms.author: v-geberr;
ms.openlocfilehash: b681598f953d217ca636fb5c0adc3de4ddbebd60
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031792"
---
# <a name="integrate-speech-service"></a>Integrar o serviço de reconhecimento de voz
O [serviço de reconhecimento de voz](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) permite-lhe utilizar um único pedido a receber de áudio e devolver predição LUIS objetos JSON.

Neste artigo, transfira e utilize um projeto c# no Visual Studio para enunciar um utterance para um microfone e receber informações de predição de LUIS. O projeto utiliza o reconhecimento de voz [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) pacote, já incluído como uma referência. 

Para este artigo, precisa de um livre [LUIS] [ LUIS] conta de Web site para importar a aplicação.

## <a name="create-luis-endpoint-key"></a>Criar a chave de ponto final de LUIS
No portal do Azure, [criar](luis-how-to-azure-subscription.md#create-luis-endpoint-key) um **compreensão de idiomas** chave (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importar recursos humanos LUIS aplicação
Utterances para este artigo e de pendentes são da aplicação disponível a partir dos recursos humanos LUIS o [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) repositório do Github. Transferir o [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) ficheiro, guarde-o com a extensão de *. JSON e [importar](create-new-app.md#import-new-app) para LUIS. 

Esta aplicação tem pendentes, as entidades e utterances relacionados com o domínio de recursos humanos. Utterances de exemplo incluem:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Adicionar KeyPhrase prebuilt entidade
Depois de importar a aplicação, selecione **entidades**, em seguida, **gerir entidades prebuilt**. Adicionar o **KeyPhrase** entidade. A entidade de KeyPhrase extrai chave peritos do utterance.

## <a name="train-and-publish-the-app"></a>Dar formação e publicar a aplicação
1. Na barra de navegação superior, direita, selecione o **preparar** botão para preparar a aplicação de LUIS.

2. Selecione **publicar** para ir para a página de publicar. 

3. Na parte inferior do **publicar** página, adicione a chave de LUIS criada no [chave de ponto final de criar LUIS](#create-luis-endpoint-key) secção.

4. Publicar a aplicação de LUIS selecionando o **publicar** botão à direita da ranhura de publicar. 

  No **publicar** página, recolher o ID da aplicação, publicar a região e o ID de subscrição da chave LUIS criado no [chave de ponto final de criar LUIS](#create-luis-endpoint-key) secção. Terá de modificar o código para utilizar estes valores neste artigo. 

  Estes valores são incluídos no URL do ponto final na parte inferior do **publicar** página para a chave que criou. 
  
  Efetue **não** utilizar a chave de arranque livre para este exercício. Apenas um **compreensão de idiomas** chave criada no portal do Azure irá funcionar para este exercício. 

  https://**região**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? chave de subscrição =**LUISKEY**& q =

## <a name="audio-device"></a>Dispositivo de áudio
Este artigo utiliza o dispositivo de áudio no seu computador. Que pode ser um headset com microfone ou um dispositivo de áudio incorporado. Verifique os níveis de entrada de áudio para ver se deve enunciar louder que faria normalmente com a sua voz detetada pelo dispositivo de áudio. 

## <a name="download-the-luis-sample-project"></a>Transferir o projeto de exemplo LUIS
 Clonar ou transferir o [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) repositório. Abra o [voz ao projeto intenção](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) com o Visual Studio e restaurar os pacotes NuGet. O ficheiro de solução VS é.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

O SDK de reconhecimento de voz já está incluído como referência. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Captura de ecrã do Visual Studio 2017 apresentação Microsoft.CognitiveServices.Speech NuGet pacote")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Modificar o código c#
Abra o **LUIS_samples.cs** de ficheiros e alterar as seguintes variáveis:

|Nome da variável|Objetivo|
|--|--|
|luisSubscriptionKey|Corresponde ao valor de chave de subscrição de URL de ponto final a partir da página de publicar|
|luisRegion|Corresponde a subdomínio primeiro URL de ponto final|
|luisAppId|Corresponde a rota de URL de ponto final seguir **aplicações /**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Captura de ecrã do Visual Studio 2017 apresentar LUIS_samples.cs variáveis")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

O ficheiro já tem os pendentes de recursos humanos mapeados.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Captura de ecrã do Visual Studio 2017 apresentar LUIS_samples.cs pendentes")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Crie e execute a aplicação. 

## <a name="test-code-with-utterance"></a>Código de teste com utterance
Selecione **1** e enunciar para microfone "Que é o Gestor de João Silva".

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

O SDK de reconhecimento de voz devolve a resposta de LUIS completa. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário elimine a aplicação de LUIS RecursosHumanos. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

Lembre-se ao eliminar o diretório de exemplos LUIS quando tiver terminado com o código de exemplo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Integrar LUIS com um BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website