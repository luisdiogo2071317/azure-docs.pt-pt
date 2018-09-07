---
title: Introdução ao Azure Content Moderator | Documentos da Microsoft
description: Como começar com Azure Content Moderator.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: 39727b4d97ade67b854fe525afad565451cc3d77
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024516"
---
# <a name="get-started-with-content-moderator"></a>Introdução ao Content Moderator

Introdução ao Content moderator das seguintes formas:

- [Começar com a ferramenta de revisão](#start-with-the-review-tool) para obter a chave de API e criar uma equipa de revisão. A vantagem é que pode utilizar a chave de API para chamar as APIs de moderação de conteúdo e as APIs de revisão de verificação para gerar as revisões, sem passos adicionais.
- [Subscrever o Content Moderator](#start-with-the-apis) no Azure para obter a chave de API. Veja a [referência da API](api-reference.md) e o [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Ainda tem de inscrever-se online para criar uma equipa de revisão.
- [Utilizar o conector de fluxo e os modelos](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) verificar uma vasta gama de integrações com um designer de fácil de usar.

Independentemente da opção que escolher, consulte a [gerir credenciais](review-tool-user-guide/credentials.md) artigo para encontrar as credenciais da API.

## <a name="start-with-the-review-tool"></a>Começar com a ferramenta de revisão
[Inscreva-se](http://contentmoderator.cognitive.microsoft.com/) no site de web para ferramenta de revisão do Content Moderator.

![Página inicial do moderador de conteúdo](images/homepage.PNG)

### <a name="create-a-review-team"></a>Criar uma equipa de revisão
Dê um nome à sua equipa. Se quiser Convide os seus colegas, pode fazê-lo ao introduzir os respetivos endereços de e-mail.

![Convidar Membro da Equipe](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Carregar imagens ou introduza o texto
Clique em **tente > imagem** ou **tente > texto**. Carregar até cinco imagens de exemplo ou introduza o texto de exemplo para moderação.

![Experimente o imagem ou de moderação de texto](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Submeter para moderação automática
Submeta o seu conteúdo para moderação automática. A ferramenta de revisão chama internamente, as APIs para analisar o conteúdo de moderação. Quando a análise estiver concluída, verá uma mensagem a informar sobre os resultados a aguardar para revisão.

![Ficheiros moderados](images/submitted.png)

### <a name="review-and-confirm-results"></a>Rever e confirmar os resultados
Reveja as etiquetas de auto-moderado, altere se necessário e submeter utilizando o **seguinte** botão. Como seu aplicativo de negócios chamadas das APIs de moderador, iniciada a conteúdo marcada enfileirado, está preparado para ser examinados pelas equipes de revisão humana. Analisar rapidamente grandes volumes de conteúdo usando essa abordagem.

![Resultados da revisão](images/reviewresults.png)

Saiba como utilizar todos os [revisar recursos da ferramenta](Review-Tool-User-Guide/human-in-the-loop.md) ou continuar com a secção seguinte para saber mais sobre as APIs. Ignore o passo de inscrição, porque tem a chave de API provisionada para na ferramenta de revisão, como mostra a [gerir credenciais](review-tool-user-guide/credentials.md) artigo.

### <a name="use-the-apis"></a>Utilizar as APIs

Saiba como integrar o Content Moderator com seus aplicativos de negócios. Veja a [referência da API](api-reference.md) e o [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net).

## <a name="subscribe-in-the-azure-portal"></a>Subscrever no portal do Azure

[Subscrever o Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) no portal do Azure. Começar com uma das seguintes APIs:

### <a name="image-moderation"></a>Moderação de imagens

Começar com o [consola de API](try-image-api.md) ou utilizar o [guia de introdução do .NET](image-moderation-quickstart-dotnet.md) para analisar imagens e detetar potenciais conteúdos para adultos com etiquetas, as pontuações de confiança e outras extraiu informações.

### <a name="text-moderation"></a>Moderação de texto

Começar com o [consola de API](try-text-api.md) ou utilizar o [guia de introdução do .NET](text-moderation-quickstart-dotnet.md) para analisar o conteúdo de texto para potencial linguagem inapropriada, classificação de texto indesejado assistida (pré-visualização) e de identificação pessoal informações (PII). 


### <a name="video-moderation"></a>Moderação de vídeos

Começar com o [guia de introdução do .NET](video-moderation-api.md) para analisar vídeos e detetar potenciais conteúdos para adultos. 


### <a name="review-apis"></a>APIs de revisão

Comece por aqui ao selecionar a tarefa, revisão e APIs de fluxo de trabalho.

- O [API de tarefa](try-review-api-job.md) analisa o conteúdo ao utilizar as APIs de moderação e gera as revisões na ferramenta de revisão. 
- O [revisão de API](try-review-api-review.md) cria diretamente as revisões de vídeo, texto ou imagem de moderadores humanos sem primeiro a analisar o conteúdo. 
- O [API do fluxo de trabalho](try-review-api-workflow.md) cria, atualiza e obtém os detalhes sobre os fluxos de trabalho personalizados que cria a sua equipa.

## <a name="next-steps"></a>Passos Seguintes

Veja a [referência da API](api-reference.md) e o [SDKs](sdk-and-samples.md#sdks-for-python-java-nodejs-and-net). Impulsione a sua integração com o [amostras do SDK de .NET](sdk-and-samples.md#net-sdk-samples), [exemplos de REST API em c#](https://github.com/sanjeev3/azure-docs-pr/blob/master/articles/cognitive-services/Content-Moderator/sdk-and-samples.md#rest-api-samples-in-c) e [tutoriais](sdk-and-samples.md#tutorials).
