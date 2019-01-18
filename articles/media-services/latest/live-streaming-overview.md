---
title: Descrição geral da transmissão em direto através dos serviços de multimédia do Azure | Documentos da Microsoft
description: Isso permite o artigo uma visão geral do live a transmissão em fluxo através dos serviços de multimédia do Azure v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: 6f7c6c2265fe13eb50aa900e9a51e11edfd90201
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382085"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Transmissão em direto com Media Services do Azure v3

Serviços de multimédia do Azure permite-lhe fornecer eventos em direto aos seus clientes na cloud do Azure. Para transmitir os seus eventos em direto com Media Services, precisa do seguinte:  

- Uma câmera que é utilizada para capturar o evento em direto.
- Um codificador vídeo em direto que converte sinais da câmara (ou outro dispositivo, como um laptop) numa contribuição feed, que é enviada para os serviços de multimédia. O feed de contribuição pode incluir sinais relacionadas com a publicidade, como marcadores SCTE 35.
- Componentes nos serviços de multimédia, permitem-lhe ingerir, pré-visualização, do pacote, gravam, encriptar e transmitir o evento em direto para os seus clientes ou para uma CDN para uma maior distribuição.

Este artigo fornece uma visão geral detalhada, orientação e inclui diagramas dos componentes principais envolvidos na transmissão em fluxo em direto com os serviços de multimédia.

## <a name="live-streaming-workflow"></a>Fluxo de trabalho de transmissão em fluxo em direto

Eis os passos para um fluxo de trabalho de transmissão em fluxo em direto:

1. Criar uma **evento em direto**.
2. Criar uma nova **Asset** objeto.
3. Criar uma **Live saída** e utilize o nome de recurso que criou.
4. Criar uma **política de transmissão em fluxo** e **chave de conteúdo** se pretende encriptar o seu conteúdo com o DRM.
5. Se não utilizar DRM, crie uma **localizador de transmissão em fluxo** com o incorporado **política de transmissão em fluxo** tipos.
6. Listar os caminhos na **política de transmissão em fluxo** para recuperar os URLs para utilizar (esses são determinísticos).
7. Obter o nome do anfitrião para o **ponto final de transmissão em fluxo** que deseja transmitir em fluxo de (Certifique-se de que o ponto final de transmissão em fluxo está em execução). 
8. Combine o URL no passo 6 com o nome de anfitrião no passo 7 para obter o URL completo.
9. Se pretender parar a tornar sua **evento em direto** podem ser visualizados, terá de parar a transmissão em fluxo do evento, eliminando a **localizador de transmissão em fluxo**.

Para obter mais informações, consulte um [tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md) que se baseia a [Live .NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live) exemplo.

## <a name="overview-of-main-components"></a>Descrição geral dos componentes principais

Para entregar transmissões em fluxo a pedido ou em direto com Media Services, tem de ter, pelo menos, um [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints). Quando a sua conta de serviços de multimédia é criada um **predefinição** StreamingEndpoint é adicionado à sua conta no **parado** estado. Tem de iniciar o StreamingEndpoint a partir do qual pretende transmitir o seu conteúdo para os seus utilizadores. Pode utilizar a predefinição **StreamingEndpoint**, ou criar outro personalizados **StreamingEndpoint** com a sua configuração desejada e definições da CDN. Pode optar por ativar pontos finais de vários, cada uma segmentação de uma CDN diferente e fornecer um nome de anfitrião exclusivo para a entrega de conteúdos. 

