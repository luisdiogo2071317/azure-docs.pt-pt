---
title: 'Início rápido: Introdução ao Content Moderator'
titlesuffix: Azure Cognitive Services
description: Como começar com o Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: sajagtap
ms.openlocfilehash: 79fe761bc6d8bb9561701fd11b06b010bf3454f6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260011"
---
# <a name="quickstart-get-familiar-with-content-moderator"></a>Início rápido: Se familiarizar com o Content Moderator

Neste início rápido, irá utilizar a ferramenta de revisão do moderador conteúdo online para testar a funcionalidade básica do Content Moderator sem ter de escrever qualquer código. Se pretender integrar este serviço na sua aplicação mais rapidamente, consulte os outros guias de introdução a [próximos passos](#next-steps) secção.

## <a name="prerequisites"></a>Pré-requisitos

- Um navegador da web

## <a name="set-up-the-review-tool"></a>Configurar a ferramenta de revisão
A ferramenta de revisão de moderador conteúdo é uma ferramenta baseada na web que permite que os revisores humanos auxiliar o serviço cognitivo na tomada de decisões. Neste guia, irá percorrer o processo de curto de configurar a ferramenta de revisão para que pode ver como funciona o serviço do Content Moderator. Vá para o [ferramenta de revisão do conteúdo moderador](https://contentmoderator.cognitive.microsoft.com/) do site e inscreva-se.

![Página inicial do moderador de conteúdo](images/homepage.PNG)

## <a name="create-a-review-team"></a>Criar uma equipa de revisão

Em seguida, crie uma equipa de revisão. Num cenário de trabalho, este será o grupo de pessoas que analisará manualmente as decisões de moderação do serviço. Por enquanto, basta criar um nome de equipa. Se desejar convidar colegas para a equipe, pode fazê-lo ao introduzir aqui os respetivos endereços de e-mail.

![Convidar Membro da Equipe](images/QuickStart-2-small.png)

## <a name="upload-sample-content"></a>Carregar conteúdo de exemplo

Agora, está pronto para carregar o conteúdo de exemplo. Selecione **tente > imagem**, **tente > texto**, ou **tente > vídeo**.

![Experimente o imagem ou de moderação de texto](images/tryimagesortext.png)

Submeta o seu conteúdo para moderação. Internamente, a ferramenta de revisão irá chamar as APIs para analisar o conteúdo de moderação. Quando a análise estiver concluída, verá uma mensagem a informar que há resultados aguardar sua revisão.

![Ficheiros moderados](images/submitted.png)

## <a name="review-moderation-tags"></a>Etiquetas de moderação de revisão

Reveja as etiquetas aplicadas moderação. Pode ver as etiquetas foram aplicadas ao seu conteúdo e o que estava a classificação em cada categoria. Consulte a [imagem](image-moderation-api.md), [texto](text-moderation-api.md), e [vídeo](video-moderation-api.md) indicam de tópicos de moderação para saber mais sobre o que o conteúdo diferente com as etiquetas.

![Resultados da revisão](images/reviewresults_text.png)

Num projeto, ou sua equipa de revisão pode alterar essas marcas ou adicionar mais etiquetas conforme necessário. Vai enviar essas alterações com o **seguinte** botão. Como seu aplicativo de negócios chamadas a APIs de moderador, o conteúdo marcado irá enfileirar aqui, está preparado para ser examinados pelas equipes de revisão humana. Pode analisar rapidamente grandes volumes de conteúdo usando essa abordagem.

Neste momento, utilizou a ferramenta de revisão de moderador conteúdo para ver exemplos do que o serviço do Content Moderator pode fazer. Em seguida, pode optar por saber mais sobre a ferramenta de revisão e como integrá-lo num projeto de software usando as APIs de revisão ou, pode avançar para o [próximos passos](#next-steps) secção para saber como utilizar as APIs de moderação próprios na sua aplicação.

## <a name="learn-more-about-the-review-tool"></a>Saiba mais sobre a ferramenta de revisão

Para saber mais sobre como utilizar a ferramenta de revisão de moderador conteúdo, veja a [humanos no loop](Review-Tool-User-Guide/human-in-the-loop.md) orientar e veja as APIs de ferramenta de revisão para aprender a otimizar a experiência de revisão humana:
- O [API de tarefa](try-review-api-job.md) analisa o conteúdo ao utilizar as APIs de moderação e gera as revisões na ferramenta de revisão. 
- O [revisão de API](try-review-api-review.md) cria diretamente as revisões de vídeo, texto ou imagem de moderadores humanos sem primeiro a analisar o conteúdo. 
- O [API do fluxo de trabalho](try-review-api-workflow.md) cria, atualiza e obtém os detalhes sobre os fluxos de trabalho personalizados que cria a sua equipa.

Em alternativa, continue com os passos seguintes para começar a utilizar as APIs de moderação em seu código.

## <a name="next-steps"></a>Passos Seguintes

Saiba como utilizar as APIs de moderação próprios na sua aplicação.
- Implementar a moderação de imagens. Utilize o [consola de API](try-image-api.md) ou o [ C# início rápido](image-moderation-quickstart-dotnet.md) para procurar imagens e detetar potenciais conteúdos para adultos com etiquetas, as pontuações de confiança e outras extraiu informações.
- Implementar a moderação de texto. Utilizar o [consola de API](try-text-api.md) ou utilizar o [ C# início rápido](text-moderation-quickstart-dotnet.md) para analisar o conteúdo de texto para potencial linguagem inapropriada, classificação de texto indesejado assistida (pré-visualização) e de identificação pessoal informações (PII). 
- Implementar a moderação de vídeo. Siga os [guia de procedimentos de moderação de vídeo para o C# ](video-moderation-api.md) para procurar vídeos e detetar potenciais conteúdos para adultos. 
