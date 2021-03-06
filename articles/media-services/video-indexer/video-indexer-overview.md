---
title: O que é o Video Indexer?
titlesuffix: Azure Media Services
description: Este tópico apresenta uma descrição geral do serviço Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/17/2019
ms.author: juliako
ms.openlocfilehash: c1c81ba0f168759ab9cc05f3441e07ff03275e84
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417385"
---
# <a name="what-is-video-indexer"></a>O que é o Video Indexer?

O Azure Video Indexer é uma aplicação da cloud criada com base na Análise de Multimédia do Azure, no Azure Search e nos Serviços Cognitivos (tais como, a API Face, o Microsoft Translator, a API de Imagem Digitalizada e o Serviço de Voz Personalizada). Permite-lhe extrair as informações dos seus vídeos com modelos de áudio e vídeos do Video Indexer descritos abaixo:
  
## <a name="video-insights"></a>Informações de vídeo

- **Deteção facial**: Detecta e grupos de rostos que aparecem no vídeo.
- **Identificação de celebridade**: O Video Indexer identifica automaticamente celebridades de mais de 1 milhão – como líderes mundiais, atores e actresses, atletas Favoritos, pesquisadores, empresas e líderes de tecnologia em todo o mundo. Os dados sobre estas celebridades também podem ser encontrados em vários sites conhecidos como, por exemplo, o IMDB e a Wikipédia.
- **Identificação facial com base na conta**: O Video Indexer prepara um modelo para uma conta específica. Em seguida, reconhece os rostos no vídeo com base no modelo treinado especificamente para vídeos nessa conta.
- **Extração de miniatura do rostos** ("melhor face"): Identifica a melhor face capturada em cada grupo de faces (com base na qualidade, o tamanho e posição frontal) e extraí-lo como um recurso de imagem automaticamente.
- **Reconhecimento de texto visual** (OCR): Extrai o texto que visualmente é apresentado no vídeo.
- **Moderação de conteúdos Visual**: Deteta elementos visuais para adultos e/ou para adultos.
- **Etiquetas de identificação**: Identifica os objetos visuais e ações apresentadas.
- **Captura de deteção**: Determina quando uma cena é alterado no vídeo.
- **Marcar a negrito de deteção de quadro**: Identifica os quadros pretas apresentados no vídeo.
- **Extração de quadro-chave**: Deteta keyframes estável num vídeo.

## <a name="audio-insights"></a>Informações de áudio

- **Deteção de idioma automática**: Identifica automaticamente o idioma falado dominante. São suportados, entre outros, os seguintes idiomas: inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, russo e português (Brasil). Será revertido para inglês quando não conseguir detetar o idioma.
- **Transcrição de áudio**: Converte voz em texto 12 idiomas e permite que as extensões. Idiomas suportados incluem o inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, árabe, russo, português (Brasil), Híndi e coreano.
- **Legendagem de áudio**: Cria fechado as legendas de áudio em três formatos: VTT, TTML, SRT.
- **Dois channel processamento**: Deteta automaticamente, separe transcrição e mesclagens para uma única linha do tempo.
- **Redução de ruído**: Vai limpar gravações de áudio ou desnecessárias de telefonia (com base nos filtros do Skype).
- **Personalização de transcrição** (CRIS): Prepara e executa a conversão de voz personalizada expandida para modelos de texto para criar transcrições específicos da indústria.
- **Enumeração de orador**: Mapeia e compreende que orador spoke que palavras e quando.
- **Estatísticas de orador**: Apresenta estatísticas para proporções de voz de oradores.
- **Moderação de conteúdo textual**: Deteta texto explícito na transcrição áudio.
- **Efeitos de áudio**: Identifica os efeitos de áudio como claps mão, voz e silêncio.
- **Deteção de emoções**: Identifica emoções com base nas ajudas de voz e áudio. As emoções podem ser alegria, tristeza, raiva ou medo.
- **Tradução**: Cria as traduções de transcrição áudio a 54 diferentes idiomas.

## <a name="audio-and-video-insights-multi-channels"></a>Informações de áudio e vídeo (vários canais)

Quando indexação pelo resultado parcial de um canal para esses modelos estará disponível

- **Extração de palavras-chave**: Extrai as palavras-chave de voz e texto visual.
- **Marcas extração**: Extrai as marcas de voz e texto visual.
- **Inferência de tipos de tópico**: Faz a inferência de tipos de tópicos principais de transcrições. A taxonomia IPTC ao nível do dia 1 está incluída.
- **Artefactos**: Extrai o conjunto avançado de "próximo nível de detalhes" artefactos para cada um dos modelos.
- **Análise de sentimentos**: Identifica positivos, negativos e neutros sentimentos de voz e texto visual.
 
  
 
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
