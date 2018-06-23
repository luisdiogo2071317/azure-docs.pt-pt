---
title: Incorporar widgets do indexador de vídeo do Azure na sua aplicações | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/04/2018
ms.author: juliako
ms.openlocfilehash: 8da06253c58a2de474e879f18013fa1e57f5668d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355639"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Incorporar widgets do indexador de vídeo para as suas aplicações

Indexador de vídeo suporta incorporar dois tipos de widgets na sua aplicação: **Insights cognitivos** e **Player**. 

* A **Insights cognitivos** widget inclui todas as informações de visual que foram extraídas do seu vídeo de indexação de processo. 
    O widget insights suporta os seguintes parâmetros de URL opcionais:

    |Nome|Definição|Descrição|
    |---|---|---|
    |widgets|Separados por vírgulas de cadeias|Permite-lhe controlar as informações que pretende apresentar. <br/>Exemplo: **widgets = pessoas, marcas** irá compor apenas as pessoas e marcas insights da IU<br/>As opções disponíveis: pessoas, palavras-chave, anotações, marcas, Sentiments, Transcript, pesquisa | 
* A **Player** widget permite-lhe transmitir o vídeo utilizando bits adaptável taxa.

    O widget player suporta os seguintes parâmetros de URL opcionais:

    |Nome|Definição|Descrição|
    |---|---|---|
    |T|Segundos do início|Permite o início de leitor reproduzir desde o momento de determinado ponto.<br/>Exemplo: t = 60|
    |legendas|Código de idioma|Obtém a legenda do idioma especificado durante a miniaplicação carregar esteja disponível no menu de legendas.<br/>Exemplo: legendas = en-Us|
    |showCaptions|Um valor booleano|Faz com que o leitor de carga com legendas ainda esteja ativadas.<br/>Exemplo: showCaptions = true|
    |tipo||Ativa uma skin de áudio player (vídeo parte é removido).<br/>Exemplo: escreva = áudio|
    |reprodução automática ROM|Um valor booleano|Decidir se deve iniciar leitor de reprodução de vídeo quando carregar (a predefinição é verdadeiro).<br/>Exemplo: a reprodução automática ROM = false|
    |Idioma|Código de idioma|Controlo de leitor de controla localização (a predefinição é en-US)<br/>Exemplo: linguagem = Alemanha da Alemanha|

## <a name="embedding-public-content"></a>Ao incorporar conteúdo público

