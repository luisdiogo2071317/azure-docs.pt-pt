---
title: Descrição geral da transmissão em direto através dos serviços de multimédia do Azure | Documentos da Microsoft
description: Isso permite o tópico em direto de uma descrição geral da transmissão em fluxo através dos serviços de multimédia do Azure v3.
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
ms.date: 11/08/2018
ms.author: juliako
ms.openlocfilehash: a4569505cb9a42f6682391a8b06725dea5e539dc
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344982"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Transmissão em direto com Media Services do Azure v3

Os seguintes componentes estão normalmente relacionados ao entregar eventos de transmissão em fluxo em direto com Media Services do Azure:

* Uma câmara, que é utilizada para difundir um evento.
* Um codificador vídeo em direto que converte sinais da câmara (ou outro dispositivo, como o laptop) para fluxos que são enviados para o serviço de transmissão em direto. Os sinais podem também incluir SCTE 35 e Ad-indicações de publicidade. 
* O serviço de suporte de dados dos Serviços Live Streaming permite-lhe ingerir, pré-visualizar, empacotamento, gravar, encriptar e transmitir o conteúdo para seus clientes ou para uma CDN para uma maior distribuição.

Este artigo fornece uma visão geral detalhada e inclui diagramas de componentes principais envolvidos na transmissão em fluxo em direto com os serviços de multimédia.

## <a name="overview-of-main-components"></a>Descrição geral dos componentes principais

