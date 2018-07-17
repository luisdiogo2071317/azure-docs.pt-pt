---
title: Utilize a voz para texto | Documentos da Microsoft
description: Saiba como utilizar a conversão de voz em texto no serviço de voz
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 26cecedfc3ad2d472b9686e25054fe08253cee77
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068527"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Utilize "Conversão de voz para texto" no serviço de voz

Pode usar **conversão de voz em texto** em seus aplicativos de duas formas diferentes.

| Método | Descrição |
|-|-|
| [SDK](speech-sdk.md) | Método mais simples para os desenvolvedores de C/C++, c# e Java |
| [REST](rest-apis.md) | Reconhecer expressões com curtos usando um pedido POST de HTTP | 

## <a name="using-the-sdk"></a>Utilizar o SDK

O [SDK de voz](speech-sdk.md) fornece a forma mais simples de usar **voz em texto** em seu aplicativo com funcionalidade completa.

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de voz e [região](regions.md) ou um token de autorização. Também pode configurar opções, como o idioma do reconhecimento ou um ponto final personalizado para seus próprios modelos de reconhecimento de voz, neste momento.

2. Obtenha um reconhecedor da fábrica. Três tipos diferentes de reconhecedores estão disponíveis. Cada tipo de reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

    Reconhecedor | Função
    -|-
    Reconhecedor de voz|Fornece a transcrição de texto de voz
    Reconhecedor intenção|Deriva a intenção de orador através de [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) depois de reconhecimento\*
    Reconhecedor de tradução|Traduz texto transcrito para outro idioma (consulte [tradução por voz](how-to-translate-speech.md))

    \* *Para o reconhecimento da intenção, terá de utilizar uma chave de subscrição separada do LUIS, ao criar uma fábrica de voz para o reconhecedor intenção.*
    
4. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais. Caso contrário, a aplicação irá receber um resultado final transcrição.

5. Inicie o reconhecimento.
   Para o reconhecimento de única, como reconhecimento de comando ou uma consulta, utilize `RecognizeAsync()`, que retorna a primeira expressão a ser reconhecido.
   Para o reconhecimento de longa execução, como transcrição, utilize `StartContinuousRecognitionAsync()` automático e ligar os eventos para resultados de reconhecimento assíncrono.

### <a name="sdk-samples"></a>Amostras de SDK

Para o conjunto mais recente de exemplos, consulte a [cognitivos GitHub de exemplo do SDK de voz de serviços repositório](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Com a API REST

A API REST é a forma mais simples para reconhecer a voz, se não estiver a utilizar um idioma suportado pelo SDK. Fazer um pedido HTTP POST para o ponto final de serviço, passando a expressão inteira no corpo do pedido. Receber uma resposta que contém o texto reconhecido.

> [!NOTE]
> Expressões com são limitadas para 15 segundos ou menos ao utilizar a API de REST.

Para obter mais informações sobre o **conversão de voz em texto** API de REST, veja [REST APIs](rest-apis.md#speech-to-text). Para ver em ação, transfira o [exemplos de REST API](https://github.com/Azure-Samples/SpeechToText-REST) do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer voz em C++](quickstart-cpp-windows.md)
- [Como reconhecer voz em C#](quickstart-csharp-dotnet-windows.md)
- [Como reconhecer voz em Java](quickstart-java-android.md)
