---
title: Submeter tarefas de recorte Recortador de suporte de dados do Azure | Microsoft Docs
description: Passos para submeter tarefas de recorte de Recortador de suporte de dados do Azure
services: media-services
keywords: "Clip; subclip codificação; suporte de dados"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8372c405087c0dc7a000a65265bb99c395c3a8d6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="submit-clipping-jobs-from-azure-media-clipper"></a>Submeter tarefas de recorte de Recortador de suporte de dados do Azure
Recortador de suporte de dados do Azure requer um **submitSubclipCallback** método para ser implementada para processar a submissão da tarefa de recorte. Esta função é para implementar um POST de HTTP de saída Recortador a um serviço web. Este serviço web é onde pode submeter a tarefa de codificação. O resultado do Recortador é a um codificador de multimédia Standard codificação predefinição para as tarefas compostas ou o payload de REST API para chamadas de filtro de manifesto dinâmico. Este modelo pass-through é necessário porque as credenciais de conta de serviços de suporte de dados não são seguras no browser do cliente.

O diagrama de sequência seguinte ilustra o fluxo de trabalho entre o browser cliente, o serviço web e os Media Services do Azure: ![diagrama de sequência de Recortador de suporte de dados do Azure](media/media-services-azure-media-clipper-submit-job/media-services-azure-media-clipper-sequence-diagram.PNG)

No diagrama anterior, as quatro entidades são: browser do utilizador final, o serviço web, o ponto final da CDN que aloja os recursos de Recortador e os Media Services do Azure. Quando o utilizador final navega para a página web, a página obtém o Recortador JavaScript e os recursos do CSS do ponto final de CDN alojamento. O utilizador final configura a tarefa de recorte ou chamada de criação de filtro de manifesto dinâmica partir do respetivo browser. Quando o utilizador final submete a tarefa ou um filtro de criação de chamada, o browser coloca o payload de tarefa a um serviço web que tem de implementar. Este serviço web, fundamentalmente, submete a tarefa de recorte ou as credenciais da conta de serviços de criação de chamada de filtro para utilizar o suporte de dados de Media Services do Azure.

O exemplo de código seguinte ilustra um exemplo **submitSubclipCallback** método. Neste método, implementa o HTTP POST da predefinição de codificação codificador de multimédia Standard. Se a publicação foi efetuada com êxito (**resultado**), o **promessa** for resolvido, caso contrário, que é rejeitado com detalhes do erro.

```javascript
// Submit Subclip Callback
//
// Parameter:
// - subclip: object that represents the subclip (output contract).
//
// Returns: a Promise object that, when resolved, retuns true if the operation was accept in the back-end; otherwise, returns false.
var onSubmitSubclip = function (subclip) {
    var promise = new Promise(function (resolve, reject) {
        // TODO: perform the back-end AJAX request to submit the subclip job.
        var result = true;

        if (/* everything turned out fine */ result) {
            resolve(result);
        }
        else {
            reject(Error("error details"));
        }
    });

    return promise;
};

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
```
O resultado a submissão da tarefa é o a predefinição de codificação codificador de multimédia Standard para a tarefa composta ou o payload de REST API para os filtros de manifesto dinâmicos.

## <a name="submitting-encoding-job-to-create-video"></a>A submeter a tarefa de codificação para criar as vídeo
Pode submeter uma tarefa de codificação codificador de multimédia Standard para criar uma moldura exata clip de vídeo. Codificação produzir tarefa composto vídeos, um novo fragmentados ficheiro MP4.

O contrato de saída da tarefa para recorte composto é um objeto JSON com as seguintes propriedades:

```json
{
  /* Required */
  "name": "My New Subclip Output Name",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: The REST API version, currently 2.0 is supported
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id-1",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }, {
    /* Required */
    "id": "my-asset-id-2",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "job",

    /* Required if "type" === "job" */
    /* NOTE: This is the preset for the Media Encoder Standard (MES) processor that can be used in the back-end to sumit the subclip job.
    The encoding profile ("Codecs" property) depends on the "singleBitrateMp4Profile" and "multiBitrateMp4Profile" option parameters
    specified when creating the widget instance. */
    /* REFERENCE: https://docs.microsoft.com/azure/media-services/media-services-advanced-encoding-with-mes */
    "job": {
      "Version": 1.0,
      "Codecs": [{
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [{
            "Profile": "Auto",
            "Level": "auto",
            "Bitrate": 6750,
            "MaxBitrate": 6750,
            "BufferWindow": "00:00:05",
            "Width": 1920,
            "Height": 1080,
            "BFrames": 3,
            "ReferenceFrames": 3,
            "AdaptiveBFrame": true,
            "Type": "H264Layer",
            "FrameRate": "0/1"
          }],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [{
        "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
        "Format": {
          "Type": "MP4Format"
        }
      }],
      "Sources": [{
        "AssetId": "my-asset-id-1",
        "StartTime": "0.00:01:15.000",
        "Duration": "0.00:00:25.000"
      }, {
        "AssetId": "my-asset-id-2",
        "StartTime": "0.00:20:04.000",
        "Duration": "0.00:33:07.500"
      }]
    }
  }
}
```

Para efetuar a tarefa de codificação, submeter o codificador de multimédia Standard da tarefa de codificação associada com configuração predefinida. Consulte este artigo para obter detalhes sobre a codificação a submeter as tarefas utilizando o [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-encode-with-media-encoder-standard) ou [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-encode-asset).

## <a name="quickly-creating-video-clips-without-encoding"></a>Criar rapidamente aplicações de clips de vídeos sem codificação
Alternativa para a criação de uma tarefa de codificação, pode utilizar o Recortador de suporte de dados do Azure para criar o manifesto de filtros dinâmicos. Os filtros não necessitam de codificação e podem ser criados rapidamente, como um novo elemento não é criado. O contrato de saída para um recorte de filtro é um objeto JSON com as seguintes propriedades:

```json
{
  /* Required: Filter name (Alphanumeric characters and hyphens only, no whitespace) */
  "name": "FilterName",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: This subclipper version always returns '2.0' in this field.
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "filter",

    /* Required if "type" === "filter" */
    /* REFERENCE: https://docs.microsoft.com/rest/api/media/operations/assetfilter */
    "filter": {
      "PresentationTimeRange": {
        "StartTimestamp": "340000000",
        "EndTimestamp": "580000000",
        "Timescale": "10000000"
      },
      "Tracks": [{
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "video",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "audio",
          "Operator": "Equal"
        }, {
          "Property": "Name",
          "Value": "audio-track-1",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "text",
          "Operator": "Equal"
        }, {
          "Property": "Language",
          "Value": "en",
          "Operator": "Equal"
        }]
      }]
    }
  }
}
```

Para submeter a chamada REST para criar o filtro de manifesto dinâmico, submeter o payload de filtro associado utilizando o [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest).