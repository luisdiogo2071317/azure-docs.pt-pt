---
title: O que é o serviço de Voz?
description: 'O serviço de Voz, parte dos Serviços Cognitivos da Microsoft, unifica vários serviços de voz do Azure que anteriormente estavam disponíveis em separado: a Voz do Bing (que inclui o reconhecimento de voz e a conversão de texto em voz), a Voz Personalizada e a Tradução de Voz.'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 09/24/2018
ms.author: erhopf
ms.openlocfilehash: 7a61d85d96b993d49a536c38c2907a3d6ef55e65
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365463"
---
# <a name="what-is-the-speech-service"></a>O que é o serviço de Voz?


Tal como os outros serviços de voz do Azure, o serviço de Voz funciona com as tecnologias de voz utilizadas em produtos como a Cortana e o Microsoft Office.

O serviço de Voz unifica as funcionalidades de voz do Azure anteriormente disponíveis através dos serviços [API de Voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Tradução de Voz](https://docs.microsoft.com/azure/cognitive-services/translator-speech/) e [Voz Personalizada](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home). Agora, uma subscrição dá acesso a todas estas capacidades.

## <a name="main-speech-service-functions"></a>Principais funções do serviço de Voz

As funções principais do serviço de Voz são a Conversão de Voz em Texto (também denominada reconhecimento de voz ou transcrição), Conversão de Texto em Voz (sintetização de voz) e Tradução de Voz.

|Função|Funcionalidades|
|-|-|
|[Conversão de Voz em Texto](speech-to-text.md)| <li>Transcreve voz em tempo real contínua para texto.<li>Pode fazer a transcrição em série de voz de gravações de áudio. <li>Suporta resultados intermédios, deteção de fim de voz, formatação automática de texto e máscara de linguagem inapropriada. <li>Pode recorrer a [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para calcular a intenção do utilizador a partir da voz transcrita.\*|
|[Conversão de Texto em Voz](text-to-speech.md)| <li>Converte texto em voz com som natural. <li>Oferece vários géneros e/ou dialetos para muitos idiomas suportados. <li>Suporta a introdução de texto simples ou Speech Synthesis Markup Language (SSML). |
|[Tradução de Voz](speech-translation.md)| <li>Traduz áudio de transmissão em fluxo quase em tempo real.<li> Também pode processar voz gravada.<li>Fornece resultados como texto ou voz sintetizada. |


## <a name="customize-speech-features"></a>Personalizar funcionalidades de voz

Pode utilizar os seus dados para preparar os modelos subjacentes às funcionalidades de Conversão de Voz em Texto e Conversão de Texto em Voz do serviço de Voz.

|Funcionalidade|Modelo|Objetivo|
|-|-|-|
|Conversão de Voz em Texto|[Modelo acústico](how-to-customize-acoustic-models.md)|Ajuda a transcrever oradores e ambientes específicos, como carros ou fábricas.|
||[Modelo de linguagem](how-to-customize-language-model.md)|Ajuda a transcrever vocabulário e gramática específicos de uma área, como gíria médica ou informática.|
||[Modelo de pronúncia](how-to-customize-pronunciation.md)|Ajuda a transcrever abreviaturas e acrónimos, por exemplo, "IOU" para "i owe you". |
|Conversão de Texto em Voz|[Tipo de voz](how-to-customize-voice-font.md)|Dá à sua aplicação uma voz própria ao preparar o modelo em amostras de voz humana.|

Pode utilizar os seus modelos personalizados em qualquer lugar onde utilizaria os modelos padrão na funcionalidade de Conversão de Voz em Texto ou Conversão de Texto em Voz da sua aplicação.

## <a name="use-the-speech-service"></a>Utilizar o serviço de Voz

Para simplificar o desenvolvimento de aplicações preparadas para voz, a Microsoft fornece o [SDK de Voz](speech-sdk.md) para utilização com o serviço de Voz. O SDK de Voz fornece APIs nativas consistentes de Conversão de Voz em Texto e Tradução de Voz para C#, C++ e Java. Se estiver a desenvolver com uma destas linguagens, o SDK de Voz torna o desenvolvimento mais fácil ao tratar dos detalhes de rede por si.

O serviço de Voz também tem uma [API REST](rest-apis.md) que funciona com qualquer linguagem de programação que possa fazer pedidos HTTP. A interface REST não oferece a funcionalidade de transmissão em fluxo e em tempo real do SDK.

|<br>Método|Voz<br>em Texto|Texto em<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDK de Voz](speech-sdk.md)|Sim|Não|Sim|APIs nativas para C#, C++ e Java para simplificar o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma API simples baseada em HTTP que facilita a adição de voz às suas aplicações.|

### <a name="websockets"></a>WebSockets

O serviço de Voz também tem protocolos WebSocket para transmissão em fluxo de Conversão de Voz em Texto e Tradução de Voz. Os SDKs de Voz utilizam estes protocolos para comunicar com o serviço de Voz. Utilize o SDK de Voz em vez de tentar implementar a sua própria comunicação de WebSocket com o serviço de Voz.

Se já tiver código que utiliza a Voz do Bing ou a Tradução de Voz através de WebSockets, pode atualizá-lo para utilizar o serviço de Voz. Os protocolos WebSocket são compatíveis, apenas os pontos finais são diferentes.

### <a name="speech-devices-sdk"></a>SDK de Dispositivos de Voz

O [SDK de Dispositivos de Voz](speech-devices-sdk.md) é uma plataforma integrada de hardware e software para programadores de dispositivos preparados para voz. O nosso parceiro de hardware fornece designs de referência e unidades de desenvolvimento. A Microsoft fornece um SDK otimizado para dispositivos que tira total partido das capacidades do hardware.


## <a name="speech-scenarios"></a>Cenários de voz

Os casos de utilização para o serviço de Voz incluem:

> [!div class="checklist"]
> * Criar aplicações acionadas por voz
> * Transcrever gravações de centros de atendimento telefónico
> * Implementar bots de voz

### <a name="voice-user-interface"></a>Interface de utilizador de Voz

A entrada de voz é uma excelente forma de tornar a sua aplicação flexível, automatizada e rápida de utilizar. Com uma aplicação preparada para voz, os utilizadores podem simplesmente perguntar pelas informações que pretendem.

Se a sua aplicação se destina a ser utilizada pelo público em geral, pode utilizar os modelos de reconhecimento de voz predefinidos. Reconhecem uma grande variedade de oradores em ambientes comuns.

Se a aplicação for utilizada num domínio específico, por exemplo, medicina ou TI, pode criar um [modelo de idioma](how-to-customize-language-model.md). Pode utilizar este modelo para ensinar ao serviço de Voz a terminologia especial utilizada pela sua aplicação.

Se a aplicação for utilizada num ambiente ruidoso, como uma fábrica, pode criar um [modelo acústico](how-to-customize-acoustic-models.md) personalizado. Este modelo ajuda o serviço de Voz a distinguir voz de ruído.

### <a name="call-center-transcription"></a>Transcrição de centros de atendimento telefónico

Muitas vezes, as gravações dos centros de atendimento telefónico são consultadas apenas se surgir um problema numa chamada. Com o serviço de Voz, é fácil transcrever todas as gravações para texto. Pode facilmente indexar o texto para [pesquisa em texto completo](https://docs.microsoft.com/azure/search/search-what-is-azure-search) ou aplicar a [Análise de Texto](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) para detetar o sentimento, o idioma e expressões-chave.

Se as suas gravações de centro de atendimento telefónico envolverem terminologia especializada (por exemplo, nomes de produtos ou gíria informática), pode criar um [modelo de linguagem](how-to-customize-language-model.md) e ensinar ao serviço de Voz esse vocabulário. Um [modelo acústico](how-to-customize-acoustic-models.md) personalizado pode ajudar o serviço de Voz a compreender as ligações telefónicas de fraca qualidade.

Para obter mais informações sobre este cenário, leia mais sobre a [transcrição em série](batch-transcription.md) com o serviço de Voz.

### <a name="voice-bots"></a>Bots de voz

Os [bots](https://dev.botframework.com/) são uma forma popular de ligar os utilizadores às informações que pretendem e os clientes às empresas favoritas. Ao adicionar uma interface de utilizador de conversação ao seu site ou aplicação facilita a localização da suas funcionalidades e torna o acesso às mesmas mais rápido. Com o serviço de Voz, esta conversação assume uma nova dimensão em termos de fluência ao responder a consultas faladas.

Para adicionar uma personalidade única ao seu bot preparado para voz, pode dar-lhe uma voz própria. A criação de uma voz personalizada é um processo de dois passos. Em primeiro lugar, [faça gravações](record-custom-voice-samples.md) da voz que pretende utilizar. Em seguida, [submeta essas gravações](how-to-customize-voice-font.md) (juntamente com uma transcrição de texto) para o [portal de personalização de voz](https://cris.ai/Home/CustomVoice) do serviço de Voz, que fará o resto. Depois de criar a sua voz personalizada, os passos para utilizá-la na sua aplicação são simples.

## <a name="next-steps"></a>Passos seguintes

Obtenha uma chave de subscrição para o serviço de Voz.

> [!div class="nextstepaction"]
> [Experimente o serviço de Voz gratuitamente](get-started.md)
