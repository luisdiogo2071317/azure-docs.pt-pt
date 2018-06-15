---
title: Utilize o reconhecimento de voz para texto | Microsoft Docs
description: Saiba como utilizar o reconhecimento de voz para texto no serviço de reconhecimento de voz
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 878a31992415b1f8688afcfb186fcd94ce2567b4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356231"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Utilize "Reconhecimento de voz para texto" no serviço de reconhecimento de voz

Pode utilizar **reconhecimento de voz para texto** nas suas aplicações de duas formas diferentes.

| Método | Descrição |
|-|-|
| [SDK](speech-sdk.md) | Método mais simples para programadores C/C++, c# e Java * |
| [REST](rest-apis.md) | Reconhecer utterances curtos através de um pedido POST de HTTP | 

\* *O SDK de Java faz parte o [SDK de dispositivos de reconhecimento de voz](speech-devices-sdk.md).*

## <a name="using-the-sdk"></a>Utilizar o SDK

O [SDK de reconhecimento de voz](speech-sdk.md) fornece a forma mais simples utilizar **reconhecimento de voz para texto** na sua aplicação com funcionalidade completa.

1. Crie uma fábrica de reconhecimento de voz, fornecendo uma chave de subscrição do serviço de reconhecimento de voz ou um token de autorização. Também pode configurar opções, como o idioma de reconhecimento ou um ponto de final personalizado para os seus próprios modelos de reconhecimento de voz, neste momento.

2. Obter um reconhecedor a fábrica. Três tipos diferentes de recognizers estão disponíveis. Cada tipo de reconhecedor pode utilizar microfone de predefinido do seu dispositivo, uma sequência de áudio ou no áudio a partir de um ficheiro.

    Reconhecedor | Função
    -|-
    Utilitário de reconhecimento de voz|Fornece transcription de texto de reconhecimento de voz
    Reconhecedor intenção|Derive intenção de orador através de [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) após reconhecimento\*
    Reconhecedor tradução|Converte o texto transcribed para outro idioma (consulte [tradução de reconhecimento de voz](how-to-translate-speech.md))

    \* *Para reconhecimento intenção, terá de utilizar uma chave de subscrição LUIS separada ao criar uma fábrica de reconhecimento de voz para o reconhecedor intenção.*
    
4. Associar a eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama os processadores de eventos quando tem resultados intermédio e finais. Caso contrário, a aplicação irá receber um resultado de final transcription.

5. Inicie o reconhecimento.

### <a name="sdk-samples"></a>Exemplos SDK

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Utilizando a API REST

A API REST é a forma mais simples para reconhecer o reconhecimento de voz se não estiver a utilizar um idioma suportado pelo SDK. Efetuar um pedido POST de HTTP para o ponto final de serviço, transmitir o utterance todo no corpo do pedido. Receberá uma resposta que contém o texto reconhecido.

> [!NOTE]
> Utterances estão limitadas para 15 segundos ou menos ao utilizar a API REST.


Para mais informações sobre o **reconhecimento de voz para texto** API de REST, consulte [REST APIs](rest-apis.md#speech-to-text). Para vê-lo em ação, transfira o [amostras de REST API](https://github.com/Azure-Samples/SpeechToText-REST) a partir do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer voz em c#](quickstart-csharp-windows.md)
