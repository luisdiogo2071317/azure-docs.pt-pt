---
title: Descrição geral em direto de transmissão em fluxo utilizando os Media Services do Azure | Microsoft Docs
description: Este oferece tópico em direto de uma descrição geral da transmissão em fluxo utilizando v3 de Media Services do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: b8c9375d8ad915200cbc8b2e1a62979fd1b7d179
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238305"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Em direto transmissão em fluxo com Media Services do Azure v3

Quando distribui os eventos de transmissão em fluxo em direto com Media Services do Azure os seguintes componentes estão normalmente relacionados:

* Uma câmara, que é utilizada para difundir um evento.
* Um codificador vídeo em direto que converte sinais da câmara (ou outro dispositivo, como o computador portátil) para fluxos que são enviados para os serviços de suporte de dados em direto de serviço de transmissão em fluxo. Podem também incluir os sinais SCTE 35 e Ad ajudas de publicidade. 
* O serviço serviços de suporte de dados de transmissão em fluxo em direto permite-lhe de inserção, pré-visualizar, do pacote, registar, encriptar e difusão o conteúdo para os seus clientes ou para um CDN de distribuição adicional.

Este artigo fornece uma descrição geral detalhada e inclui diagramas dos principais componentes envolvidos na transmissão em fluxo em direto com Media Services.

## <a name="overview-of-main-components"></a>Descrição geral dos componentes principais

