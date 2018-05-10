---
title: Esquemas de grelha de eventos do Azure para eventos de Media Services
description: Descreve as propriedades que são fornecidas para eventos de Media Services com a grelha de eventos do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: c443ea755c9e7e88b4685682d5e43f675d42efa4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Esquemas de grelha de eventos do Azure para eventos de Media Services

Este artigo fornece os esquemas e as propriedades de eventos de Media Services.

## <a name="media-services-job-state-change-event"></a>Evento de alteração de estado de tarefa de serviços de suporte de dados

Esta secção fornece que as propriedades e o esquema para o estado da tarefa de serviços de suporte de dados de evento de alteração.  

### <a name="available-event-types"></a>Tipos de evento disponíveis

Dos Media Services **tarefa** emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| É desencadeado quando um Estado das alterações de tarefa. |

### <a name="example-event"></a>Eventos de exemplo

O exemplo seguinte mostra o esquema de um evento de estado de tarefa concluído: 

```json
[
  {
    "topic": "/subscriptions/{subscription id}/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
    "subject": "transforms/VideoAnalyzerTransform/jobs/{job id}",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "<id>",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

### <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Tópico | string | Grelha de evento fornece este valor. |
| Requerente | string | O caminho do recurso sob o recurso de conta de Media Services. |
| eventType | string | Um dos tipos de eventos registados para esta origem de evento. Por exemplo, "Microsoft.Media.JobStateChange". |
| eventTime | string | A hora que do evento é gerado com base na hora UTC do fornecedor. |
| ID | string | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos de Media Services. |
| dataVersion | string | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | string | A versão de esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| previousState | string | O estado da tarefa antes do evento. |
| state | string | O novo estado da tarefa que está a ser notificado neste evento. Por exemplo, "em fila: A tarefa está aguardar Recursos" ou "agendada: A tarefa está pronta para começar a".|

Em que o estado da tarefa pode ser um dos valores: *em fila*, *agendada*, *processamento*, *concluído*, *erro*, *Cancelada*, *Cancelar*

## <a name="next-steps"></a>Passos Seguintes

[Registar eventos de alteração de estado de tarefa](job-state-events-cli-how-to.md)
