---
title: O que é o serviço de Voz (pré-visualização)?
description: 'O serviço de Voz, parte dos Serviços Cognitivos da Microsoft, unifica vários serviços de voz do Azure que anteriormente estavam disponíveis em separado: a Voz do Bing (que inclui o reconhecimento de voz e a conversão de texto em voz), a Voz Personalizada e a Tradução de Voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 922320bb0b880e933b27025257e6a533fe257680
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091477"
---
# <a name="what-is-the-speech-service"></a>O que é o serviço de Voz?

O serviço de Voz unifica as funcionalidades de voz do Azure anteriormente disponíveis através dos serviços [API de Voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home), [Tradução de Voz](https://docs.microsoft.com/azure/cognitive-services/translator-speech/) e [Voz Personalizada](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home). Agora, uma subscrição dá acesso a todas estas capacidades.

Tal como os outros serviços de voz do Azure, o serviço de Voz funciona com as tecnologias de voz comprovadas utilizadas em produtos como a Cortana e o Microsoft Office. Pode contar com a qualidade dos resultados e a fiabilidade da cloud do Azure.

> [!NOTE]
> O serviço de Voz está atualmente em pré-visualização pública. Volte aqui regularmente para ver atualizações da documentação, novos exemplos de código e muito mais.

## <a name="main-speech-service-functions"></a>Principais funções do serviço de Voz

As funções principais do serviço de Voz são a Conversão de Voz em Texto (também denominada reconhecimento de voz ou transcrição), Conversão de Texto em Voz (sintetização de voz) e Tradução de Voz.

|Função|Funcionalidades|
|-|-|
|[Conversão de Voz em Texto](speech-to-text.md)| <ul><li>Transcreve voz em tempo real contínua para texto.<li>Pode fazer a transcrição em série de voz de gravações de áudio. <li>Oferece modos de reconhecimento para casos de utilização interativa, de conversação e de ditado.<li>Suporta resultados intermédios, deteção de fim de voz, formatação automática de texto e máscara de linguagem inapropriada. <li>Pode recorrer a [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para calcular a intenção do utilizador a partir da voz transcrita.\*|
|[Conversão de Texto em Voz](text-to-speech.md)| <ul><li>Converte texto em voz com som natural. <li>Oferece vários géneros e/ou dialetos para muitos idiomas suportados. <li>Suporta a introdução de texto simples ou Speech Synthesis Markup Language (SSML). |
|[Tradução de Voz](speech-translation.md)| <ul><li>Traduz áudio de transmissão em fluxo quase em tempo real<li> Também pode processar voz gravada<li>Fornece resultados como texto ou voz sintetizada. |

\* *O reconhecimento da intenção exige uma subscrição do LUIS.*


## <a name="customizing-speech-features"></a>Personalizar funcionalidades de voz

O serviço de Voz permite-lhe utilizar os seus dados para preparar os modelos subjacentes às funcionalidades de Conversão de Voz em Texto e Conversão de Texto em Voz do serviço de Voz. 

|Funcionalidade|Modelo|Objetivo|
|-|-|-|
|Conversão de Voz em Texto|[Modelo acústico](how-to-customize-acoustic-models.md)|Ajuda a transcrever oradores e ambientes específicos, como carros ou fábricas|
||[Modelo de linguagem](how-to-customize-language-model.md)|Ajuda a transcrever vocabulário e gramática específicos de uma área, como gíria médica ou informática|
||[Modelo de pronúncia](how-to-customize-pronunciation.md)|Ajuda a transcrever abreviaturas e acrónimos, por exemplo, "IOU" para "i oh you" |
|Conversão de Texto em Voz|[Tipo de voz](how-to-customize-voice-font.md)|Dá à sua aplicação uma voz própria ao preparar o modelo em amostras de voz humana.|

Depois de criado, os seus modelos personalizados podem ser utilizados em qualquer lugar onde utilizaria os modelos padrão na funcionalidade de Conversão de Voz em Texto ou Conversão de Texto em Voz da sua aplicação.


## <a name="using-the-speech-service"></a>Utilizar o serviço de Voz

Para simplificar o desenvolvimento de aplicações preparadas para voz, a Microsoft fornece o [SDK de Voz](speech-sdk.md) para utilização com o novo serviço de Voz. O SDK de Voz fornece APIs nativas consistentes de Conversão de Voz em Texto e Tradução de Voz para C#, C++ e Java. Se estiver a desenvolver com uma destas linguagens, o SDK de Voz torna o desenvolvimento mais fácil tratar dos detalhes de rede por si.

O serviço de Voz também tem uma [API REST](rest-apis.md) que funciona com qualquer linguagem de programação que possa fazer pedidos HTTP. No entanto, a interface REST não oferece a funcionalidade de transmissão em fluxo e em tempo real do SDK.

|<br>Método|Voz<br>em Texto|Texto em<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDK de Voz](speech-sdk.md)|Sim|Não|Sim|APIs nativas para C#, C++ e Java para simplificar o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma API simples baseada em HTTP que facilita a adição de voz às suas aplicações.|

### <a name="websockets"></a>WebSockets

O serviço de Voz também tem protocolos WebSockets para transmissão em fluxo de Conversão de Voz em Texto e Tradução de Voz. Os SDKs de Voz utilizam estes protocolos para comunicar com o serviço de Voz. Deve utilizar o SDK de Voz em vez de tentar implementar a sua própria comunicação de WebSockets com o serviço de Voz.

No entanto, se já tiver código que utiliza a Voz do Bing ou a Tradução de Voz através de WebSockets, é muito simples atualizá-lo para utilizar o serviço de Voz. Os protocolos WebSockets são compatíveis; apenas os pontos finais são diferentes.

### <a name="speech-devices-sdk"></a>SDK de Dispositivos de Voz

O [SDK de Dispositivos de Voz](speech-devices-sdk.md) é uma plataforma integrada de hardware e software para programadores de dispositivos preparados para voz. O nosso parceiro de hardware fornece designs de referência e unidades de desenvolvimento. A Microsoft fornece um SDK otimizado para dispositivos que tira total partido das capacidades do hardware.


## <a name="speech-scenarios"></a>Cenários de voz

Os casos de utilização para o serviço de Voz incluem:

> [!div class="checklist"]
> * Criar aplicações acionadas por voz
> * Transcrever gravações de centros de atendimento telefónico
> * Implementar bots de voz

### <a name="voice-user-interface"></a>Interface de utilizador de Voz

A entrada de voz é uma excelente forma de tornar a sua aplicação flexível, automatizada e rápida de utilizar. Numa aplicação preparada para voz, os utilizadores podem simplesmente perguntar pelas informações que pretendem em vez de terem de navegar até elas.

Se a sua aplicação se destina a ser utilizada pelo público em geral, pode utilizar os modelos de reconhecimento de voz predefinidos. Reconhecem bem uma grande variedade de oradores em ambientes comuns.

Se a sua aplicação se destina a ser utilizada num domínio específico (por exemplo, medicina ou TI), pode criar um [modelo de linguagem](how-to-customize-language-model.md) para ensinar ao serviço de Voz a terminologia especial utilizada pela sua aplicação.

Se a sua aplicação se destina a ser utilizada num ambiente ruidoso, como uma fábrica, pode criar um [modelo acústico](how-to-customize-acoustic-models.md) personalizado para permitir que o serviço de Voz faça melhor a distinção entre voz e ruído.

Para começar, basta transferir o [SDK de Voz](speech-sdk.md) e seguir um artigo de [Início Rápido](quickstart-csharp-dotnet-windows.md) relevante.

### <a name="call-center-transcription"></a>Transcrição de centros de atendimento telefónico

Muitas vezes, as gravações dos centros de atendimento telefónico são consultadas apenas de surgir um problema numa chamada. Com o serviço de Voz, é fácil transcrever todas as gravações para texto. Quando já estão em forma de texto, pode facilmente indexá-las para [pesquisa em texto completo](https://docs.microsoft.com/azure/search/search-what-is-azure-search) ou aplicar a [Análise de Texto](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) para detetar o sentimento, o idioma e expressões-chave.

Se as suas gravações de centro de atendimento telefónico girarem em torno de terminologia especializada (por exemplo, nomes de produtos ou gíria informática), pode criar um [modelo de linguagem](how-to-customize-language-model.md) e ensinar ao serviço de Voz esse vocabulário. Um [modelo acústico](how-to-customize-acoustic-models.md) personalizado pode ajudar o serviço de Voz a compreender as ligações telefónicas de fraca qualidade.

Para obter mais informações sobre este cenário, leia mais sobre a [transcrição em série](batch-transcription.md) com o serviço de Voz.

### <a name="voice-bots"></a>Bots de voz

Os [bots](https://dev.botframework.com/) são uma forma cada vez mais popular de ligar os utilizadores às informações que pretendem e os clientes às empresas que adoram. Adicionar uma interface de utilizador de conversação ao seu site ou aplicação facilita a localização da suas funcionalidades e torna o acesso às mesmas mais rápido. Com o serviço de Voz, esta conversação assume uma nova dimensão em termos de fluência ao responder a consultas faladas.

Para adicionar uma personalidade única ao seu bot preparado para voz (e fortalecer a sua marca), pode dar-lhe uma voz própria. A criação de uma voz personalizada é um processo de dois passos. Em primeiro lugar, [faça gravações](record-custom-voice-samples.md) da voz que pretende utilizar. Em seguida, [submeta essas gravações](how-to-customize-voice-font.md) (juntamente com uma transcrição de texto) para o [portal de personalização de voz](https://cris.ai/Home/CustomVoice) do serviço de Voz, que fará o resto. Depois de criar a sua voz personalizada, é muito simples utilizá-la na sua aplicação.

## <a name="next-steps"></a>Passos seguintes

Obtenha uma chave de subscrição para o serviço de Voz.

> [!div class="nextstepaction"]
> [Experimente o serviço de Voz gratuitamente](get-started.md)
