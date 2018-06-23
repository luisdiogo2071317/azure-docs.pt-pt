---
title: Introdução ao Azure Moderator conteúdo | Microsoft Docs
description: Como começar com Azure Moderator de conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: ae4333047ebd95733c7baaed0323a0c2c477d323
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352567"
---
# <a name="get-started-with-content-moderator"></a>Introdução ao Content Moderator

Começar com o conteúdo Moderator APIs e a ferramenta de revisão das seguintes formas:

- [Começar a utilizar a ferramenta de revisão](#start-with-the-review-tool) para criar as chaves de API e uma equipa de revisão. Explore a ferramenta de revisão e saiba como integrar com as APIs de Moderator conteúdo.
- [Subscrever Moderator conteúdo](#start-with-the-apis) no portal do Azure. Ainda tem de inscrever-se online para criar um agrupamento de revisão.
- [Utilizar o conector de fluxo e modelos](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) verificar uma vasta gama de integrações com um designer de fácil utilização.

Independentemente da opção que escolher, consulte o [gerir credenciais](review-tool-user-guide/credentials.md) artigo para localizar as credenciais da sua API.

## <a name="start-with-the-review-tool"></a>Começar a utilizar a ferramenta de revisão
[Inscrever-se](http://contentmoderator.cognitive.microsoft.com/) no conteúdo Moderator revisão ferramenta web site.

![Página inicial de Moderator de conteúdo](images/homepage.PNG)

### <a name="create-a-review-team"></a>Criar um agrupamento de revisão
Dê um nome de sua equipa. Se pretender convidar os seus colegas, pode fazê-lo, introduzindo os respetivos endereços de e-mail.

![Convidar membro do agrupamento](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Carregar imagens ou introduza o texto
Clique em **tente > imagem** ou **tente > texto**. Carregar imagens de exemplo até cinco ou introduza o texto de exemplo para moderação interrupção.

![Tente moderação de interrupção de texto ou de imagem](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Submeter para moderação de interrupção automatizada
Submeta o seu conteúdo para a moderação de interrupção automatizada. Internamente, a ferramenta de revisão chama a moderação de interrupção APIs para analisar o conteúdo. Assim que a análise estiver concluída, verá uma mensagem a informar sobre os resultados a aguardar a revisão.

![Ficheiros moderados](images/submitted.png)

### <a name="review-and-confirm-results"></a>Reveja e confirme os resultados
Reveja as etiquetas de auto-moderated, alterar se for necessário e submeter utilizando o **seguinte** botão. Como a aplicação de negócio chamadas das APIs do Moderator, a começa a conteúdo marcada colocação cópias de segurança, pronto para ser revisto pelas equipas de revisão humanos. Reveja rapidamente grandes volumes de conteúdo através desta abordagem.

![Resultados da revisão](images/reviewresults.png)

Saiba como utilizar todos os o [reveja funcionalidades da ferramenta](Review-Tool-User-Guide/human-in-the-loop.md) ou continuar com a secção seguinte para saber mais sobre as APIs. Ignorar o passo de inscrição, porque tem a chave de API aprovisionada para si na ferramenta de revisão, conforme mostrado no [gerir credenciais](review-tool-user-guide/credentials.md) artigo.

### <a name="use-the-apis"></a>Utilizar as APIs

Agora que já explorou a moderação de interrupção de conteúdo e reveja a experiência de ferramenta, saiba como integrar o conteúdo Moderator com as suas aplicações empresariais. Utilize a secção seguinte para saber mais e a compreensão com os SDKs e amostras de controlar fast.

## <a name="start-with-the-apis"></a>Começar a utilizar as APIs

[Subscrever Moderator conteúdo](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) no portal do Azure. Começar com uma das APIs do seguinte:

### <a name="image-moderation"></a>Moderação de imagens

Começar a utilizar o [consola API](try-image-api.md) ou utilize o [início rápido do .NET](image-moderation-quickstart-dotnet.md) analisar as imagens e detetar potencial conteúdo para adultos e racy utilizando etiquetas, pontuações de confiança e outras extraídos informações.

### <a name="text-moderation"></a>Moderação de texto

Começar a utilizar o [consola API](try-text-api.md) ou utilize o [início rápido do .NET](text-moderation-quickstart-dotnet.md) para analisar o conteúdo de texto para potencial profanity, classificação assistida por máquina indesejável texto (pré-visualização) e identificação pessoal informações (PII). 


### <a name="video-moderation"></a>Moderação de vídeos

Começar a utilizar o [início rápido do .NET](video-moderation-api.md) analisar vídeos e detetar potencial conteúdo para adultos e racy. 


### <a name="review-apis"></a>APIs de revisão

Comece por aqui, seleccionando a tarefa, reveja e APIs de fluxo de trabalho.

- O [API de tarefa](try-review-api-job.md) analisa o conteúdo utilizando a moderação de interrupção APIs e gera revisões na ferramenta de revisão. 
- O [revisão API](try-review-api-review.md) diretamente cria imagem, texto ou revisões de vídeos para moderators humanos sem primeiro analisar o conteúdo. 
- O [API de fluxo de trabalho](try-review-api-workflow.md) cria, atualizações e obtém os detalhes sobre os fluxos de trabalho personalizados que cria a sua equipa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a moderação de interrupção de conteúdo a partir de [imagem moderação de interrupção API](image-moderation-api.md).
