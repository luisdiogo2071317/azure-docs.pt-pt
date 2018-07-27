---
title: Cenários de voz de serviços cognitivos do Azure
description: Cenários
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: 3ec5fede8b4e196a2f7573b4c2f202d589a55f32
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282673"
---
# <a name="speech-scenarios"></a>Cenários de Voz

Existem muitos cenários que fique capacitados usando a tecnologia de conversão de voz. Vamos analisar alguns dos mais comuns e destacar os recursos relevantes na documentação. Para a maioria do conteúdo, o [SDK](speech-sdk.md) é fundamental na habilitação estes cenários.

A página aborda como:
> [!div class="checklist"]
> * Crie aplicações acionadas por voz
> * Transcrição de chamadas de áudio do Centro de chamada
> * Bots de voz

## <a name="voice-triggered-apps"></a>Voz acionada por aplicações

Número de utilizadores que pretende ativar a entrada de voz nas suas aplicações. Entrada de voz é uma excelente forma de tornar a sua aplicação flexível, sê-lo a usá-lo mãos gratuitos (por exemplo um carro) ou acelerar a várias tarefas, como pedir as direções as informações de notícias ou meteorológicas. 

### <a name="voice-triggered-apps-with-baseline-models"></a>Voz acionada por aplicações com modelos de linha de base

Se a sua aplicação vai ser utilizado pelo público geral em ambientes onde não é excessivo o barulho de fundo, a maneira mais fácil e rápida de fazer isso simplesmente baixando nosso [SDK de voz](speech-sdk.md) e seguir o relevante [ Exemplos de](quickstart-csharp-dotnet-windows.md). O SDK com tecnologia de sua [chave de subscrição do Azure](https://azure.microsoft.com/try/cognitive-services/) permite aos programadores carregar áudio para modelos de reconhecimento de voz de linha de base de energia Cortana e Skype. Os mdoels são uma funcionalidade de ponta e são utilizados pelos produtos mencionados anteriormente. Pode ficar tudo operacional em minutos.

### <a name="voice-triggered-apps-with-custom-models"></a>Voz acionada por aplicações com modelos personalizados

Se seu aplicativo atende um domínio específico, (Digamos supramolecular, biologia ou necessidades especiais de dietary), em seguida, deve considerar adaptar um [modelo de idioma](how-to-customize-language-model.md). Adaptação de um modelo de idioma irá ensiná-o Decodificador sobre as frases mais comuns e as palavras utilizadas pela sua aplicação. O Decodificador será capaz de transcrever com mais precisão uma voz de entrada com um modelo de idioma personalizado para um determinado domínio, em vez do modelo de linha de base. Da mesma forma se o barulho de fundo em que a aplicação vai ser utilizado é proeminente poderá adaptar um modelo acústico. Explore a documentação para outros casos em que [adaptação de linguagem](how-to-customize-language-model.md) e [adaptação acústica](how-to-customize-acoustic-models.md) fornecer valor e visite nosso [portal de adaptação](https://customspeech.ai) para kick-Starting a experiência de criação de modelo. É semelhante aos modelos de linha de base, modelos personalizados são chamados por meio de nosso [SDK de voz](speech-sdk.md) e seguir o relevante [exemplos](quickstart-csharp-dotnet-windows.md).

## <a name="transcribe-call-center-audio-calls"></a>Transcrição de chamadas de áudio do Centro de chamada

Centrais de chamadas acumular-se em grandes quantidades de áudio. Oculto dentro do valor de permanece esses arquivos de áudio que pode ser obtido, no entanto, transcrição. A duração da chamada, o sentimento, a satisfação dos clientes e o valor geral a chamada fornecido ao chamador, podem ser detetados, obtendo transcrições de chamadas.

O melhor ponto de partida é o [transcrição de API do Batch](batch-transcription.md) juntamente com relacionados [exemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

Terá de obter primeiro uma [chave de subscrição do Azure](https://azure.microsoft.com/try/cognitive-services/) e, em seguida, irá precisar de consultar o [documentação]([Batch transcription API](batch-transcription.md)).

### <a name="transcribe-call-center-audio-calls-with-baseline-models"></a>Transcrição de chamadas de áudio de centro de chamada com modelos de linha de base

A decisão que precisa ser feita é se irá utilizar os modelos internos de linha de base para executar a transcrição, adaptar-se de uma linguagem ou um modelo acústico ou ambos. Utilizar modelos de linha de base, a API requer apenas a chave de API. Internamente, a API irá invocar o melhor modelo para os seus dados e adaptar-se.

### <a name="transcribe-call-center-audio-calls-with-custom-models"></a>Transcrição de chamadas de áudio de centro de chamada com modelos personalizados

Se planeia utilizar um modelo personalizado, em seguida, terá do ID desse modelo, juntamente com a chave de API. O ID de modelo é obtido a partir da [portal de adaptação](https://customspeech.ai). Não é o ID de ponto final que encontrar na vista de detalhes do ponto final, mas o ID de modelo que pode ser obtida quando clica em "Detalhes" desse modelo.

## <a name="voice-bots"></a>Bots de voz

Desenvolvedor pode capacitar seus aplicativos com a saída de voz. O serviço de voz pode synthetize voz para diversas [idiomas](supported-languages.md) e fornece a [pontos de extremidade](rest-apis.md) para acessar e adicionar essa capacidade à sua aplicação.

Além disso, para os utilizadores que pretendem adicionar mais personalidade e exclusividade aos seus bots, o serviço de voz permite aos programadores personalizar um tipo de voz exclusivo. Semelhante à personalização de reconhecimento de fala, tipos de voz de modelos exigem que os dados de utilizador. Os desenvolvedores são carregar esses dados em nossos [portal de adaptação de voz](https://customspeech.ai) e comece a criar a sua única da marca de voz para o bot. Detalhes são descritos [aqui](how-to-text-to-speech.md) , bem como a [FAQ](faq-text-to-speech.md) páginas 

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Começar com o SDK de voz](speech-sdk.md)