Nos serviços de multimédia [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis por ingerir e processar os feeds de vídeo em direto. Quando cria um LiveEvent, um ponto de final de entrada é criado a que pode utilizar para enviar um sinal ao vivo a partir de um codificador remoto. O codificador em direto remoto envia a contribuição do feed que o ponto final através de entrada a [RTMP](https://www.adobe.com/devnet/rtmp.html) ou [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) protocol (MP4 fragmentado). Para a transmissão em fluxo uniforme de protocolo de ingestão, os esquemas de URL suportados são `http://` ou `https://`. Para o RTMP de protocolo de ingestão, os esquemas de URL suportados são `rtmp://` ou `rtmps://`. Para obter mais informações, consulte [recomendado codificadores de transmissão em direto](recommended-on-premises-live-encoders.md).<br/>
Ao criar um **LiveEvent**, pode especificar endereços IP permitidos em um dos seguintes formatos: Endereço IpV4 com 4 números, o intervalo de endereços CIDR.

Uma vez a **LiveEvent** começa a receber a contribuição de feed, pode utilizar o seu ponto de extremidade de pré-visualização (URL de pré-visualização para pré-visualizar e validar que estão a receber a transmissão em direto antes da publicação ainda mais. Depois de ter verificado que o fluxo de pré-visualização é bom, pode utilizar o LiveEvent para disponibilizar a transmissão em direto para uma entrega por meio de um ou mais (previamente criado) **pontos finais**. Para tal, crie um novo [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) sobre o **LiveEvent**. 

O **LiveOutput** objeto é como um gravador de banda que irá capturar e registrar a transmissão em direto para um elemento na sua conta de Media Services. O conteúdo gravado será persistido para a conta de armazenamento do Azure anexada à sua conta, no contêiner definido pelo recurso do recurso.  O **LiveOuput** também permite-lhe controlar algumas propriedades do fluxo em direto saída, por exemplo, a quantidade da transmissão em fluxo é mantida na gravação do arquivo (por exemplo, a capacidade de DVR na cloud). O arquivo no disco é um arquivo circular "janela" que contém apenas a quantidade de conteúdo especificado na **archiveWindowLength** propriedade da **LiveOutput**. Conteúdo que se encontre fora essa janela é descartado automaticamente do contentor de armazenamento e não é recuperável. Pode criar vários LiveOutputs (até três máximo) num LiveEvent com definições e comprimentos de arquivo diferente.  

Com serviços de multimédia pode tirar partido das **empacotamento dinâmico**, que permite-lhe visualizar e difusão de transmissões em direto na [formatos MPEG DASH, HLS e Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) de contribuição de feed que são enviados para o serviço. Os visualizadores têm acesso podem reproduzir a transmissão em direto com qualquer leitores compatível com HLS, TRAÇO ou transmissão em fluxo uniforme. Pode usar [leitor de multimédia do Azure](http://amp.azure.net/libs/amp/latest/docs/index.html) no seu web ou aplicações móveis para fornecer a sua transmissão em fluxo em qualquer um desses protocolos.

Serviços de multimédia permite-lhe forneça o conteúdo encriptado dinamicamente (**encriptação dinâmica**) com o Advanced Encryption Standard (AES-128) ou qualquer um dos três sistemas de gestão (DRM) de direitos digitais principais: Microsoft PlayReady, Widevine da Google e Apple FairPlay. Serviços de multimédia também fornecem um serviço para entregar licenças DRM e de chaves AES para os clientes autorizados. Para obter mais informações sobre como encriptar o seu conteúdo com os Media Services, consulte [proteger descrição geral do conteúdo](content-protection-overview.md)

Se assim o desejar, também pode aplicar filtragem dinâmica, que podem ser utilizadas para controlar o número de faixas, formatos, velocidades de transmissão e janelas de tempo de apresentação que são enviadas para os jogadores. Para obter mais informações, consulte [filtros e dos manifestos dinâmicos](filters-dynamic-manifest-overview.md).

### <a name="new-capabilities-for-live-streaming-in-v3"></a>Novos recursos para transmissão em fluxo em direto da v3

Com a v3 APIs de serviços de multimédia, se beneficiar as novas funcionalidades seguintes:

- Novo modo de baixa latência. Para obter mais informações, consulte [latência](live-event-latency.md).
- Suporte RTMP aprimorado (maior estabilidade e mais suporte do codificador de origem).
- Ingerir RTMPS seguro.<br/>Quando cria um LiveEvent, obter 4 URLs de inserção. O 4 ingerir URLs são quase idênticos, ter o mesmo token de transmissão em fluxo (AppId), apenas a parte do número de porta é diferente. Dois dos URLs são principais e cópia de segurança para RTMPS.   
- Pode transmitir em fluxo eventos em direto que estão até 24 horas longas quando utilizar os serviços de mídia à transcodificação uma contribuição de velocidade de transmissão única para alimentar num fluxo de saída que tenha múltiplas velocidades de transmissão. 

## <a name="liveevent-types"></a>Tipos de LiveEvent

R [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) pode ser um dos dois tipos: codificação de pass-through e em direto. 

### <a name="pass-through"></a>Pass-through

![pass-through](./media/live-streaming/pass-through.png)

Ao usar o LiveEvent pass-through, contar seu codificador em direto de locais para gerar um fluxo de vídeo de velocidade de transmissão múltiplas e enviar como a contribuição feed para LiveEvent (usando o protocolo RTMP ou MP4 fragmentado). Em seguida, o LiveEvent transporta por meio dos fluxos de vídeo de entrada sem qualquer processamento adicional. Tal um LiveEvent pass-through está otimizado para eventos em direto de longa execução ou transmissão em direto lineares 24 x 365. Ao criar este tipo de LiveEvent, especifique nenhum (LiveEventEncodingType.None).

Pode enviar a contribuição de feed em resoluções até 4 K e num quadro de taxa de 60 quadros por segundo, com H.264/AVC ou H.265/HEVC codecs de vídeo e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio.  Consulte a [LiveEvent tipos de comparação e limitações](live-event-types-comparison.md) artigo para obter mais detalhes.

> [!NOTE]
> A utilização de um método pass-through é a forma mais económica de realizar uma transmissão em fluxo em direto quando estiver a realizar vários eventos durante um longo período de tempo e já investiu em codificadores no local. Consulte os detalhes dos [preços](https://azure.microsoft.com/pricing/details/media-services/).
> 

Ver um exemplo em direto no [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![codificação do Live](./media/live-streaming/live-encoding.png)

Ao utilizar o live encoding com Media Services, poderia configurar a seu codificador em direto de locais para enviar uma velocidade de transmissão única vídeo como a contribuição do feed LiveEvent (usando o protocolo RTMP ou Mp4 fragmentado). O LiveEvent codifica essa entrada velocidade de transmissão única transmitir para um [vários transmissão em fluxo vídeo](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), torna-o disponível para entrega ao reproduzir os dispositivos através de protocolos como MPEG-DASH, HLS e Smooth Streaming. Ao criar este tipo de LiveEvent, especifique o tipo de codificação **padrão** (LiveEventEncodingType.Standard).

Pode enviar a contribuição de feed de cada até 1080 resolução numa taxa de quadros de 30 quadros/segundo, com o codec de vídeo H.264/AVC e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio. Consulte a [LiveEvent tipos de comparação e limitações](live-event-types-comparison.md) artigo para obter mais detalhes.

## <a name="liveevent-types-comparison"></a>Comparação de tipos de LiveEvent

O seguinte artigo contém uma tabela que compara as funcionalidades dos dois tipos de LiveEvent: [Comparação](live-event-types-comparison.md).

## <a name="liveoutput"></a>LiveOutput

R [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) permite-lhe controlar as propriedades do fluxo em direto saída, como muito da transmissão em fluxo é registrada (por exemplo, a capacidade de DVR na cloud) e se é ou não podem iniciar os visualizadores assistem à transmissão em direto. A relação entre um **LiveEvent** e a respetiva **LiveOutput**relação s é semelhante à transmissão de televisão tradicional, no qual um canal (**LiveEvent**) representa um fluxo constante de vídeo e uma gravação (**LiveOutput**) tem um âmbito para um segmento de tempo específico (por exemplo, noite notícias da 6 17:30, para as 19:00: 00). Pode gravar programas de televisão com um gravador de vídeo Digital (DVR) – a funcionalidade equivalente no LiveEvents é gerenciada por meio da propriedade ArchiveWindowLength. É uma duração de período de tempo de ISO-8601 (por exemplo, PTHH:MM:SS), que especifica a capacidade do DVR e pode ser definida a partir de um mínimo de 3 minutos até um máximo de 25 horas.

> [!NOTE]
> **LiveOutput**s início após a criação e param quando eliminado. Ao eliminar a **LiveOutput**, não está a eliminar subjacentes **Asset** e conteúdo no ativo. 
>
> Se tiver publicado **localizador de transmissão em fluxo**s no ativo para o **LiveOutput**, o evento (até o tamanho de janela DVR) irá continuar a ser visualizado até a hora de fim do **localizador de transmissão em fluxo**  ou até ao eliminar o localizador, o que ocorrer primeiro.   

Para obter mais informações, consulte [DVR na cloud do Using](live-event-cloud-dvr.md).

## <a name="streamingendpoint"></a>StreamingEndpoint

Assim que tiver o fluxo a ser encaminhados para o **LiveEvent**, pode começar o evento de transmissão em fluxo através da criação de um **Asset**, **LiveOutput**, e **StreamingLocator** . Isto irá arquivar a transmissão e disponibilizá-la para os espetadores através da [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Quando a conta de serviços de multimédia é criada uma predefinição de ponto final de transmissão em fluxo é adicionada à sua conta no estado parado. Para iniciar o seu conteúdo de transmissão em fluxo e tirar partido do empacotamento e encriptação dinâmica, o ponto final de transmissão em fluxo do qual pretende transmitir conteúdo tem de estar no Estado em execução.

## <a name="a-idbilling-liveevent-states-and-billing"></a><a id="billing" />Estados de LiveEvent e faturação

Um LiveEvent começa assim que o seu estado faz a transição para de faturação **em execução**. Para parar o LiveEvent de faturação, terá de parar o LiveEvent.

Para obter informações detalhadas, consulte [Estados e faturação](live-event-states-billing.md).

## <a name="latency"></a>Latência

Para obter informações detalhadas sobre a latência de LiveEvents, consulte [latência](live-event-latency.md).

## <a name="next-steps"></a>Passos Seguintes

- [Comparação de tipos de LiveEvent](live-event-types-comparison.md)
- [Estados e faturação](live-event-states-billing.md)
- [Latência](live-event-latency.md)
