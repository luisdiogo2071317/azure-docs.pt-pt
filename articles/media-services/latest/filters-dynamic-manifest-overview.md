---
title: Manifestos dinâmicos, filtros e dos serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico descreve como criar filtros para que o cliente possa utilizar secções específicas do fluxo de um fluxo. Serviços de multimédia cria os manifestos dinâmicos para arquivar este seletiva de transmissão em fluxo.
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
ms.date: 12/20/2018
ms.author: juliako
ms.openlocfilehash: 7dc2136fe6ee28da0583ebdb2b2749ddf1c37049
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728045"
---
# <a name="filters-and-dynamic-manifests"></a>Filtros e dos manifestos dinâmicos

Quando a entrega de conteúdo aos clientes (transmissão em fluxo eventos em direto ou de vídeo a pedido) o cliente poderá ter mais flexibilidade do que o que é descrito no arquivo de manifesto do recurso padrão. Serviços de multimédia do Azure permite-lhe definir os filtros de conta e filtros de recurso para o seu conteúdo. 

Os filtros são regras do lado do servidor que permitem que os clientes podem fazer coisas como: 

- Reproduza apenas uma secção de um vídeo (em vez de reproduzir o vídeo todo). Por exemplo:

    - Reduzir o manifesto para mostrar um subclip de um evento em direto ("subclips filtragem"), ou
    - Corte o início de um vídeo ("recortar um vídeo").

- Entrega apenas as representações especificadas e/ou faixas de idioma especificado que são suportadas pelo dispositivo que é utilizado para reproduzir o conteúdo ("filtragem de representação"). 
- Ajuste a janela de apresentação (DVR) a fim de fornecer uma duração limitada da janela DVR no player de ("Ajuste janela apresentação").

