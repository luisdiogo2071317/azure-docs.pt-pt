---
title: O que é o Video Indexer?
titlesuffix: Azure Media Services
description: Este tópico apresenta uma descrição geral do serviço Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: ae8634fbfdaa250cbabda6189c6c2eeef8e5e4f1
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292376"
---
# <a name="what-is-video-indexer"></a>O que é o Video Indexer?

O Azure Video Indexer é uma aplicação da cloud criada com base na Análise de Multimédia do Azure, no Azure Search e nos Serviços Cognitivos (tais como, a API Face, o Microsoft Translator, a API de Imagem Digitalizada e o Serviço de Voz Personalizada). Esta aplicação permite extrair as informações dos seus vídeos através dos modelos do Video Indexer descritos abaixo:
 
- **Automatic language detection** (Deteção automática do idioma): identifica automaticamente o idioma falado dominante. São suportados, entre outros, os seguintes idiomas: inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, russo e português (Brasil). Será revertido para inglês quando não conseguir detetar o idioma.
- **Audio transcription** (Transcrição de áudio): converte voz em texto em 10 idiomas e permite extensões. São suportados, entre outros, os seguintes idiomas: inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, árabe, russo e português (Brasil).
- **Closed captioning** (Legendas): cria legendas em três formatos: VTT, TTML e SRT.
- **Two channel processing** (Processamento de dois canais): deteta automaticamente, separa e une transcrições numa única linha cronológica.
- **Noise reduction** (Redução de ruído): torna as conversas telefónicas e as gravações ruidosas mais claras (com base em filtros do Skype).
- **Transcript customization (CRIS)** (Personalização da transcrição [CRIS]): prepara e executa modelos de conversão de voz em texto personalizados e expandidos para criar transcrições específicas da indústria.
- **Speaker enumeration** (Enumeração de oradores): mapeia e identifica o orador que proferiu determinadas palavras e o momento em que o fez.
- **Speaker statistics** (Estatísticas dos oradores): fornece estatísticas relativamente às taxas de discurso dos oradores.
- **Visual text recognition (OCR)** (Reconhecimento de texto visual [OCR]): extrai o texto que é visualmente apresentado no vídeo.
- **Keyframe extraction** (Extração de keyframes): deteta keyframes estáveis num vídeo.
- **Sentiment analysis** (Análise de sentimentos): identifica sentimentos positivos, negativos e neutros a partir do discurso e de texto visual.
- **Visual content moderation** (Moderação de conteúdo visual): deteta elementos visuais para adultos.
- **Keywords extraction** (Extração de palavras-chave): extrai palavras-chave a partir do discurso e de texto visual.
- **Labels identification** (Identificação através de etiquetas): identifica as ações e os objetos visuais apresentados.
- **Brands extraction** (Extração de marcas): extrai marcas a partir do discurso e de texto visual.
- **Face detection** (Deteção facial): deteta e agrupa os rostos que aparecem no vídeo.
- **Thumbnail extraction for faces ("best face")** Extração de miniaturas de rostos ["o melhor rosto"]: identifica automaticamente o melhor rosto captado em cada grupo de rostos (com base na qualidade, no tamanho e na posição frontal) e extrai esse rosto sob a forma de recurso de imagem.
- **Identificação de celebridades**: o Video Indexer identifica automaticamente mais de 1 milhão de celebridades – como líderes mundiais, atores e atrizes, atletas, investigadores e líderes de empresas e de grupos tecnológicos em todo o mundo. Os dados sobre estas celebridades também podem ser encontrados em vários sites conhecidos como, por exemplo, o IMDB e a Wikipédia.
- **Identificação facial com base na conta**: o Video Indexer prepara um modelo para uma conta específica. Em seguida, reconhece os rostos no vídeo com base no modelo treinado especificamente para vídeos nessa conta.
- **Textual content moderation** (Moderação de conteúdo textual): deteta texto explícito na transcrição de áudio.
- **Shot detection** (Deteção de cenas): determina os momentos em que há mudanças de cena no vídeo.
- **Black frame detection** (Deteção de fotogramas pretos): identifica os fotogramas pretos apresentados no vídeo.
- **Audio effects** (Efeitos de áudio): identifica os efeitos de áudio, tais como palmas, voz e silêncio.
- **Topic inference** (Inferência de tópicos): faz inferências relativamente aos principais tópicos a partir de transcrições. A taxonomia do [IPTC](https://iptc.org/standards/media-topics/) de 1.º nível está incluída.
- **Emotion detection** (Deteção de emoções): identifica emoções com base em estímulos de voz e áudio. As emoções podem ser alegria, tristeza, raiva ou medo.
- **Artifacts** (Artefactos): extrai um conjunto avançado de artefactos mais detalhados para cada um dos modelos.
- **Translation** (Tradução): cria traduções da transcrição de áudio para 54 idiomas diferentes.

Assim que o Video Indexer terminar o processamento e a análise, pode rever, organizar, pesquisar e publicar as informações do vídeo.

Se a sua função é ser gestor de conteúdos ou programador, o serviço Video Indexer pode atender as suas necessidades. Os gestores de conteúdo podem utilizar o portal Web do Video Indexer para consumirem o serviço, sem terem de escrever uma única linha de código. Veja [Introdução ao site do Video Indexer](video-indexer-get-started.md). Os programadores podem tirar partido das APIs para processar conteúdo em escala, veja [Utilizar a API REST do Video Indexer](video-indexer-use-apis.md). O serviço também permite aos clientes utilizarem widgets para publicarem transmissões de vídeo e informações extraídas nas suas próprias aplicações, veja [Incorporar widgets visuais na sua aplicação](video-indexer-embed-widgets.md).

Pode inscrever-se no serviço com a sua conta existente do AAD, LinkedIn, Facebook, Google ou MSA. Para obter mais informações, veja [introdução](video-indexer-get-started.md).

## <a name="scenarios"></a>Cenários

Seguem-se alguns cenários em que o Video Indexer pode ser útil

- Pesquisa – as informações extraídas de vídeo podem servir para melhorar a experiência de pesquisa numa biblioteca de vídeos. Por exemplo, a indexação de palavras faladas e rostos pode ativar a experiência de pesquisa de encontrar momentos num vídeo em que uma determinada pessoa falou determinadas palavras ou quando duas pessoas foram vistas juntas. A pesquisa com base nessas informações dos vídeos é aplicável a agências de notícias, institutos educacionais, emissoras, proprietários de conteúdo de entretenimento, aplicações LOB empresariais e, em geral, a qualquer setor que tenha uma biblioteca de vídeos que os utilizadores precisam para pesquisar.

- Monetização – o Video Indexer pode ajudar a melhorar o valor dos vídeos. Por exemplo, os setores que se baseiam na receita de anúncios (por exemplo, imprensa, redes sociais, etc.), podem apresentar anúncios mais relevantes através das informações extraídas como sinais adicionais para o servidor do anúncio (apresentar um anúncio de calçado de desporto é mais relevante no meio de um jogo de futebol do que numa competição de natação).

- Cativação – as informações de vídeo podem ser utilizadas para melhorar a cativação ao posicionar os momentos de vídeo relevantes para os utilizadores. Por exemplo, considere um vídeo educacional que fala sobre esferas durante os primeiros 30 minutos e pirâmides nos 30 minutos seguintes. Um estudante que lê sobre pirâmides iria beneficiar mais se o vídeo estivesse posicionado a partir do marcador dos 30 minutos.

Para obter mais informações, veja este [blogue](https://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Passos Seguintes

Está pronto para começar a utilizar o Video Indexer. Para obter mais informações, veja os artigos seguintes:

- [Introdução ao site do Video Indexer](video-indexer-get-started.md)
- [Processar conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Incorporar widgets visuais na sua aplicação](video-indexer-embed-widgets.md)
