---
title: Moderar conteúdo com revisões realizadas por pessoas com a consola de API - Content Moderator
titlesuffix: Azure Cognitive Services
description: Utilize da revisão de operações de API rever para criar as revisões de imagem ou texto de moderação humana.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 9d2daad06ad6cb97fe562db98cab57c4ed970a60
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260214"
---
# <a name="create-reviews-from-the-api-console"></a>Criar análises a partir da consola de API

Utilizar a API rever [rever operações](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) para criar as revisões de imagem ou texto de moderação humana. Os moderadores humanos utilizam a ferramenta de revisão para rever o conteúdo. Use essa operação com base na lógica de negócio de pós-moderação. Utilizá-lo após a ter análise seu conteúdo ao utilizar qualquer uma das outras APIs serviços cognitivos ou APIs de texto ou a imagem do moderador de conteúdo. 

Depois de um moderador humano revisa o atribuída automaticamente etiquetas e dados de predição e envia uma decisão de moderação final, a API de revisão envia todas as informações para o ponto final de API.

## <a name="use-the-api-console"></a>Utilizar a consola de API
Para testar a API utilizando a consola online, terá de alguns valores para celebrar o console:

- **teamName**: O nome da equipa que criou quando configurou a sua conta da ferramenta de revisão. 
- **ContentId**: Essa cadeia de caracteres é passada para a API e devolvida pelo retorno de chamada. O ContentId é útil para associar os resultados de uma tarefa de moderação identificadores internos ou de metadados.
- **Metadados**: Pares de chave-valor personalizados devolvidos ao seu ponto final da API durante o retorno de chamada. Se a chave é um código pequeno que é definido na ferramenta de revisão, ele aparece como uma etiqueta.
- **Ocp-Apim-Subscription-Key**: Localizado no **definições** separador. Para obter mais informações, veja [Descrição geral](overview.md).

A forma mais simples para aceder a uma consola de teste é a partir da **credenciais** janela.

1.  Na **credenciais** janela, selecione [referência da API de revisão](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  O **reveja - criar** é aberta a página.

2.  Para **consola de teste de API aberta**, selecione a região que melhor descreve sua localização.

  ![Examine - criar página seleção de região](images/test-drive-region.png)

  O **reveja - criar** é aberta a consola de API.
  
3.  Introduza valores para os parâmetros de consulta necessário, tipo de conteúdo e a chave de subscrição. Na **corpo do pedido** caixa, especifique o conteúdo (por exemplo, a localização de imagem), metadados e outras informações associadas com o conteúdo.

  ![Examine - criar parâmetros de consulta da consola, cabeçalhos e caixa de corpo de pedido](images/test-drive-review-1.PNG)
  
4.  Selecione **Enviar**. É criado um ID de revisão. Copie este ID para utilizar nos passos seguintes.

  ![Examine - criar console caixa apresenta o ID da revisão de conteúdo de resposta](images/test-drive-review-2.PNG)
  
5.  Selecione **obter**e, em seguida, abra a API ao selecionar o botão que corresponde à sua região. Na página resultante, introduza os valores para **teamName**, **ReviewID**, e **chave de subscrição**. Selecione o **enviar** botão na página. 

  ![Examine - criar console resultados de Get](images/test-drive-review-3.PNG)
  
6.  Verá os resultados da análise.

  ![Examine - criar a caixa de conteúdo de resposta de consola](images/test-drive-review-4.PNG)
  
7.  No Dashboard de moderador de conteúdo, selecione **revisão** > **imagem**. A imagem que analisados aparece, pronta para revisão humana.

  ![Imagem de ferramenta de revisão de uma bola de futebol](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [guia de introdução do .NET de revisões](moderation-reviews-quickstart-dotnet.md) para integrar com a sua aplicação.