Nos Media Services, [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis por processar o conteúdo de transmissão em fluxo em direto. Um LiveEvent fornece um ponto final de entrada (URL de inserção) que, em seguida, forneça a um codificador em direto no local. O LiveEvent recebe fluxos em direto de entrada do codificador em direto no formato RTMP ou transmissão em fluxo uniforme e torna a mesma disponível para transmissão em fluxo através de um ou mais [lidam](https://docs.microsoft.com/rest/api/media/streamingendpoints). A [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) permite-lhe controlar a publicação, gravação e definições de janela DVR da transmissão em fluxo em direto. O LiveEvent também fornece um ponto final de pré-visualização (URL de pré-visualização) que utilizar para pré-visualizar e validar a sua transmissão em fluxo antes do processamento adicional e entrega. 

Os Media Services fornecem **empacotamento dinâmico**, que lhe permite pré-visualizar e conteúdo do MPEG DASH, HLS, transmissão em fluxo uniforme de difusão formatos de transmissão em fluxo sem ter de manualmente Empacote novamente esta para estes formatos de transmissão em fluxo. Pode reproduzir novamente com qualquer HLS, TRAÇO ou uniforme jogadores compatíveis. Também pode utilizar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) para testar a sua transmissão em fluxo.

Os Media Services permitem entregar o conteúdo encriptado dinamicamente (**encriptação dinâmica**) com avançadas encriptação Standard (AES-128) ou qualquer um dos três principais direitos digitais (DRM) de gestão sistemas: Microsoft PlayReady, Widevine da Google e FairPlay da Apple. Os Media Services também fornecem um serviço para entrega de chaves AES e DRM licenças (PlayReady, Widevine e do FairPlay) para clientes autorizados.

Se assim o desejar, também pode aplicar **filtragem dinâmica**, que podem ser utilizadas para controlar o número de faixas, formatos, de forma, que é enviadas para os jogadores. Os Media Services também suporta a inserção do ad.


## <a name="liveevent-types"></a>Tipos de LiveEvent

A [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) pode ser um dos dois tipos: codificação em direto e pass-through. 

### <a name="live-encoding-with-media-services"></a>Live encoding com Media Services

![Live encoding](./media/live-streaming/live-encoding.png)

Um codificador em direto no local envia um fluxo de velocidade de transmissão única para LiveEvent que está ativado para realizar live encoding com Media Services dos seguintes protocolos: RTMP ou transmissão em fluxo uniforme (MP4 fragmentados). O LiveEvent executa codificação em direto de entrada de transmissão única para um fluxo de vídeo de transmissão múltipla (adaptável). Quando solicitado, os Media Services disponibilizam a transmissão em fluxo para os clientes.

Ao criar este tipo de LiveEvent, especifique **básico** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Pass-through

![Pass-through](./media/live-streaming/pass-through.png)

Pass-through está otimizado para fluxos em direto de execução longa ou 24x7 linear em direto codificação utilizando um codificador em direto no local. O codificador no local envia múltipla **RTMP** ou **transmissão em fluxo uniforme** (MP4 fragmentado) para o LiveEvent que está configurado para **pass-through** entrega. O **pass-through** entrega é quando as transmissões em fluxo pass-through **LiveEvent**s sem qualquer processamento adicional. 

Pass-through LiveEvents pode suportar cópias de segurança para a resolução de 4K e HEVC passar quando utilizado com transmissão em fluxo uniforme protocolo de inserção. 

Ao criar este tipo de LiveEvent, especifique **nenhum** (LiveEventEncodingType.None).

> [!NOTE]
> A utilização de um método pass-through é a forma mais económica de realizar uma transmissão em fluxo em direto quando estiver a realizar vários eventos durante um longo período de tempo e já investiu em codificadores no local. Consulte os detalhes dos [preços](https://azure.microsoft.com/pricing/details/media-services/).
> 

## <a name="liveevent-types-comparison"></a>Comparação de tipos de LiveEvent 

A tabela seguinte compara as funcionalidades dos dois tipos de LiveEvent.

| Funcionalidade | LiveEvent pass-through | LiveEvent básico |
| --- | --- | --- |
| Entrada de velocidade de transmissão única é codificada em múltiplos de forma na nuvem |Não |Sim |
| Resolução máxima, número de camadas |4Kp30  |720p, 6 camadas, 30 fps |
| Protocolos de entrada |RTMP, transmissão em fluxo de uniforme |RTMP, transmissão em fluxo de uniforme |
| Preço |Consulte o [página de preços](https://azure.microsoft.com/pricing/details/media-services/) e clique no separador "Vídeo em direto" |Consulte o [página de preços](https://azure.microsoft.com/pricing/details/media-services/) |
| Tempo de execução máximo |24x7 |24x7 |
| Suporte para inserir slates |Não |Sim |
| Suporte para ad sinalização através de API|Não |Sim |
| Suporte para ad sinalização através de SCTE35 inband|Sim |Sim |
| Legendas de CEA 608/708 pass-through |Sim |Sim |
| Capacidade de recuperar breves stalls no feed de contribuição |Sim |Não (LiveEvent começará slating após 6 + segundos w/o dados de entrada)|
| Suporte para GOPs de entrada não uniforme |Sim |Não – entrada têm de ser corrigida seg 2 GOPs |
| Suporte para a entrada de taxa de moldura variável |Sim |Não – entrada têm de ser corrigida velocidade de fotogramas.<br/>Variações secundárias são tolerated, por exemplo, durante em segundo plano do movimento elevada. Mas o codificador não é possível remover a 10 fotogramas por segundo. |
| Auto-shutoff de LiveEvent quando entrada feed está perdido |Não |Após 12 horas, se não houver nenhuma LiveOutput em execução |

## <a name="liveevent-states"></a>Estados de LiveEvent 

O estado atual de um LiveEvent. Os valores possíveis incluem:

|Estado|Descrição|
|---|---|
|**Parado**| Este é o estado inicial o LiveEvent após a respetiva criação (a menos que foi selecionado autostart especificado.) Não existem faturação ocorre neste estado. Neste estado, as propriedades de LiveEvent podem ser atualizadas, mas não é permitida a transmissão em fluxo.|
|**A iniciar**| O LiveEvent está a ser iniciada. Não existem faturação ocorre neste estado. Não são permitidas transmissões em fluxo nem atualizações durante este estado. Se ocorrer um erro, devolve a LiveEvent para o estado parado.|
|**Em execução**| O LiveEvent é capaz de processar fluxos em direto. Agora, está faturação utilização. Tem de parar o LiveEvent para impedir mais de faturação.|
|**A parar**| O LiveEvent está a ser parado. Não existem faturação ocorre neste estado transitório. Não são permitidas transmissões em fluxo nem atualizações durante este estado.|
|**A eliminar**| O LiveEvent está a ser eliminada. Não existem faturação ocorre neste estado transitório. Não são permitidas transmissões em fluxo nem atualizações durante este estado.|

## <a name="liveoutput"></a>LiveOutput

A [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) permite-lhe controlar a publicação, gravação e definições de janela DVR da transmissão em fluxo em direto. A relação LiveEvent e LiveOutput é semelhante à multimédia tradicional onde um canal (LiveEvent) tem uma transmissão em fluxo constante de conteúdo e um programa (LiveOutput) está confinado a alguns eventos temporizados nesse LiveEvent.
Pode especificar o número de horas que pretenda manter o conteúdo gravado para o LiveOutput definindo a **ArchiveWindowLength** propriedade. **ArchiveWindowLength** é uma duração ISO 8601 timespan da duração da janela de arquivo (Digital gravador de vídeo ou DVR). Este valor pode ser definido a partir de um mínimo de 5 minutos até um máximo de 25 horas. 

**ArchiveWindowLength** dita também o número máximo de clientes de hora pode recuar a partir da posição atual em direto. LiveOutputs podem ser executados durante o período de tempo especificado, mas o conteúdo que se situe atrás da duração da janela é continuamente descartado. O valor desta propriedade também determina quanto tempo os manifestos dos clientes podem aumentar.

Cada LiveOutput está associado um [Asset](https://docs.microsoft.com/rest/api/media/assets)e regista os dados para um contentor no armazenamento do Azure anexado à conta de Media Services. Para publicar o LiveOutput, tem de criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) para o elemento associado. Ter este localizador irá permitir compilar um URL de transmissão em fluxo que pode fornecer aos seus clientes.

Um LiveEvent suporta até três LiveOutputs em execução em simultâneo para que possa criar vários arquivos da mesma transmissão em fluxo recebida. Isto permite publicar e arquivar diferentes partes de um evento, conforme necessário. Por exemplo, é o requisito de negócio para difundir um feed de linear em direto 24x7, mas que pretende criar "gravações" de programas ao longo do dia para oferecer aos clientes como o conteúdo a pedido para visualização catch-up.  Para este cenário, primeiro crie um LiveOutput principal, com uma janela de arquivo curto de 1 hora ou menos dos clientes para otimizar para como fluxo em direto primário. Deverá criar um StreamingLocator para este LiveOutput e publicá-lo à sua aplicação ou o web site como o feed "Em direto".  Como o feed está em execução, através de programação pode criar uma segunda LiveOutput simultânea no início de um Mostrar (ou 5 minutos numa fase inicial para fornecer alguns identificadores cortar mais tarde.) Este segundo LiveOutput pode ser parado 5 minutos após terminar o programa ou o evento e, em seguida, pode criar um novo StreamingLocator para publicar este programa como um recurso a pedido no catálogo da sua aplicação.  Pode repetir várias vezes este processo para limites de outro programa ou realça que pretende partilhar como a pedido imediatamente, todos os enquanto o "em direto" feed do primeiro LiveOutput continua a difusão o feed linear.  Além disso, pode tirar partido do suporte de filtro dinâmica para limitar a cauda do arquivo de LiveOutput que introduziu para "segurança de sobreposição" entre programas e head e alcançar um início e de fim do conteúdo mais exata. Conteúdo arquivado, também pode ser submetido para um [transformar](https://docs.microsoft.com/rest/api/media/transforms) para codificação ou subclipping exata de moldura para vários formatos de saída para ser utilizado como sindicação a outros serviços.

## <a name="streamingendpoint"></a>StreamingEndpoint

Assim que tiver o fluxo que fluem para o LiveEvent, pode começar o evento de transmissão em fluxo através da criação de um recurso, LiveOutput e StreamingLocator. Isto irá arquivar a transmissão e disponibilizá-lo para os espetadores através de [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Quando a conta de Media Services é criada um ponto final de transmissão em fluxo de predefinido é adicionado à sua conta no estado parado. Para iniciar o conteúdo de transmissão em fluxo e tirar partido do empacotamento dinâmico e a encriptação dinâmica, o ponto final de transmissão em fluxo partir do qual pretende transmitir o conteúdo tem de estar no Estado em execução.

## <a name="billing"></a>Faturação

Um LiveEvent começa a faturação, assim como o estado passa para "Em execução". Para parar o LiveEvent de faturação, terá de parar o LiveEvent.

> [!NOTE]
> Quando **LiveEventEncodingType** no seu [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) está definido para básico, a serviços de suporte de dados será automaticamente shutoff de qualquer LiveEvent que ainda está no estado "Em execução" 12 horas após o feed de entrada não se tenha perdido e existem nenhum LiveOutputs execução. No entanto, pode ainda serão cobrados durante o período de tempo que a LiveEvent estava no estado "Em execução".
>

A tabela seguinte mostra como os Estados de LiveEvent mapeiam para o modo de faturação.

| Estado de LiveEvent | É faturação? |
| --- | --- |
| A iniciar |Não (estado transitório) |
| A executar |SIM |
| A parar |Não (estado transitório) |
| Parada |Não |

## <a name="next-steps"></a>Passos Seguintes

[Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)