1. Inicie sessão no seu [vídeo indexador](https://api-portal.videoindexer.ai/) conta. 
2. Clique no botão "incorporar" que é apresentado o vídeo abaixo.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Depois de clicar no botão, aparecerá uma modal incorporação no ecrã de onde pode escolher que widget pretende incorporação na sua aplicação.
    Selecionar um widget (**Player** ou **Insights cognitivos**), gera o código incorporado que pode colar na sua aplicação.
 
3. Escolha o tipo de widget pretende (**Insights cognitivos** ou **Player**).
4. Copie o código de incorporação e adicionar à sua aplicação. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Ao incorporar o conteúdo privado

Pode obter incorporar códigos de incorporar os pop-ups (conforme ilustrado na secção anterior) para **pública** vídeos apenas. 

Se pretende incorporar um **privada** vídeo, tem de passar um token de acesso no **iframe**do **src** atributo:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Utilizar o [ **obter Widget de Insights** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API para obter o conteúdo de widget Insights cognitivos, ou utilize [ **obter vídeo acesso Token** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) e que como adicionar um consulta param para o url, conforme mostrado acima. Especifique este URL como o **iframe**do **src** valor.

Se pretender fornecer edição insights capacidades de (como temos na nossa aplicação web) no seu widget incorporado, terá de transmitir um token de acesso com permissões de edição. Utilize [ **obter Widget de Insights** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) ou [ **obter vídeo acesso Token** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) com **& valor de allowEdit = true**. 

## <a name="widgets-interaction"></a>Interação de widgets

O **Insights cognitivos** widget pode interagir com um vídeo na sua aplicação. Esta secção mostra como atingir este interação do utilizador.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Comunicações de várias origens

Para obter widgets do indexador de vídeo para comunicar com outros componentes, o serviço de vídeo indexador faz o seguinte:

- Utiliza a comunicação de várias origens HTML5 método **postMessage** e 
- Valida a mensagem em VideoIndexer.ai origem. 

Se optar por implementar o seu próprio código leitor e efetue a integração com **Insights cognitivos** widgets, é da responsabilidade do cliente para validar a origem da mensagem de VideoIndexer.ai.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Incorporar os dois tipos de widgets na sua aplicação / blogue (recomendado) 

Esta secção mostra como atingir a interação entre duas widgets do indexador de vídeo, pelo que, quando um utilizador clica o controlo de informações da sua aplicação, leitor de passar para o momento relevante.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Copiar o **Player** widget código de incorporação.
2. Copiar o **Insights cognitivos** código de incorporação.
3. Adicionar o [ **ficheiro Mediator** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) para processar a comunicação entre os dois widgets:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Agora quando um utilizador clica o controlo de informações da sua aplicação, leitor de passar para o momento relevante.

Para obter mais informações, consulte [esta demonstração](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Incorporar a miniaplicação Insights cognitivos e utilizar o Azure Media Player para reproduzir o conteúdo

Esta secção mostra como atingir a interação entre um **Insights cognitivos** widget e uma instância de leitor de multimédia do Azure utilizando o [Plug-in do AMP](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Adicione um plug-in do indexador de vídeo para o leitor de AMP.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Instanciar o leitor de multimédia do Azure com o plug-in do indexador de vídeo.

        // Init Source
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // Here is how to load vtt from VI, you can replace it with your vtt url.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plugin
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically
            initSource.call(this);
        });

3. Copiar o **Insights cognitivos** código de incorporação.

Deve ser capaz de agora comunicar com o seu leitor de multimédia do Azure.

Para obter mais informações, consulte [esta demonstração](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Incorporar widget Insights cognitivos do vídeo indexador e utilize a suas próprias leitor (pode ser qualquer player)

Se utilizar o seus próprios player, terá de asseguramos manipular o leitor de si próprio para alcançar a comunicação. 

1. Insira o seu leitor de vídeo.

    Por exemplo, um leitor HTML5 padrão

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Incorpore o widget Insights cognitivos.
3. Implementar comunicações para o leitor, o evento de "mensagem" a escutar. Por exemplo:

        <script>
    
            (function(){
            // Reference your player instance
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Here you need to call your player "jumpTo" implementation
                playerInstance.currentTime = evt.data.time;
               
                // Confirm arrival to us
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to message event
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>


Para obter mais informações, consulte [esta demonstração](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Adicionar subtitles

Se incorporar insights indexador de vídeo com a suas próprias player AMP, pode utilizar o **GetVttUrl** método obter fechadas legendas (subtitles). Também pode chamar um método de javascript a partir do plug-in do vídeo indexador AMP **getSubtitlesUrl** (conforme mostrado anteriormente). 

## <a name="customizing-embeddable-widgets"></a>Personalizar embeddable widgets

### <a name="cognitive-insights-widget"></a>Widget de insights cognitivos
Pode escolher os tipos de informações que pretende especificando-los como um valor para o parâmetro de URL seguinte adicionado o código de incorporação obter (a partir da API ou da aplicação web):

**& widgets =** \<lista de widgets reclamados >

Os valores possíveis são: as pessoas, palavras-chave, sentiments, transcript, pesquisa.

Por exemplo, se pretende incorporar um widget que contém apenas informações pessoas e pesquisa o iframe incorporar o URL terá este aspeto: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people, pesquisa

Também pode ser personalizado o título da janela iframe fornecendo **& título =** <YourTitle> para o url de iframe. (Irá personalizar o html \<título > valor).
Por exemplo, se pretende conceder a janela de iframe o título "MyInsights", o url terá este aspeto: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. Tenha em atenção que esta opção é relevante apenas em casos quando é necessário abrir as informações numa nova janela.

### <a name="player-widget"></a>Widget de leitor
Se incorporar o leitor de vídeo indexador pode escolher o tamanho do leitor de especificando o tamanho do iframe.

Por exemplo:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

Ao leitor de vídeo indexador predefinido será ter automaticamente gerado legendas fechadas com base no transcript do vídeo extraído do vídeo com o idioma de origem que foi selecionado quando o vídeo foi carregado.

Se pretende incorporar com um idioma diferente, pode adicionar **& legendas = < idioma | "all" | "false" >** para o URL de leitor de incorporação ou colocar "tudo" como o valor se pretender ter legendas do todos os idiomas disponíveis.
Se quiser legendas para ser apresentado por predefinição, pode passar **& showCaptions = true**

O URL de incorporação, em seguida, terá este aspeto: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Se pretender desativar legendas pode passar "false" como valor de parâmetro de legendas.

Play automática – por predefinição, que o leitor irá começar a reproduzir o vídeo. Pode optar por não a por transmitir & a reprodução automática ROM = false para o URL de incorporação acima.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como ver e editar insights indexador de vídeo, consulte [isto](video-indexer-view-edit.md) artigo.

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md)
