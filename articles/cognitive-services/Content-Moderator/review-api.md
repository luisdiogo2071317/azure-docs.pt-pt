---
title: Tarefas de moderação e avaliações de humanos em loop - Content Moderator
titlesuffix: Azure Cognitive Services
description: Aplicam-se a supervisão humana para moderação assistida por computador para obter melhores resultados.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: b4a2f62b1c9cefb716cb217baf7389c3e7c790b8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223260"
---
# <a name="moderation-jobs-and-reviews"></a>Tarefas de moderação e revisões

Combinar moderação assistida por computador com recursos humanos em loop através da utilização do Azure Content Moderator [revisão de API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) para obter os melhores resultados para a sua empresa.

A revisão de API oferece as seguintes opções para incluir a supervisão humana em seu processo de moderação de conteúdos:

* `Job` operações são utilizadas para iniciar o moderação assistida por computador e a criação de revisão humana como uma etapa.
* `Review` operações são utilizadas para a criação de revisão humana, fora a etapa de moderação.
* `Workflow` operações são utilizadas para gerir fluxos de trabalho que automatizam a verificação com limiares para a criação de revisão.

O `Job` e `Review` operações de aceitam os pontos de extremidade do retorno de chamada para o recebimento de status e os resultados.

Este artigo aborda os `Job` e `Review` operações. Leitura a [descrição geral de fluxos de trabalho](workflow-api.md) para obter informações sobre como criar, editar e obtém as definições de fluxo de trabalho.

## <a name="job-operations"></a>Operações de tarefa

### <a name="start-a-job"></a>Iniciar uma tarefa
Utilize o `Job.Create` operação para iniciar uma moderação e a tarefa de criação de revisão humana. O Content Moderator analisa o conteúdo e avalia o fluxo de trabalho designado. Com base nos resultados de fluxo de trabalho, ele o cria revisões ou ignora o passo. Submete também as etiquetas de pós- moderação de e pós-revisão para o ponto final de retorno de chamada.

As entradas incluem as seguintes informações:

- O ID de equipa de revisão.
- O conteúdo para ser moderado.
- O nome do fluxo de trabalho. (A predefinição é o fluxo de trabalho de "predefinição".)
- Seu retorno de chamada de API ponto para notificações.
 
A seguinte resposta mostra o identificador da tarefa que foi iniciado. Utilize o identificador de tarefa para obter o estado da tarefa e receber informações detalhadas.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>Obter estado da tarefa

Utilize o `Job.Get` operação e o identificador de tarefa para obter os detalhes de uma tarefa em execução ou concluído. A operação devolve imediatamente enquanto a tarefa de moderação é executada de forma assíncrona. Os resultados são devolvidos através do ponto final de retorno de chamada.

Suas entradas incluem as seguintes informações:

- O ID da equipa de revisão: O identificador de tarefa devolvido pela operação anterior

A resposta inclui as seguintes informações:

- O identificador de revisão criada. (Utilizar este ID para obter os resultados de revisão final).
- O estado da tarefa (conclusão ou em curso): as etiquetas de moderação atribuído (pares chave-valor).
- O relatório de execução de tarefa.
 
 
        {
            "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
            "TeamName": "some team name",
            "Status": "Complete",
            "WorkflowId": "OCR",
            "Type": "Image",
            "CallBackEndpoint": "",
            "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
            "ResultMetaData":[
            {
            "Key": "hasText",
            "Value": "True"
            },
            {
            "Key": "ocrText",
            "Value": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
            }
            ],
            "JobExecutionReport": [
            {
                "Ts": "2018-01-07T00:38:29.3238715",
                "Msg": "Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
                },
                {
                "Ts": "2018-01-07T00:38:29.2928416",
                "Msg": "Job marked completed and job content has been removed"
                },
                {
                "Ts": "2018-01-07T00:38:29.0856472",
                "Msg": "Execution Complete"
                },
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
 
![Revisão de imagem para moderadores humanos](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Operações de revisão

### <a name="create-reviews"></a>Criar as revisões

Utilize o `Review.Create` operação para criar as revisões de realizadas por pessoas. Moderá-los em outro lugar ou usar lógica personalizada para atribuir as etiquetas de moderação.

Suas entradas para esta operação incluem:

- O conteúdo a ser revisto.
- As etiquetas (pares chave-valor) atribuídas para revisão por moderadores humanos.

A seguinte resposta mostra o identificador de revisão:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Obter estado da revisão
Utilize o `Review.Get` operação para obter os resultados após a conclusão de uma revisão de segurança da imagem moderada. Seja notificado através de ponto final da chamada de retorno fornecida. 

A operação retorna dois conjuntos de etiquetas: 

* As etiquetas atribuídas pelo serviço de moderação
* As marcas de revisão humana foi concluída

Suas entradas incluem-se no mínimo:

- O nome da equipa de revisão
- O identificador de revisão devolvido pela operação anterior

A resposta inclui as seguintes informações:

- O estado de revisão
- As etiquetas (pares chave-valor) confirmadas pelo revisor humano
- As etiquetas (pares chave-valor) atribuídas pelo serviço de moderação

Ver ambas as etiquetas atribuídas revisor (**reviewerResultTags**) e as etiquetas iniciais (**metadados**) na resposta de exemplo seguinte:

    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="next-steps"></a>Passos Seguintes

* Faça o test drive da [consola de API de tarefa](try-review-api-job.md)e utilize os exemplos de código da REST API. Se estiver familiarizado com o Visual Studio e c#, também, veja a [guia de introdução do .NET de tarefas](moderation-jobs-quickstart-dotnet.md). 
* Para revisões, comece com o [consola de API de revisão](try-review-api-review.md)e utilize os exemplos de código da REST API. Em seguida, consulte a [guia de introdução do .NET de revisões](moderation-reviews-quickstart-dotnet.md).
* Para revisões de vídeo, utilize o [guia de introdução do vídeo revisão](video-reviews-quickstart-dotnet.md)e saiba como [adicionar transcrições para a revisão de vídeo](video-transcript-reviews-quickstart-dotnet.md).
