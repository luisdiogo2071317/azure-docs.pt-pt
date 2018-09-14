---
title: Incorporar widgets do indexador de vídeo do Azure nas suas aplicações | Documentos da Microsoft
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: 6ee87b6d7c89745472bf6000de66f682cbf2cca9
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45544120"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Incorporar widgets do indexador de vídeos nas suas aplicações

Este artigo mostra como pode incorporar widgets do indexador de vídeos nas suas aplicações. O Video Indexer suporta incorporação dois tipos de widgets na sua aplicação: **Insights cognitivos** e **Player**. 
## <a name="widget-types"></a>Tipos de widget

### <a name="cognitive-insights-widget"></a>Cognitivo widget de Insights

R **Insights cognitivos** widget inclui todas as informações de visual que foram extraídas do seu processo de indexação de vídeo. O widget insights suporta os seguintes parâmetros de URL opcionais:

|Nome|Definição|Descrição|
|---|---|---|
|widgets|Cadeias de caracteres separadas por vírgulas|Permite-lhe controlar as informações que desejar. <br/>Exemplo: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search` processará apenas as pessoas e marcas insights de interface do Usuário<br/>As opções disponíveis: as pessoas, palavras-chave, anotações, marcas, sentimentos, transcrição, pesquisa.<br/>não suportado através de URL, versão = 2<br/><br/>**Nota:** a **widgets** param de URL não é suportada se **versão = 2** é utilizado. |
|versão|Versões dos **Insights cognitivos** widget|Para obter as informações mais recentes atualizações de widget, adicionar `?version=2` param para o url de incorporação de consulta. Por exemplo, `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?version=2` <br/> Para obter a versão mais antiga, basta remover o `version=2` da URL.

### <a name="player-widget"></a>Widget do leitor

R **Player** widget permite-lhe transmitir o vídeo com a taxa de bits adaptativa. O widget do leitor suporta os seguintes parâmetros de URL opcionais:

|Nome|Definição|Descrição|
|---|---|---|
|T|Segundos do início|Faz com que o início de player reproduzir a partir do momento em determinado ponto.<br/>Exemplo: t = 60|
|legendas|Código de idioma|Obtém a legenda no idioma especificado durante o widget para estar disponível no menu de legendas a carregar.<br/>Exemplo: a legendagem de áudio = en-US|
|showCaptions|Um valor booleano|Faz com que o jogador de carga com as legendas já ativadas.<br/>Exemplo: showCaptions = true|
|tipo||Ativa uma capa de leitor de áudio (parte do vídeo é removido).<br/>Exemplo: escreva = áudio|
|reprodução automática|Um valor booleano|Indica se o jogador deve começar a reproduzir o vídeo quando carregado (a predefinição é verdadeira).<br/>Exemplo: autoplay = false|
|Idioma|Código de idioma|Controla o idioma de player (a predefinição é en-US)<br/>Exemplo: linguagem = de-DE|

## <a name="embedding-public-content"></a>Incorporar conteúdo público

1. Navegue para o [Video Indexer](https://www.videoindexer.ai/) Web site e o início de sessão.
2. Clique no botão "incorporar" que aparece abaixo do vídeo.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Depois de clicar no botão, um modal de incorporação será apresentado no ecrã onde pode escolher quais widget que pretende incorporar na sua aplicação.
    Selecionar um widget (**Player** ou **Insights cognitivos**), gera o código incorporado que pode colar na sua aplicação.
 
3. Escolha o tipo de widget desejar (**Insights cognitivos** ou **Player**).
4. Copie o código de incorporação e adicionar à sua aplicação. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Incorporar conteúdo privado

Pode obter incorporar códigos de incorporação pop-ups (conforme mostrado na secção anterior) para **público** apenas a vídeos. 

Se quiser incorporar uma **privada** vídeo, precisa passar um token de acesso a **iframe**do **src** atributo:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>
    
Utilizar o [ **obter o Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API para obter o conteúdo do widget Insights cognitivos ou utilize [ **obter vídeo Token de acesso** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) e adicioná-la como um Consulte param para o URL, conforme mostrado acima. Especifique este URL como o **iframe**da **src** valor.

Se quiser fornecer informações capacidades de edição (como temos em nosso aplicativo web) no seu widget embedded, terá de passar um token de acesso com permissões de edição. Uso [ **obter o Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) ou [ **obter vídeo acesso Token** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) com **& pokud vlastnost AllowEdit má = true**. 

## <a name="widgets-interaction"></a>Interação de widgets

O **Insights cognitivos** widget pode interagir com um vídeo no seu aplicativo. Esta secção mostra como conseguir essa interação.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Comunicações de várias origens

Para obter os widgets do indexador de vídeo para comunicar com outros componentes, o serviço do Video Indexer faz o seguinte:

- Utiliza a comunicação de várias origens método HTML5 **postMessage** e 
- Valida a mensagem em VideoIndexer.ai origem. 

Se optar por implementar seu próprio código de player e fazer a integração com o **Insights cognitivos** widgets, é sua responsabilidade para validar a origem da mensagem que provém de VideoIndexer.ai.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Incorporar os dois tipos de widgets na sua aplicação / blog (recomendado) 

Esta secção mostra como conseguir a interação entre dois widgets do Video Indexer isso quando um usuário clica o controlo de informações da sua aplicação, o jogador salta para um momento relevante.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Copiar o **Player** código de incorporação de widget.
2. Copiar o **Insights cognitivos** código de incorporação.
3. Adicionar a [ **ficheiro de Mediador** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) para lidar com a comunicação entre os dois widgets:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Agora quando um usuário clica o controlo de informações da sua aplicação, o jogador salta para um momento relevante.

Para obter mais informações, consulte [esta demonstração](https://codepen.io/videoindexer/pen/NzJeOb).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Incorporar o widget Insights cognitivos e utilize o leitor de multimédia do Azure para reproduzir o conteúdo

Esta secção mostra como conseguir a interação entre uma **Insights cognitivos** widget e uma instância de leitor de multimédia do Azure com o [Plug-in do AMP](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Adicione um plug-in do indexador de vídeo para o jogador AMP.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Criar uma instância de leitor de multimédia do Azure com o plug-in do Video Indexer.

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

Deve ser agora capaz de comunicar com o leitor de multimédia do Azure.

Para obter mais informações, consulte [esta demonstração](https://codepen.io/videoindexer/pen/rYONrO).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Incorpore o widget de informações de cognitivos do indexador de vídeo e utilize o seu leitor (pode ser qualquer leitor)

Se usar seu próprio player, tem que cuidar de manipulação de seu player-se de forma a alcançar a comunicação. 

1. Insira seu player de vídeo.

    Por exemplo, um leitor HTML5 standard

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Incorpore o widget Insights cognitivos.
3. Implementar a comunicação para seu player através da escuta para o evento de "message". Por exemplo:

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


Para obter mais informações, consulte [esta demonstração](https://codepen.io/videoindexer/pen/YEyPLd).

## <a name="adding-subtitles"></a>Adicionar legendas

Se incorporar informações do indexador de vídeo com o seu próprio player AMP, pode utilizar o **GetVttUrl** método para obter legendas de áudio (legendas). Também é possível chamar um método de javascript do plug-in de AMP do indexador de vídeo **getSubtitlesUrl** (como mostrado anteriormente). 

## <a name="customizing-embeddable-widgets"></a>Personalização a widgets incorporável

### <a name="cognitive-insights-widget"></a>Widget de cognitivos insights
Pode escolher os tipos de informações que pretende, especificando-as como um valor para o parâmetro de URL seguinte adicionado para o código de incorporação que obtenha (a partir da API ou da aplicação web):

**& widgets =** \<lista de widgets reclamados >

Os valores possíveis são: as pessoas, palavras-chave, sentimentos, transcrição, pesquisa.

Por exemplo, se quiser incorporar um widget que contém apenas informações de pessoas e pesquisa de URL de incorporação de iframe ficará assim: https://www.videoindexer.ai/embed/insights/ <accountId> / <videoId>/? widgets = pessoas, pesquisa

O título da janela do iframe também pode ser personalizado, fornecendo **& title =** <YourTitle> para o URL do iframe. (Ele irá personalizar o html \<title > valor).
Por exemplo, se quiser fornecer sua janela de iframe o título "MyInsights", o URL ficará assim: https://www.videoindexer.ai/embed/insights/ <accountId> / <videoId>/? title = MyInsights. Tenha em atenção que esta opção é relevante apenas em casos, quando precisa abrir as informações numa nova janela.

### <a name="player-widget"></a>Widget do leitor
Se incorporar o player de indexador de vídeo, pode escolher o tamanho do jogador, especificando o tamanho do iframe.

Por exemplo:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />

Por player do Video Indexer padrão será ter gerado automaticamente legendas de áudio com base na transcrição do vídeo que foram extraída do vídeo com a linguagem de código que foi selecionado quando o vídeo foi carregado.

Se quiser incorporar com um idioma diferente, pode adicionar **& legendas = < idioma | "all" | "false" >** para o URL de leitor de incorporação ou put "todos" como o valor, se quiser ter todas as legendas de idiomas disponíveis.
Se pretender que as legendas para ser apresentado por predefinição, pode passar **& showCaptions = true**

O URL de incorporação, em seguida, terá o seguinte aspeto: https://www.videoindexer.ai/embed/player/ <accountId> / <videoId>/? legendas = italiano. Se pretender desativar legendas, pode passar "false" como valor de parâmetro de legendas de áudio.

Reprodução automática – por predefinição, que o jogador irá começar a reproduzir o vídeo. Pode optar por não ao passando & autoplay = false para o URL de incorporação acima.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como ver e editar informações do indexador de vídeo, consulte [isso](video-indexer-view-edit.md) artigo.

Além disso, confira [codepen do indexador de vídeo](https://codepen.io/videoindexer/pen/eGxebZ).
