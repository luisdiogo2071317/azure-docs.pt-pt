---
title: O que é o serviço de voz (pré-visualização)?
description: 'O serviço de voz, parte dos serviços cognitivos da Microsoft, une os vários serviços de voz do Azure que estavam disponíveis anteriormente em separado: de voz do Bing (que inclui o reconhecimento de voz e de texto em fala), conversão de voz personalizada e tradução de voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 60ff2f71766a14af17ebb1cb9d20825976471296
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "41987986"
---
# <a name="what-is-the-speech-service-preview"></a>O que é o serviço de voz (pré-visualização)?

Com uma subscrição, o serviço de voz, dá aos programadores uma forma fácil de adicionar funcionalidades poderosas habilitado para fala aos seus aplicativos. As suas aplicações podem agora apresentam os comandos de voz, transcrição, ditado, síntese de fala e a tradução de voz.

O serviço de voz com tecnologia as tecnologias usadas em outros produtos da Microsoft, incluindo o Cortana e o Microsoft Office.

> [!NOTE]
> O serviço de voz está atualmente em pré-visualização pública. Regresse aqui regularmente para atualizações à documentação, exemplos de código adicionais e muito mais.

## <a name="speech-service-features"></a>Funcionalidades do serviço de voz

|Função|Descrição|
|-|-|
|[Conversão de voz em texto](speech-to-text.md)| Converte os fluxos de áudio em texto que seu aplicativo pode aceitar como entrada. Também se integra com o [serviço de compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para derivar a intenção do utilizador de expressões.|
|[Voz](text-to-speech.md)| Converte texto sem formatação para conversão de voz com o som natural, entregue ao seu aplicativo num arquivo de áudio. Várias vozes, variando em sexo ou com/sem acento, estão disponíveis para muitos idiomas com suporte. |
|[Tradução de voz](speech-translation.md)| Pode ser utilizado para traduzir o áudio de transmissão em fluxo em quase tempo real ou para processar a voz gravado. |
|Personalizado de voz em texto|Pode personalizar de voz em texto ao criar a sua própria [adaptações](how-to-customize-acoustic-models.md) e [linguagem](how-to-customize-language-model.md) modelos e ao especificar personalizado [pronúncia](how-to-customize-pronunciation.md) regras. |
|[Voz personalizada](how-to-customize-voice-font.md)|Pode criar seu próprio vozes para voz.|
|[Dispositivos de voz SDK](speech-devices-sdk.md)| Com a introdução do serviço de voz unificado, a Microsoft e respetivos parceiros oferecem uma plataforma de hardware/software integrado otimizada para o desenvolvimento de dispositivos habilitados para conversão de voz |

## <a name="access-to-the-speech-service"></a>Acesso ao serviço de voz

O serviço de voz é disponibilizado em duas formas. [O SDK](speech-sdk.md) abstrai os detalhes dos protocolos de rede para o desenvolvimento mais fácil em plataformas suportadas. O [REST API](rest-apis.md) funciona com qualquer linguagem de programação, mas não oferece todas as funções oferecidas pelo SDK.

|<br>Método|Voz<br>para texto|Texto a<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|Sim|Não|Sim|Bibliotecas de linguagens de programação específicas, utilize procotol com base em Websocket, que simplificam o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma simple baseada em HTTP API que torna mais fácil adicionar voz à sua aplicação.|

## <a name="speech-scenarios"></a>Cenários de voz

Alguns usos comuns da tecnologia de conversão de voz são abordados resumidamente abaixo. O [SDK de voz](speech-sdk.md) é central para a maioria desses cenários.

> [!div class="checklist"]
> * Crie aplicações acionadas por voz
> * Transcrição de gravações de centro de chamada
> * Implementar bots de voz

### <a name="voice-triggered-apps"></a>Aplicações acionadas por voz

Entrada de voz é uma excelente forma de tornar a sua aplicação flexível, automatizada e rápidas de usar. Num aplicativo habilitado para voz, os utilizadores apenas podem fazer para as informações que pretendem, em vez de precisar navegar até ele ao clicar ou tocar.

Se a sua aplicação destina-se a ser utilizado pelo público geral, pode usar o modelo de reconhecimento de voz de linha de base fornecido pelo serviço de voz. Ele faz um bom trabalho ao reconhecer uma ampla variedade de oradores em ambientes típicos.

Se a sua aplicação irá ser utilizada num domínio específico (por exemplo, medicina ou IT), pode criar uma [modelo de idioma](how-to-customize-language-model.md) ensinar o serviço de voz sobre a terminologia especial utilizada pela sua aplicação.

Se seu aplicativo será usado num ambiente desnecessárias, como uma fábrica, pode criar um personalizado [modelo acústico](how-to-customize-acoustic-models.md) para melhor permitir que o serviço de voz distinguir voz de ruído.

Começar a utilizar é tão fácil quanto a transferir os [SDK de voz](speech-sdk.md) e seguir um relevantes [guia de introdução](quickstart-csharp-dotnet-windows.md) artigo.

### <a name="transcribe-call-center-recordings"></a>Transcrição de gravações de centro de chamada

Muitas vezes, chamada center gravações são apenas consultei-se um problema surge com uma chamada. Com o serviço de voz, é fácil de transcrever cada gravação em texto. Assim que eles são texto, permite-lhe facilmente indexá-los para [pesquisa em texto completo](https://docs.microsoft.com/azure/search/search-what-is-azure-search) ou aplicar [análise de texto](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/) para detetar o sentimento, idioma e expressões-chave.

Se sua gravações de centro de chamada geralmente contêm terminologia especializada (por exemplo, nomes de produtos ou gíria, IT), pode criar uma [modelo de idioma](how-to-customize-language-model.md) ensinar o serviço de voz que vocabulário. Um personalizado [modelo acústico](how-to-customize-acoustic-models.md) pode ajuda o serviço de voz compreender as ligações de telefone aquém do ideal.

Para obter mais informações sobre este cenário, leia mais sobre [transcrição do batch](batch-transcription.md) com o serviço de voz.

### <a name="voice-bots"></a>Bots de voz

[Bots](https://dev.botframework.com/) são uma forma cada vez mais popular de ligar os utilizadores com as informações que desejarem e os clientes com o que as empresas que desejam. Adicionar uma interface do usuário conversacionais para seu site ou aplicação torna a sua funcionalidade mais fácil encontrar e mais rápida de aceder. Com o serviço de voz, essa conversa leva numa nova dimensão de fluência, na verdade, a responder às consultas faladas com fala sintetizada.

Para adicionar uma personalidade exclusiva para o seu bot habilitados por voz (e consolide a sua marca), é possível dar uma voz por conta própria. Criar uma voz personalizada é um processo de dois passos. Primeiro, [fazer gravações](record-custom-voice-samples.md) de voz que pretende utilizar. Em seguida, [submeter essas gravações](how-to-customize-voice-font.md) (juntamente com uma transcrição de texto) para o serviço de voz [portal de personalização de voz](https://cris.ai/Home/CustomVoice), que faz o resto. Depois de criar a sua voz personalizada, é simples usá-lo na sua aplicação.

## <a name="next-steps"></a>Passos Seguintes

Obtenha uma chave de subscrição para o serviço de voz.

> [!div class="nextstepaction"]
> [Experimente gratuitamente o serviço de voz](get-started.md)
