---
title: Executar tarefas de moderação de interrupção de conteúdo no Azure conteúdo Moderator | Microsoft Docs
description: Saiba como executar tarefas de moderação de interrupção de conteúdos na consola de API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 6f741be1001ae70d5fdbf6f374204aaad1601abe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351590"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Iniciar uma tarefa de moderação interrupção a partir da consola de API

Utilize a API de revisão [operações de tarefa](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) para iniciar tarefas de moderação de interrupção de conteúdo ponto-a-ponto para o conteúdo de imagem ou texto em Moderator de conteúdo do Azure. 

A tarefa de moderação interrupção analisa o conteúdo utilizando a API do conteúdo Moderator imagem moderação interrupção ou a API de moderação interrupção de texto. Em seguida, a tarefa de moderação interrupção utiliza fluxos de trabalho (definidos na ferramenta de revisão) para gerar revisões na ferramenta de revisão. 

Depois de um moderator humana revê as etiquetas atribuídos e os dados de predição e submete uma decisão de moderação de interrupção final, a API de revisão submete todas as informações para o ponto final de API.

## <a name="prerequisites"></a>Pré-requisitos

Navegue para o [Rever ferramenta](https://contentmoderator.cognitive.microsoft.com/). Inscrever-se se não o fez, ainda. Na ferramenta de revisão, [definir um fluxo de trabalho personalizado](Review-Tool-User-Guide/Workflows.md) para utilizar neste `Job` operação.

## <a name="use-the-api-console"></a>Utilize a consola de API
Para test-drive a API utilizando a consola online, terá de alguns valores para introduzir para a consola:
    
- `teamName`: Utilize o `Id` campo a partir do ecrã de credenciais da sua ferramenta de revisão. 
- `ContentId`: Esta cadeia é transmitida para a API e devolvida através de chamada de retorno. **ContentId** é útil para associar os resultados de uma tarefa de moderação interrupção. identificadores internos ou metadados- `Workflowname`: O nome do [fluxo de trabalho que criou](Review-Tool-User-Guide/Workflows.md) na secção anterior.
- `Ocp-Apim-Subscription-Key`: Localizado o **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

Consola de acesso a API é a partir de **credenciais** janela.

### <a name="navigate-to-the-api-reference"></a>Navegue para a referência de API
No **credenciais** janela, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  O `Job.Create` é aberta a página.

### <a name="select-your-region"></a>Selecione a região
Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.
  ![Tarefa - seleção de região de página de criação](images/test-drive-job-1.png)

  O `Job.Create` é aberta a consola de API. 

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza valores para os parâmetros de consulta necessário e a chave de subscrição. No **corpo do pedido** caixa, especifique a localização das informações que pretende analisar. Neste exemplo, vamos utilizar isto [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Tarefa - os parâmetros de consulta da consola, cabeçalhos e caixa de corpo do pedido de criação](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Submeter o pedido
Selecione **Enviar**. Um ID de tarefa é criado. Copie esta opção para utilizar os passos seguintes.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Abra a página de detalhes da tarefa de obter
Selecione **obter**e, em seguida, abra a API, selecionando o botão que corresponda à sua região.

  ![Tarefa - criar consola resultados de Get](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Reveja a resposta

Introduza os valores para **teamName** e **JobID**. Introduza a chave de subscrição e, em seguida, selecione **enviar**. A resposta seguinte mostra o estado da tarefa de exemplo e os detalhes.

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
No Dashboard do conteúdo Moderator, selecione **revisão** > **imagem**. Aparece a imagem que lhe foram analisados, prontos para revisão humano.

  ![Reveja a imagem de ferramenta de três cyclists](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [início rápido de .NET de tarefas](moderation-jobs-quickstart-dotnet.md) para integrar com a sua aplicação.
