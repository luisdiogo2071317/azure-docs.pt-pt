---
title: Moderada de conteúdos através da utilização de revisões humanos no Azure conteúdo Moderator | Microsoft Docs
description: Saiba como criar revisões humanos na consola do conteúdo Moderator API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: e9faf595e65ba4475a743e4cb45919fd30fbd6e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351578"
---
# <a name="create-reviews-from-the-api-console"></a>Criar revisões a partir da consola de API

Utilize a API rever [rever operações](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) criar revisões de imagem ou de texto para moderação de interrupção humana. Moderators humanos utilizam a ferramenta de revisão para rever o conteúdo. Utilize esta operação com base na lógica de negócio de pós-moderação de interrupção. Utilizá-lo Depois de ter analisados o conteúdo utilizando qualquer um dos outros cognitivos APIs de serviços ou APIs de texto ou a imagem de conteúdo Moderator. 

Depois de um moderator humana revê as etiquetas atribuídos e os dados de predição e submete uma decisão de moderação de interrupção final, a API de revisão submete todas as informações para o ponto final de API.

## <a name="use-the-api-console"></a>Utilize a consola de API
Para test-drive a API utilizando a consola online, terá de alguns valores para introduzir para a consola:

- **teamName**: O nome do agrupamento que criou quando configurou a sua conta da ferramenta de revisão. 
- **ContentId**: Esta cadeia é transmitida para a API e devolvida através de chamada de retorno. O ContentId é útil para associar os resultados de uma tarefa de moderação interrupção identificadores internos ou de metadados.
- **Metadados**: pares chave-valor de personalizado devolvido para o ponto final de API durante a chamada de retorno. Se a chave é um código curto que está definido na ferramenta de revisão, aparece como uma etiqueta.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

A forma mais simples para aceder a uma consola de teste é do **credenciais** janela.

1.  No **credenciais** janela, selecione [referência da API de revisão](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  O **rever - criar** é aberta a página.

2.  Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.

  ![Reveja - seleção de região de página de criação](images/test-drive-region.png)

  O **rever - criar** é aberta a consola de API.
  
3.  Introduza valores para os parâmetros de consulta necessário, tipo de conteúdo e a chave de subscrição. No **corpo do pedido** caixa, especifique o conteúdo (por exemplo, a localização de imagem), metadados e outras informações associadas com o conteúdo.

  ![Reveja - os parâmetros de consulta da consola, cabeçalhos e caixa de corpo do pedido de criação](images/test-drive-review-1.PNG)
  
4.  Selecione **Enviar**. É criado um ID de revisão. Copie este ID para utilizar os seguintes passos.

  ![Reveja - criar consola resposta conteúda caixa apresenta o ID de revisão](images/test-drive-review-2.PNG)
  
5.  Selecione **obter**e, em seguida, abra a API, selecionando o botão que corresponda à sua região. Na página de resultante, introduza os valores para **teamName**, **ReviewID**, e **chave de subscrição**. Selecione o **enviar** botão na página. 

  ![Reveja - consola de criação de resultados de Get](images/test-drive-review-3.PNG)
  
6.  Verá os resultados da análise.

  ![Reveja - criar caixa de conteúdo de resposta de consola](images/test-drive-review-4.PNG)
  
7.  No Dashboard do conteúdo Moderator, selecione **revisão** > **imagem**. Aparece a imagem que lhe foram analisados, prontos para revisão humano.

  ![Reveja a imagem de ferramenta de um ball soccer](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [início rápido de .NET de revisões](moderation-reviews-quickstart-dotnet.md) para integrar com a sua aplicação.
