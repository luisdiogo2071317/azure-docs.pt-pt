---
title: Descrição geral do Video Indexer do Azure | Microsoft Docs
description: Este tópico apresenta uma descrição geral do serviço Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 07/25/2018
ms.author: nolachar
ms.openlocfilehash: f52c4af29d0c7de8b5edbe869640ffc5dddb5c5e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397896"
---
# <a name="what-is-video-indexer-preview"></a>O que é o Video Indexer? (pré-visualização)

O Video Indexer é uma aplicação da cloud criada através da Análise de Multimédia do Azure, dos Serviços Cognitivos (por exemplo a API Face, o Microsoft Translator, a API de Imagem Digitalizada e o Serviço de Voz Personalizada) e do Azure Search. Permite-lhe extrair as seguintes informações dos seus vídeos com tecnologias de inteligência artificial:

- **Deteção automática do idioma**: o Video Indexer consegue detetar automaticamente o idioma do vídeo. A deteção automática do idioma suporta atualmente inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês e russo. Será revertido para inglês quando não conseguir detetar o idioma.
- **Transcrição de áudio**: o Video Indexer tem uma funcionalidade de conversão de voz em texto, que permite aos clientes obterem uma transcrição das palavras faladas. Os idiomas suportados incluem inglês, espanhol, francês, alemão, italiano, chinês (simplificado), português (Brasil), japonês e russo (com muitos mais por vir no futuro). 
- **Controlo e a identificação de rostos**: as tecnologias de rosto ativam a deteção de rostos num vídeo. Os rostos detetados são comparados com uma base de dados de celebridades para avaliar as celebridades que estão presentes no vídeo. Os clientes também podem identificar rostos que não correspondem a uma celebridade. O Video Indexer cria um modelo de rosto com base nessas etiquetas e pode reconhecer esses rostos nos vídeos submetidos no futuro.
- **Indexação do orador**: o Video Indexer tem a capacidade de mapear e compreender qual foi o orador que falou determinadas palavras e quando.
- **Reconhecimento de texto visual**: com esta tecnologia, o serviço do Video Indexer extrai o texto apresentado nos vídeos.  
- **Deteção de atividade de voz**: a deteção permite que o Video Indexer separe o ruído de fundo e a atividade da voz. 
- **Deteção de cena**: o Video Indexer tem a capacidade de executar uma análise visual no vídeo para determinar quando uma cena é alterada num vídeo.
- **Extração de keyframe**: o Video Indexer deteta automaticamente os keyframes num vídeo. 
- **Análise de sentimentos**: o Video Indexer realiza a análise de sentimentos no texto extraído através da conversão de voz em texto e do reconhecimento ótico de carateres, e apresenta essas informações na forma de sentimentos positivos, negativos ou neutros, juntamente com códigos de tempo.
- **Tradução**: o Video Indexer tem a capacidade de traduzir a transcrição de áudio de um idioma para outro. São suportados os seguintes idiomas: inglês, espanhol, francês, alemão, italiano, chinês simplificado, português do Brasil, japonês e russo. Depois de estar traduzido, o utilizador pode inclusive obter o texto de legenda no leitor de vídeo noutros idiomas.
- **Moderação de conteúdo visual**: esta tecnologia permite a deteção de material para adultos e/ou ousado presente no vídeo e pode servir para filtragem de conteúdo. 
- **Extração de palavras-chave**: o Video Indexer extrai as palavras-chave com base na transcrição das palavras faladas e do texto reconhecido pelo reconhecedor de texto visual.
- **Etiquetas**: o Video Indexer apresenta etiquetas para objetos visuais, como um gato, um cão, uma mesa, um carro, bem como para ações como estar em pé, a correr ou a voar.
- **Marcas**: o Video Indexer extrai as marcas de empresas com base na transcrição das palavras faladas e do texto reconhecido pelo reconhecedor de texto visual.

Assim que o Video Indexer terminar o processamento e a análise, pode rever, organizar, pesquisar e publicar as informações do vídeo.

Se a sua função é ser gestor de conteúdos ou programador, o serviço Video Indexer pode atender as suas necessidades. Os gestores de conteúdo podem utilizar o portal Web do Video Indexer para consumirem o serviço, sem terem de escrever uma única linha de código, veja [Introdução à utilização do portal do Video Indexer](video-indexer-get-started.md). Os programadores podem tirar partido das APIs para processar conteúdo em escala, veja [Utilizar a API REST do Video Indexer](video-indexer-use-apis.md). O serviço também permite aos clientes utilizarem widgets para publicarem transmissões de vídeo e informações extraídas nas suas próprias aplicações, veja [Incorporar widgets visuais na sua aplicação](video-indexer-embed-widgets.md).

Pode inscrever-se no serviço com a sua conta existente do AAD, LinkedIn, Facebook, Google ou MSA. Para obter mais informações, veja [introdução](video-indexer-get-started.md).

## <a name="scenarios"></a>Cenários

Seguem-se alguns cenários em que o Video Indexer pode ser útil

- Pesquisa – as informações extraídas de vídeo podem servir para melhorar a experiência de pesquisa numa biblioteca de vídeos. Por exemplo, a indexação de palavras faladas e rostos pode ativar a experiência de pesquisa de encontrar momentos num vídeo em que uma determinada pessoa falou determinadas palavras ou quando duas pessoas foram vistas juntas. A pesquisa com base nessas informações dos vídeos é aplicável a agências de notícias, institutos educacionais, emissoras, proprietários de conteúdo de entretenimento, aplicações LOB empresariais e, em geral, a qualquer setor que tenha uma biblioteca de vídeos que os utilizadores precisam para pesquisar.

- Monetização – o Video Indexer pode ajudar a melhorar o valor dos vídeos. Por exemplo, os setores que se baseiam na receita de anúncios (por exemplo, imprensa, redes sociais, etc.), podem apresentar anúncios mais relevantes através das informações extraídas como sinais adicionais para o servidor do anúncio (apresentar um anúncio de calçado de desporto é mais relevante no meio de um jogo de futebol do que numa competição de natação).

- Cativação – as informações de vídeo podem ser utilizadas para melhorar a cativação ao posicionar os momentos de vídeo relevantes para os utilizadores. Por exemplo, considere um vídeo educacional que fala sobre esferas durante os primeiros 30 minutos e pirâmides nos 30 minutos seguintes. Um estudante que lê sobre pirâmides iria beneficiar mais se o vídeo estivesse posicionado a partir do marcador dos 30 minutos.

Para obter mais informações, veja este [blogue](http://aka.ms/videoindexerblog).

## <a name="next-steps"></a>Passos seguintes

Está pronto para começar a utilizar o Video Indexer. Para obter mais informações, veja os artigos seguintes:

- [Introdução ao portal do Video Indexer](video-indexer-get-started.md)
- [Processar conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Incorporar widgets visuais na sua aplicação](video-indexer-embed-widgets.md)