Nos Serviços de Multimédia, os [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis por processar o conteúdo da transmissão em fluxo em direto. Um LiveEvent fornece um ponto de final de entrada (URL de ingestão) que, em seguida, forneça a um codificador em direto no local. O LiveEvent recebe transmissões em direto de entrada do codificador em direto no formato RTMP ou Smooth Streaming e disponibiliza-o para transmissão em fluxo através de um ou mais [pontos finais](https://docs.microsoft.com/rest/api/media/streamingendpoints). R [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) permite-lhe controlar a publicação, gravação e as definições de intervalo DVR da transmissão em fluxo em direto. O LiveEvent também fornece um ponto de extremidade pré-visualização (URL de pré-visualização) que utilizar para pré-visualizar e validar a sua transmissão antes de mais processamentos e entregas. 

Os Media Services fornecem **empacotamento dinâmico**, que lhe permite pré-visualizar e seu conteúdo em MPEG DASH, HLS, Smooth Streaming de difusão em fluxo em formatos sem ter de reempacotar manualmente para estes formatos de transmissão em fluxo. Pode reproduzir com jogadores compatíveis uniformes, DASH ou HLS. Também pode utilizar [leitor de multimédia do Azure](http://amp.azure.net/libs/amp/latest/docs/index.html) para testar a sua transmissão em fluxo.

Serviços de multimédia permite-lhe forneça o conteúdo encriptado dinamicamente (**encriptação dinâmica**) com o Advanced Encryption Standard (AES-128) ou qualquer um dos três sistemas de gestão (DRM) de direitos digitais principais: Microsoft PlayReady, Google Widevine e FairPlay da Apple. Serviços de multimédia também fornecem um serviço para entrega de chaves AES e o DRM (PlayReady, Widevine e FairPlay) licenças para os clientes autorizados.

Se assim o desejar, também pode aplicar **filtragem dinâmica**, que podem ser utilizadas para controlar o número de faixas, formatos, velocidades de transmissão, o que são enviadas para os jogadores. Serviços de multimédia também suporta a inserção de publicidade.

### <a name="new-live-encoding-improvements"></a>Novas melhorias de codificação em direto

Os seguintes aprimoramentos novos foram feitos na versão mais recente.

- Novo modo de baixa latência. Para obter mais informações, consulte [latência](#latency).
- Suporte RTMP aprimorado (maior estabilidade e mais suporte do codificador de origem).
- Ingerir RTMPS seguro.

    Quando cria um LiveEvent, obter 4 URLs de inserção. O 4 ingerir URLs são quase idênticos, ter o mesmo token de transmissão em fluxo (AppId), apenas a parte do número de porta é diferente. Dois dos URLs são principais e cópia de segurança para RTMPS.   
- suporte de transcodificação de 24 horas. 
- Suporte aprimorado a sinalização do ad no RTMP via SCTE35.

## <a name="liveevent-types"></a>Tipos de LiveEvent

R [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) pode ser um dos dois tipos: codificação em tempo real e pass-through. 

### <a name="live-encoding-with-media-services"></a>Live encoding com Media Services

![codificação do Live](./media/live-streaming/live-encoding.png)

Um codificador em direto no local envia um fluxo de velocidade de transmissão única para o LiveEvent ativado para realizar live encoding com Media Services dos seguintes protocolos: RTMP ou transmissão em fluxo uniforme (MP4 fragmentado). O LiveEvent, em seguida, executa a codificação em direto de entrada fluxo de velocidade de transmissão única para um fluxo de vídeo de velocidade de transmissão (adaptável). Quando solicitado, os Media Services disponibilizam a transmissão em fluxo para os clientes.

Ao criar este tipo de LiveEvent, especifique **básica** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Pass-through

![pass-through](./media/live-streaming/pass-through.png)

Pass-through está otimizado para transmissões em fluxo de execução longa ou 24x7 linear codificação em tempo real utilizando um codificador em direto no local. O codificador no local envia múltipla **RTMP** ou **Smooth Streaming** (MP4 fragmentado) para o que está configurado para LiveEvent **pass-through** entrega. O **pass-through** entrega ocorre quando as transmissões em fluxo ingeridas passam pelos **LiveEvent**s sem qualquer processamento adicional. 

Pass-through LiveEvents pode oferecer suporte a cópia de segurança para a resolução de 4K e HEVC passar quando utilizado com a transmissão em fluxo uniforme protocolo de ingestão. 

Ao criar este tipo de LiveEvent, especifique **None** (LiveEventEncodingType.None).

> [!NOTE]
> A utilização de um método pass-through é a forma mais económica de realizar uma transmissão em fluxo em direto quando estiver a realizar vários eventos durante um longo período de tempo e já investiu em codificadores no local. Consulte os detalhes dos [preços](https://azure.microsoft.com/pricing/details/media-services/).
> 

## <a name="liveevent-types-comparison"></a>Comparação de tipos de LiveEvent 

A tabela seguinte compara as funcionalidades dos dois tipos de LiveEvent.

| Funcionalidade | LiveEvent pass-through | LiveEvent padrão |
| --- | --- | --- |
| Velocidade de transmissão única entrada é codificada em múltiplas velocidades de transmissão na cloud |Não |Sim |
| Resolução máxima, número de camadas |4Kp30  |720p, 6 camadas, 30 fps |
| Protocolos de entrada |RTMP, Smooth Streaming |RTMP, Smooth Streaming |
| Preço |Consulte a [página de preços](https://azure.microsoft.com/pricing/details/media-services/) e clique no separador "Vídeo em direto" |Consulte o [página de preços](https://azure.microsoft.com/pricing/details/media-services/) |
| Tempo de execução máximo |24x7 |24x7 |
| Suporte para a inserção de slates |Não |Sim |
| Suporte para ad sinalização através da API|Não |Sim |
| Suporte para ad sinalização via SCTE35 inband|Sim |Sim |
| Legendas de legenda oculta CEA 608/708 pass-through |Sim |Sim |
| Capacidade de recuperar de paralisações breves no feed de contribuição |Sim |Não (LiveEvent começará slating após 6 + segundos sem dados de entrada)|
| Suporte para não-uniforme GOPs de entrada |Sim |Não – entrada deve ser corrigida GOPs de seg de 2 |
| Suporte para entrada de taxa de quadros de variável |Sim |Não – entrada deve ser corrigida taxa de quadros.<br/>Secundárias variações são pela tolerar, por exemplo, durante o plano de movimento elevada. Mas codificador não é possível remover a 10 quadros por segundo. |
| Auto-shutoff de LiveEvent quando a introdução do feed é perdido |Não |Após 12 horas, se não houver nenhum LiveOutput em execução |

## <a name="liveevent-states"></a>Estados de LiveEvent 

O estado atual de um LiveEvent. Os valores possíveis incluem:

|Estado|Descrição|
|---|---|
|**Parado**| Este é o estado inicial do LiveEvent após sua criação (a menos que o início automático foi selecionado especificado.) Ocorre uma faturação sem neste estado. Neste estado, as propriedades de LiveEvent podem ser atualizadas, mas não é permitida a transmissão em fluxo.|
|**A partir de**| O LiveEvent está a ser iniciado. Ocorre uma faturação sem neste estado. Não são permitidas transmissões em fluxo nem atualizações durante este estado. Se ocorrer um erro, devolve o LiveEvent para o estado parado.|
|**Em execução**| O LiveEvent é capaz de processar transmissões em fluxo. Ele é agora utilização de faturação. Tem de parar o LiveEvent para impedir que mais de faturação.|
|**A parar**| O LiveEvent está a ser parado. Ocorre uma faturação sem neste estado transitório. Não são permitidas transmissões em fluxo nem atualizações durante este estado.|
|**A eliminar**| O LiveEvent está a ser eliminado. Ocorre uma faturação sem neste estado transitório. Não são permitidas transmissões em fluxo nem atualizações durante este estado.|

## <a name="liveoutput"></a>LiveOutput

R [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) permite-lhe controlar a publicação, gravação e as definições de intervalo DVR da transmissão em fluxo em direto. A relação LiveEvent e LiveOutput é semelhante à multimédia tradicional onde um canal (LiveEvent) tem um fluxo constante de conteúdo e um programa (LiveOutput) está confinado a alguns eventos temporizados nesse LiveEvent.
Pode especificar o número de horas que pretenda manter o conteúdo gravado para o LiveOutput definindo a **ArchiveWindowLength** propriedade. **ArchiveWindowLength** é uma duração do período de tempo de ISO 8601 de extensão de janela de arquivo (gravador de vídeo Digital ou DVR). Este valor pode ser definido a partir de um mínimo de 5 minutos até um máximo de 25 horas. 

**ArchiveWindowLength** dita também o número máximo de clientes de tempo pode recuar a partir da posição atual em direto. LiveOutputs podem a ser executados durante o período de tempo especificado, mas o conteúdo que se situe atrás da duração da janela é continuamente descartado. O valor desta propriedade também determina quanto tempo os manifestos dos clientes podem aumentar.

Cada LiveOutput está associado um [Asset](https://docs.microsoft.com/rest/api/media/assets)e regista os dados para um contentor no armazenamento do Azure anexado à conta de Media Services. Para publicar o LiveOutput, tem de criar uma [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) para o elemento associado. Ter este localizador irá permitir compilar um URL de transmissão em fluxo que pode fornecer aos seus clientes.

Um LiveEvent suporta até três LiveOutputs em execução em simultâneo para que possa criar vários arquivos da mesma transmissão em fluxo recebido. Isto permite publicar e arquivar diferentes partes de um evento, conforme necessário. Por exemplo, é de seus requisitos de negócios difundir um feed de linear em direto 24x7, mas desejar criar "gravações" de programas ao longo do dia para oferecer aos clientes como conteúdo sob demanda para visualização catch-up.  Para este cenário, primeiro crie um LiveOutput principal, com uma janela de arquivo curtos de 1 hora ou menos para os clientes ajustar-se em como o principal em fluxo em direto. Seria criar um StreamingLocator para este LiveOutput e publicá-lo para a sua aplicação ou site como o feed de "Live".  Como o feed está em execução, é possível criar programaticamente um segundo LiveOutput simultânea no início de um programa (ou 5 minutos desde o início para fornecer alguns identificadores para cortar mais tarde.) Este segundo LiveOutput pode ser parado a 5 minutos após o final do programa ou de um evento e, em seguida, pode criar um novo StreamingLocator para publicar este programa como um recurso de demanda no catálogo da sua aplicação.  Pode repetir várias vezes esse processo para outros limites do programa ou destaca que pretende partilhar imediatamente como sob demanda, estando "Live" feed a partir do primeiro LiveOutput continua a difundir o feed linear.  Além disso, pode aproveitar o suporte de filtro dinâmico de cortar os principais e de cauda do arquivo do LiveOutput que introduziu para "segurança de sobreposição" entre programas e obter um início e de fim do conteúdo mais precisas. Conteúdo arquivado também pode ser submetido para uma [transformar](https://docs.microsoft.com/rest/api/media/transforms) para codificação ou subdistorção exata do quadro em vários formatos de saída a ser utilizado como a outros serviços de distribuição.

## <a name="streamingendpoint"></a>StreamingEndpoint

Assim que a transmissão em fluxo estiver a ser enviada para o LiveEvent, pode iniciar o evento de transmissão em fluxo através da criação de um Elemento, de LiveOutput e de StreamingLocator. Isto irá arquivar a transmissão e disponibilizá-la para os espetadores através da [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Quando a conta de serviços de multimédia é criada uma predefinição de ponto final de transmissão em fluxo é adicionada à sua conta no estado parado. Para iniciar o seu conteúdo de transmissão em fluxo e tirar partido do empacotamento e encriptação dinâmica, o ponto final de transmissão em fluxo do qual pretende transmitir conteúdo tem de estar no Estado em execução.

## <a name="latency"></a>Latência

Esta seção discute típicos resultados que vê ao utilizar as definições de baixa latência e de vários jogadores. Os resultados variam com base na latência de rede e da CDN.

Para usar o novo recurso de LowLatency, pode definir o **StreamOptionsFlag** ao **LowLatency** no LiveEvent. Depois do fluxo está em execução, pode utilizar o [leitor de multimédia do Azure](http://ampdemo.azureedge.net/) (AMP), página de demonstração e definir as opções de reprodução para utilizar o "baixa latência heurística perfil".

### <a name="pass-through-liveevents"></a>LiveEvents pass-through

||2s GOP baixa latência ativada|1s GOP baixa latência ativada|
|---|---|---|
|TRAVESSÃO no AMP|10s|8S|
|HLS no player de iOS nativo|14s|10s|
|HLS. JS num leitor do Mixer|30s|16s|

## <a name="billing"></a>Faturação

Um LiveEvent começa a faturação assim que o estado muda para "Em execução". Para parar o LiveEvent de faturação, terá de parar o LiveEvent.

> [!NOTE]
> Quando **LiveEventEncodingType** no seu [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) está definido para básico, a serviços de multimédia será automaticamente shutoff de qualquer LiveEvent que ainda está no estado "A executar" 12 horas depois do feed de entrada é perdido e há nenhuma Execução de LiveOutputs. No entanto, ainda será cobrado pelo tempo que a LiveEvent esteve no estado "Em execução".
>

A tabela seguinte mostra como os Estados de LiveEvent mapeiam para o modo de faturação.

| Estado de LiveEvent | É a faturação? |
| --- | --- |
| A iniciar |Não (estado transitório) |
| A executar |SIM |
| A parar |Não (estado transitório) |
| Parada |Não |

## <a name="next-steps"></a>Passos Seguintes

[Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)
