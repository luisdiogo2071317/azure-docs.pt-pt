---
title: Executar tarefas de moderação de conteúdos com a consola de API - Content Moderator
titlesuffix: Azure Cognitive Services
description: Utilize operações de tarefa a API de revisão para iniciar tarefas de moderação de conteúdos do ponto-a-ponto para o conteúdo de imagem ou texto no Azure Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 8ad504051980356737dbc9af9263bc8370421cdb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228224"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Iniciar uma tarefa de moderação a partir da consola de API

Utilizar a API de revisão [operações de tarefa](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) para iniciar tarefas de moderação de conteúdos do ponto-a-ponto para o conteúdo de imagem ou texto no Azure Content Moderator. 

A tarefa de moderação analisa o conteúdo utilizando a API de moderação de imagens de moderador de conteúdo ou a API de moderação de texto. Em seguida, a tarefa de moderação utiliza fluxos de trabalho (definidos na ferramenta de revisão) para gerar as revisões na ferramenta de revisão. 

Depois de um moderador humano revisa o atribuída automaticamente etiquetas e dados de predição e envia uma decisão de moderação final, a API de revisão envia todas as informações para o ponto final de API.

## <a name="prerequisites"></a>Pré-requisitos

Navegue para o [ferramenta de revisão](https://contentmoderator.cognitive.microsoft.com/). Inscreva-se se não o tiver feito isso ainda. Dentro da ferramenta de revisão [definir um fluxo de trabalho personalizado](Review-Tool-User-Guide/Workflows.md) de usar nisso `Job` operação.

## <a name="use-the-api-console"></a>Utilizar a consola de API
Para testar a API utilizando a consola online, terá de alguns valores para celebrar o console:
    
- `teamName`: Utilize o `Id` campo a partir do ecrã de credenciais da sua ferramenta de revisão. 
- `ContentId`: Essa cadeia de caracteres é passada para a API e devolvida pelo retorno de chamada. **ContentId** é útil para associar os resultados de uma tarefa de moderação identificadores internos ou de metadados.- `Workflowname`: O nome da [fluxo de trabalho que criou](Review-Tool-User-Guide/Workflows.md) na secção anterior.
- `Ocp-Apim-Subscription-Key`: Localizado no **definições** separador. Para obter mais informações, veja [Descrição geral](overview.md).

Consola de acesso a API é a partir da **credenciais** janela.

### <a name="navigate-to-the-api-reference"></a>Navegue para a referência de API
Na **credenciais** janela, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  O `Job.Create` é aberta a página.

### <a name="select-your-region"></a>Selecione a sua região
Para **consola de teste de API aberta**, selecione a região que melhor descreve sua localização.
  ![Tarefa - criar a seleção de região de página](images/test-drive-job-1.png)

  O `Job.Create` é aberta a consola de API. 

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza valores para os parâmetros de consulta necessário e a chave de subscrição. Na **corpo do pedido** caixa, especifique a localização das informações que pretende analisar. Neste exemplo, vamos usar isso [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Tarefa - criar parâmetros de consulta da consola, cabeçalhos e caixa de corpo de pedido](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Submeter o pedido
Selecione **Enviar**. Um ID da tarefa é criada. Copie esta opção para utilizar nos passos seguintes.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Abra a página de detalhes do obter tarefa
Selecione **obter**e, em seguida, abra a API ao selecionar o botão que corresponde à sua região.

  ![Tarefa - criar console resultados de Get](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Rever a resposta

Introduza os valores para **teamName** e **JobID**. Introduza a chave de subscrição e, em seguida, selecione **enviar**. A seguinte resposta mostra o estado da tarefa de exemplo e os detalhes.

```
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": 
        [
            {
            "Ts": "2018-01-07T00:38:26.7714671",
                "Msg": "Successfully got hasText response from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:26.4181346",
                "Msg": "Getting hasText from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:25.5122828",
                "Msg": "Starting Execution - Try 1"
            }
        ]
    }
```

## <a name="navigate-to-the-review-tool"></a>Navegue para a ferramenta de revisão
No Dashboard de moderador de conteúdo, selecione **revisão** > **imagem**. A imagem que analisados aparece, pronta para revisão humana.

  ![Imagem de ferramenta de revisão de três cyclists](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [guia de introdução do .NET de tarefas](moderation-jobs-quickstart-dotnet.md) para integrar com a sua aplicação.
