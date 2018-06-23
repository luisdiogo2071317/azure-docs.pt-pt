---
title: Azure Moderator conteúdo - tarefas moderação interrupção e humanos no ciclo revisões | Microsoft Docs
description: Aplica supervisão humana a moderação de interrupção assistida por máquina para obter os melhores resultados.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: 35b3cdaa410712c3fd08d3df4ebe4c83e3955d50
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351769"
---
# <a name="moderation-jobs-and-reviews"></a>Tarefas de moderação interrupção e revisões

Combinar moderação de interrupção auxiliada a uma máquina com capacidades de humanos no ciclo utilizando o Moderator conteúdo Azure [revisão API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) para obter os melhores resultados para a sua empresa.

A API de revisão oferece as seguintes opções para incluir supervisão humana no seu processo de moderação de interrupção de conteúdo:

* `Job` operações são utilizadas para iniciar a moderação de interrupção assistido por computador e a criação de revisão humana como um passo.
* `Review` operações são utilizadas para a criação de revisão humanos, fora do passo de moderação interrupção.
* `Workflow` operações são utilizadas para gerir fluxos de trabalho que automatizam a análise com limiares para a criação de revisão.

O `Job` e `Review` operações aceitam pontos finais da sua chamada de retorno para a receção de estado e os resultados.

Este artigo abrange o `Job` e `Review` operações. Leia o [descrição geral de fluxos de trabalho](workflow-api.md) para obter informações sobre como criar, editar e obter as definições de fluxo de trabalho.

## <a name="job-operations"></a>Operações de tarefa

### <a name="start-a-job"></a>Iniciar uma tarefa
Utilize o `Job.Create` operação para iniciar um moderação interrupção e a tarefa de criação de revisão humanos. Conteúdo Moderator analisa o conteúdo e avalia o fluxo de trabalho designado. Com base nos resultados de fluxo de trabalho, cria revisões ou ignore o passo. Submete também as etiquetas de pós-moderação de interrupção e Revisão pós-implementação para o ponto final de chamada de retorno.

As entradas incluem as seguintes informações:

- O ID de equipa de revisão.
- O conteúdo a ser moderated.
- O nome do fluxo de trabalho. (A predefinição é o fluxo de trabalho "predefinido").
- A chamada de retorno de API de pontos de notificações.
 
A resposta seguinte mostra o identificador da tarefa que foi iniciado. Utilize o identificador de tarefa para obter o estado da tarefa e receber informações detalhadas.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>Obter estado da tarefa

Utilize o `Job.Get` operação e o identificador de tarefa para obter os detalhes de uma tarefa em execução ou foi concluída. A operação devolve imediatamente enquanto a moderação interrupção tarefa será executada no modo assíncrono. Os resultados são devolvidos através do ponto final de chamada de retorno.

Os parâmetros e incluem as seguintes informações:

- O ID da equipa de revisão: O identificador de tarefa devolvido pela operação anterior

A resposta inclui as seguintes informações:

- O identificador de revisão criada. (Utilizar este ID para obter os resultados da revisão final).
- O estado da tarefa (concluída ou em curso): as etiquetas de moderação de interrupção atribuído (pares chave-valor).
- O relatório de execução da tarefa.
 
 
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
 
![Revisão de imagem para moderators humanos](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Operações de revisão

### <a name="create-reviews"></a>Criar revisões

Utilize o `Review.Create` operação para criar as revisões humanos. Moderada de noutro local ou utilizar lógica personalizada para atribuir as etiquetas de moderação interrupção.

Os parâmetros e a esta operação incluem:

- O conteúdo a ser revisto.
- As etiquetas (pares de valor de chave) atribuídas para revisão por moderators humanos.

A resposta seguinte mostra o identificador de revisão:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Obter o estado de revisão
Utilize o `Review.Get` operação para obter os resultados de uma revisão humana da imagem moderated esteja concluída. Seja notificado através do ponto final de chamada de retorno fornecida. 

A operação devolve dois conjuntos de etiquetas: 

* As etiquetas atribuídas pelo serviço moderação interrupção
* As etiquetas desde que foi concluída a revisão humano

Os parâmetros e incluem um mínimo:

- O nome do agrupamento de revisão
- O identificador de revisão devolvido pela operação anterior

A resposta inclui as seguintes informações:

- O estado de revisão
- As etiquetas (pares chave-valor) confirmadas pelo humano revisor
- As etiquetas (pares chave-valor) atribuídas pelo serviço moderação interrupção

Consulte ambas as etiquetas atribuído revisor (**reviewerResultTags**) e as etiquetas iniciais (**metadados**) na resposta de exemplo seguinte:

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

* Testar o [consola de API de tarefa](try-review-api-job.md)e utilizar os exemplos de código da REST API. Se estiver familiarizado com o Visual Studio e c#, consulte também o [início rápido de .NET de tarefas](moderation-jobs-quickstart-dotnet.md). 
* Para revisões, comece a [consola revisão API](try-review-api-review.md)e utilizar os exemplos de código da REST API. Em seguida, consulte o [início rápido de .NET de revisões](moderation-reviews-quickstart-dotnet.md).
* Para as vídeos revisões, utilize o [início rápido de revisão de vídeo](video-reviews-quickstart-dotnet.md)e saber como [adicionar transcrições para rever a vídeo](video-transcript-reviews-quickstart-dotnet.md).