Este tópico descreve [conceitos](#concepts) e [mostra as definições de filtros](#definitions). Em seguida, fornece detalhes sobre [cenários comuns](#common-scenarios). No final do artigo, encontrará links que mostram como criar filtros de forma programática.  

## <a name="concepts"></a>Conceitos

### <a name="dynamic-manifests"></a>Manifestos dinâmicos

Serviços de multimédia oferecem **manifestos dinâmica** com base no predefinidas [filtros](#filters). Depois de definir filtros, os clientes poderiam usá-los para transmitir uma representação em específica ou de clips secundárias do seu vídeo. Eles deve especificar filtros no URL de transmissão em fluxo. Filtros podem ser aplicados a velocidade de transmissão adaptável, protocolos de transmissão em fluxo: Apple HTTP Live Streaming (HLS), MPEG-DASH e Smooth Streaming. 

A tabela seguinte mostra alguns exemplos de URLs com filtros:

|Protocolo|Exemplo|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Transmissão em Fluxo Uniforme|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

> [!NOTE]
> Os manifestos dinâmicos não altere o elemento e o manifesto de padrão para esse recurso. O cliente pode optar por solicitar um fluxo com ou sem filtros. 
> 
> 

### <a name="manifest-files"></a>Ficheiros de manifesto

Quando codifica um recurso para transmissão em fluxo de velocidade de transmissão adaptável, um **manifesto** é criado (lista de reprodução) ficheiro (o arquivo é baseado em texto ou baseado em XML). O **manifesto** ficheiro inclui a transmissão em fluxo de metadados, tais como: controlar o tipo (áudio, vídeo ou texto), controlar o nome, hora de início e fim, a velocidade de transmissão (qualidades), linguagens de controle, a janela de apresentação (janela deslizante de duração fixa), (de codec de vídeo FourCC). Também instrui o player para recuperar o fragmento seguinte, fornecendo informações sobre os seguintes pode ser reproduzidos vídeo fragmentos disponíveis e a respetiva localização. Fragmentos de (ou segmentos) são os reais "segmentos" de um conteúdo de vídeo.

Eis um exemplo de um ficheiro de manifesto HLS: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Obter e examinar os arquivos de manifesto

Especifique uma lista de condições de propriedade de controle de filtro com base no qual as faixas de sua transmissão em fluxo (Live ou vídeo a pedido) devem ser incluídas no manifesto criado dinamicamente. Para obter e examinar as propriedades dos roteiros, terá de carregar o manifesto de transmissão em fluxo uniforme primeiro.

O [carregar, codificar e transmitir ficheiros com .NET](stream-files-tutorial-with-api.md) tutorial mostra-lhe como criar os URLs de transmissão em fluxo com o .NET (ver, o [criação URLs](stream-files-tutorial-with-api.md#get-streaming-urls) secção). Se executar a aplicação, um dos URLs de aponta para o manifesto de transmissão em fluxo uniforme: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Copie e cole o URL na barra de endereço do browser. O ficheiro será transferido. Pode abri-lo num editor de texto à sua escolha.

No exemplo do REST, veja [carregar, codificar e transmitir ficheiros com REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>Monitorizar a velocidade de transmissão de um fluxo de vídeo

Pode utilizar o [página de demonstração do leitor de multimédia do Azure](http://aka.ms/amp) para monitorizar a velocidade de transmissão de um fluxo de vídeo. A página de demonstração exibe as informações de diagnóstico do **diagnóstico** separador:

![Diagnóstico do leitor de multimédia do Azure][amp_diagnostics]

## <a name="defining-filters"></a>Definir filtros

Existem dois tipos de filtros ativos: 

* [Filtros de conta](https://docs.microsoft.com/rest/api/media/accountfilters) (global) - podem ser aplicados a qualquer recurso na conta de Media Services do Azure, têm uma duração da conta.
* [Filtros ativos](https://docs.microsoft.com/rest/api/media/assetfilters) (local) - apenas pode ser aplicado a um recurso com a qual o filtro foi associado após a criação, ter um tempo de vida do ativo. 

[Filtro de contas](https://docs.microsoft.com/rest/api/media/accountfilters) e [Asset filtro](https://docs.microsoft.com/rest/api/media/assetfilters) tipos têm exatamente as mesmas propriedades para definir/descrever o filtro. Exceto quando criar o **filtro de elemento**, tem de especificar o nome do elemento com a qual pretende associar o filtro.

Dependendo do seu cenário, decidir que tipo de um filtro é mais adequada (filtro de elemento ou filtro de conta). Filtros de conta são adequados para perfis de dispositivo (filtragem de representação) onde os filtros ativos poderia ser usados para cortar um recurso específico.

Utilize as seguintes propriedades para descrever os filtros. 

|Nome|Descrição|
|---|---|
|firstQuality|A primeira qualidade velocidade de transmissão do filtro.|
|PresentationTimeRange|O intervalo de tempo de apresentação. Esta propriedade é utilizada para filtragem de pontos de início/fim manifesto, duração da janela de apresentação e a posição inicial em direto. <br/>Para obter mais informações, consulte [PresentationTimeRange](#PresentationTimeRange).|
|roteiros|As condições de seleção de faixas. Para obter mais informações, consulte [roteiros](#tracks)|

### <a name="presentationtimerange"></a>PresentationTimeRange

Use essa propriedade com o **Asset filtros**. Não é recomendado para definir a propriedade com o **filtros de conta**.

|Nome|Descrição|
|---|---|
|**endTimestamp**|O limite de tempo absoluto final. Aplica-se para vídeo a pedido (VoD). Para a apresentação em direto, é ignorada e aplicada automaticamente quando as extremidades de apresentação e o fluxo de se tornar VoD.<br/><br/>O valor representa um ponto final absoluto da transmissão em fluxo. -É arredondado para o próximo início de GOP mais próximo.<br/><br/>Utilize StartTimestamp e EndTimestamp para cortar a lista de reprodução (manifesto). Por exemplo, StartTimestamp = 40000000 e EndTimestamp = 100000000 irá gerar uma lista de reprodução que contém o suporte de dados entre StartTimestamp e EndTimestamp. Se um fragmento faz a ponte do limite, o fragmento todo será incluído no manifesto.<br/><br/>Além disso, veja a **forceEndTimestamp** definição que se segue.|
|**forceEndTimestamp**|Aplica-se aos filtros em direto.<br/><br/>**forceEndTimestamp** é um valor booleano que indica se é ou não **endTimestamp** foi definido para um valor válido. <br/><br/>Se o valor for **true**, o **endTimestamp** deve ser especificado qualquer valor. Se não for especificada, em seguida, é devolvido um pedido incorreto.<br/><br/>Se, por exemplo, pretende definir um filtro que começa em 5 minutos para o vídeo de entrada e dura até ao final da transmissão em fluxo, definiria **forceEndTimestamp** como falso e omitir a definição **endTimestamp**.|
|**liveBackoffDuration**|Aplica-se em direto apenas. A propriedade é usada para definir a posição de reprodução em direto. Com esta regra, pode atrasar a posição de reprodução em direto e criar uma memória intermédia do lado do servidor para jogadores. LiveBackoffDuration é relativo a posição em direto. A duração máxima de término em direto é de 60 segundos.|
|**presentationWindowDuration**|Aplica-se em direto. Uso **presentationWindowDuration** para aplicar uma janela deslizante para a lista de reprodução. Por exemplo, definir presentationWindowDuration = 1200000000 para aplicar uma janela deslizante de dois minutos. Suporte de dados em dois minutos de borda em direto serão incluídos na playlist. Se um fragmento faz a ponte do limite, o fragmento todo será incluído na playlist. Duração da janela de apresentação mínimo é de 120 segundos.|
|**startTimestamp**|Aplica-se aos fluxos VoD ou em direto. O valor representa um ponto de início absoluto da transmissão em fluxo. O valor é arredondado para o próximo início de GOP mais próximo.<br/><br/>Uso **startTimestamp** e **endTimestamp** para cortar a lista de reprodução (manifesto). Por exemplo, startTimestamp = 40000000 e endTimestamp = 100000000 irá gerar uma lista de reprodução que contém o suporte de dados entre StartTimestamp e EndTimestamp. Se um fragmento faz a ponte do limite, o fragmento todo será incluído no manifesto.|
|**escala temporal**|Aplica-se aos fluxos VoD ou em direto. A escala temporal utilizado pelos carimbos e durações especificadas acima. A escala temporal padrão é 10000000. Pode ser utilizada uma escala temporal alternativo. A predefinição é 10000000 HNS (nanossegundos centenas).|

### <a name="tracks"></a>roteiros

Especifique uma lista de condições de propriedade de controle de filtro (FilterTrackPropertyConditions) com base no qual as faixas de sua transmissão em fluxo (Live ou vídeo a pedido) devem ser incluídas no manifesto criado dinamicamente. Os filtros são combinados com uma lógica **AND** e **ou** operação.

Condições de propriedade de controle de filtro descrevem os tipos de controlo, valores (descritos na tabela a seguir) e operações (igual, NotEqual). 

|Nome|Descrição|
|---|---|
|**Velocidade de transmissão**|Utilize a velocidade de transmissão da faixa para filtragem.<br/><br/>O valor recomendado é um intervalo de velocidades de transmissão em bits por segundo. Por exemplo, "0-2427000".<br/><br/>Nota: Embora seja possível usar um valor de velocidade de transmissão específica, como 250000 (bits por segundo), essa abordagem não é recomendada, como as velocidades de transmissão exatas podem variar de um recurso para outro.|
|**FourCC**|Utilize o valor de FourCC da faixa de filtragem.<br/><br/>O valor é o primeiro elemento do formato de codecs, conforme especificado nas [RFC 6381](https://tools.ietf.org/html/rfc6381). Atualmente, são suportados os codecs seguintes: <br/>Vídeo: "Avc1", "hev1", "hvc1"<br/>Para áudio: "Mp4a", da "ec-3"<br/><br/>Para determinar os valores de FourCC roteiros num elemento [obter e examinar o arquivo de manifesto](#get-and-examine-manifest-files).|
|**Language** (Idioma)|Utilize a linguagem da faixa para filtragem.<br/><br/>O valor é a marca de um idioma que pretende incluir, como especificado no RFC 5646. Por exemplo, "pt".|
|**Nome**|Utilize o nome da faixa para filtragem.|
|**Tipo**|Utilize o tipo da faixa para filtragem.<br/><br/>São permitidos os seguintes valores: "vídeo", "áudio" ou "text".|

### <a name="example"></a>Exemplo

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="rendition-filtering"></a>Filtragem de representação

Pode optar por codificar o seu elemento para vários perfis de codificação (linha de base H.264, H.264 elevada, AACL, AACH, Dolby Digital mais) e múltiplas velocidades de transmissão de qualidade. No entanto, nem todos os dispositivos de cliente irão suportar perfis de todos os seus ativos e velocidades de transmissão. Por exemplo, os dispositivos Android mais antigos só suportam H.264 da linha de base + AACL. Enviando velocidades de transmissão superior a um dispositivo, que não é possível obter os benefícios, desperdiça largura de banda e a computação de dispositivo. Tais dispositivos preciso decodificar todas as informações de determinado, apenas para a dimensionar para baixo para exibição.

Com um manifesto dinâmico, pode criar perfis de dispositivo, como dispositivos móveis, consola, HD/SD, etc. e incluem os roteiros e qualidades, o que deseja fazer parte de cada perfil.

![Exemplo de filtragem de representação][renditions2]

No exemplo a seguir, um codificador foi utilizado para codificar um recurso de mezanino em sete representações de vídeo de MP4s de ISO (a partir de 180p para 1080p). O elemento codificado pode ser empacotado dinamicamente em qualquer um dos seguintes protocolos de transmissão em fluxo: HLS, MPEG DASH e Smooth.  Na parte superior do diagrama, é mostrado o manifesto HLS para o elemento com sem filtros (que contém todas as representações de sete).  Na parte inferior esquerda, é mostrado o manifesto HLS ao qual foi aplicado um filtro com o nome "ott". Especifica o filtro de "ott" para remover todas as velocidades de transmissão abaixo 1 Mbps, o que resultou nos níveis de qualidade dois na parte inferior a ser eliminados na resposta. Na parte inferior direita, é mostrado o manifesto HLS ao qual foi aplicado um filtro com o nome "móvel". O filtro "móvel" Especifica a remover representações em que a resolução é maior do que 720p, o que resultou em duas representações de 1080p a ser suprimidas.

![Filtragem de representação][renditions1]

## <a name="removing-language-tracks"></a>Remover faixas de idioma
Os recursos podem incluir vários idiomas de áudio, como inglês, espanhol, francês, etc. Normalmente, os gerentes de Player SDK predefinido a seleção de faixa de áudio e áudio disponível controla por seleção do usuário. É um desafio para desenvolver tais Player SDKs, ele exige implementações diferentes em estruturas de player específicos do dispositivo. Além disso, em algumas plataformas, APIs de leitor são limitadas e não incluem o recurso de seleção de áudio em que os utilizadores não é possível selecionar ou alterar a faixa de áudio padrão. Com os filtros de recurso, pode controlar o comportamento através da criação de filtros que incluem apenas os idiomas de áudio desejados.

![Filtragem de faixas de idioma][language_filter]

## <a name="trimming-start-of-an-asset"></a>Remoção de início de um ativo
Na maioria dos eventos de transmissão em fluxo em direto, os operadores de executam alguns testes antes do evento real. Por exemplo, eles podem incluir uma ficha, assim, antes do início do evento: "Programa vai começar momentaneamente". Se o programa é arquivar, o teste e os dados de imagem fixa são também arquivados e incluídos na apresentação. No entanto, estas informações não devem ser mostradas aos clientes. Com o manifesto dinâmico, pode criar um filtro de tempo de início e remove os dados indesejados de manifesto.

![Remoção de início][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Criação de subclips (exibições) a partir de um arquivo live
Número de eventos em direto é de longa e arquivo live pode incluir vários eventos. Após o evento ao vivo terminar, emissoras talvez queira dividir este arquivo live no início do programa de lógica e parar sequências. Em seguida, publica esses programas virtual separadamente sem postagem processar este arquivo live e não crie recursos separados (o que não é o benefício dos fragmentos em cache existentes nas CDNs). Exemplos de tais programas virtual são os trimestres de um football ou jogo basquete, innings no beisebol ou eventos individuais de qualquer programa do desporto.

Com o manifesto dinâmico, pode criar filtros com horas de início/fim e criar vistas virtual ao longo da parte superior do seu arquivo live. 

![Filtro do subclip][subclip_filter]

Filtrado Asset:

![Esqui][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Ajustar a janela de apresentação (DVR)
Atualmente, os serviços de multimédia do Azure oferece arquivo circular, onde a duração pode ser configurada entre 5 minutos - 25 horas. Filtragem de manifesto pode ser utilizado para criar uma janela DVR sem interrupção ao longo da parte superior do arquivo, sem eliminar o suporte de dados. Existem muitos cenários em que desejam fornecer uma janela DVR limitada a mover-se com o edge em direto e ao mesmo tempo, manter uma janela de arquivo maior emissoras. A emissora poderá querer utilizar os dados que está fora da janela de DVR para realçar clips ou podem optar por fornecer windows DVR diferentes para diferentes dispositivos. Por exemplo, a maioria dos dispositivos móveis não lidar com grandes windows DVR (pode ter uma janela DVR de 2 minutos, para dispositivos móveis e uma hora para clientes de ambiente de trabalho).

![Janela DVR][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Ajustar LiveBackoff (posição em direto)
Filtragem de manifesto pode ser utilizado para remover vários segundos do edge em direto de um programa em direto. Filtragem permite emissoras Assista à apresentação no ponto de publicação de pré-visualização e criar pontos de inserção de anúncio antes dos visualizadores de recebem o fluxo (de segurança-off por 30 segundos). Emissoras, em seguida, podem enviar estes anúncios para suas estruturas de cliente no tempo para serem recebidos e processar as informações antes da oportunidade de anúncio.

Além do suporte de anúncio, a definição de LiveBackoff pode ser utilizada para ajustar a posição de visualizadores para que quando os clientes de inconsistências e atingir o limite ao vivo ainda podem obter fragmentos do servidor em vez de obter um erro HTTP 404 ou erro 412.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>A combinação de várias regras num único filtro
Pode combinar várias regras de filtragem num único filtro. Por exemplo, pode definir uma "regra de intervalo" para remover slates de um arquivo em direto e filtrar também disponíveis velocidades de transmissão. Ao aplicar várias regras de filtragem, o resultado final é a interseção de todas as regras.

![várias regras][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>A combinação de vários filtros (composição de filtro)

Também pode combinar vários filtros numa única URL. 

O cenário a seguir demonstra por que pode querer combinar os filtros:

1. Tem de filtrar as qualidades de vídeo para dispositivos móveis, como Android ou iPAD (para limitar as qualidades de vídeo). Para remover as qualidades indesejadas, deverá criar um filtro da conta adequada para os perfis de dispositivo. Filtros de conta podem ser utilizados para todos os seus ativos na mesma conta de serviços de multimédia sem qualquer outra associação. 
2. Também queira limitar a hora de início e fim de um ativo. Para conseguir isso, teria de criar um filtro de elemento e definir a hora de início/fim. 
3. Pode querer combinar ambos estes filtros (sem combinação, precisa adicionar filtragem de qualidade para o filtro de remoção, o que dificulta a utilização de filtro).

Para combinar os filtros, tem de definir os nomes de filtro para a manifesto/lista de reprodução URL com ponto e vírgula delimitada. Vamos supor que tem um filtro com o nome *MyMobileDevice* que filtra qualidades e tiver outro denominado *MyStartTime* definir específicos de um hora de início. Pode combiná-los como este:

Pode combinar os filtros de até três. 

Para obter mais informações, consulte [isso](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="considerations-and-limitations"></a>Considerações e limitações

- Os valores para **forceEndTimestamp**, **presentationWindowDuration**, e **liveBackoffDuration** não deve ser definida para um filtro de VoD. Só são utilizadas para cenários de filtro em direto. 
- Manifesto dinâmico funciona em GOP limites (quadros chave), por conseguinte, corte tem precisão GOP. 
- Pode utilizar o mesmo nome de filtro para os filtros de conta e recurso. Filtros de elemento tem precedência e irão substituir os filtros de conta.
- Se atualizar um filtro, pode demorar até 2 minutos para o ponto final de transmissão em fluxo atualizar as regras. Se o conteúdo tiver sido servido com alguns filtros (e armazenado em cache em proxies e CDN caches), a atualizar estes filtros pode resultar em falhas de player. Recomenda-se para limpar a cache depois de atualizar o filtro. Se esta opção não for possível, considere utilizar um filtro de diferente.
- Os clientes têm de manualmente, transfira o manifesto e analisar o startTimestamp exato e a escala de tempo.
    
    - Para determinar as propriedades dos roteiros num recurso [obter e examinar o arquivo de manifesto](#get-and-examine-manifest-files).
    - A fórmula para definir o filtro de elemento timestamp propriedades: <br/>startTimestamp = &lt;hora de início no manifesto&gt; +  &lt;esperado hora de início de filtro em segundos&gt;* escala temporal


## <a name="next-steps"></a>Passos Seguintes

Os artigos seguintes mostram como criar filtros de forma programática.  

- [Criar filtros com as APIs REST](filters-dynamic-manifest-rest-howto.md)
- [Criar filtros com .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Criar filtros com a CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
