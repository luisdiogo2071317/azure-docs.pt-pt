---
title: O Azure Event Grid esquemas para eventos de serviços de multimédia
description: Descreve as propriedades que são fornecidas para eventos de serviços de multimédia com o Azure Event Grid
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: a6a6c459e170627d26aa1445f4f4dd193965fe70
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055426"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>O Azure Event Grid esquemas para eventos de serviços de multimédia

Este artigo fornece os esquemas e as propriedades para eventos de serviços de multimédia.

Para obter uma lista de scripts de exemplo e tutoriais, consulte [origem de eventos dos serviços de multimédia](../../event-grid/event-sources.md#azure-subscriptions).

## <a name="available-event-types"></a>Tipos de eventos disponíveis

Serviços de multimédia emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Altera o estado da tarefa. |
| Microsoft.Media.LiveEventConnectionRejected | Tentativa de ligação do codificador é rejeitada. |
| Microsoft.Media.LiveEventEncoderConnected | Codificador estabelece ligação com o evento em direto. |
| Microsoft.Media.LiveEventEncoderDisconnected | Codificador desliga. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Servidor de mídia ignora os segmentos de dados porque este seja muito tarde ou um timestamp sobreposto (timestamp de novos segmentos de dados é inferior à hora de fim do segmento de dados anterior). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Servidor de suporte de dados recebe o primeiro segmento de dados para cada faixa no stream ou ligação. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Servidor de suporte de dados Deteta áudio e fluxos de vídeo não estão sincronizados. Utilizar como um aviso porque a experiência do usuário não poderá ser afetada. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Servidor de suporte de dados Deteta qualquer um dos dois fluxos de vídeo vindo do codificador externa não estão sincronizadas. Utilizar como um aviso porque a experiência do usuário não poderá ser afetada. |
| Microsoft.Media.LiveEventIngestHeartbeat | Publicado a cada 20 segundos para cada faixa quando está a executar o evento em direto. Fornece ingestão de resumo de estado de funcionamento. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Servidor de suporte de dados Deteta descontinuidade na faixa de entrada. |

Existem duas categorias para o **Live** eventos: ao nível do fluxo de eventos e eventos em nível de controle. 

Eventos em nível de Stream são emitidos por fluxo ou ligação. Cada evento tem um `StreamId` parâmetro identifica o ligação ou fluxo. Cada fluxo ou a ligação tem um ou mais roteiros de diferentes tipos. Por exemplo, uma ligação a partir de um codificador pode ter uma faixa de áudio e as faixas de vídeo de quatro. Os tipos de eventos de fluxo são:

* LiveEventConnectionRejected
* LiveEventEncoderConnected
* LiveEventEncoderDisconnected

Eventos de nível de controle são gerados por faixa. Os tipos de eventos de controle são:

* LiveEventIncomingDataChunkDropped
* LiveEventIncomingStreamReceived
* LiveEventIncomingStreamsOutOfSync
* LiveEventIncomingVideoStreamsOutOfSync
* LiveEventIngestHeartbeat
* LiveEventTrackDiscontinuityDetected

## <a name="jobstatechange"></a>JobStateChange

O exemplo seguinte mostra o esquema do **JobStateChange** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| previousState | cadeia | O estado da tarefa antes do evento. |
| state | cadeia | O novo estado da tarefa a ser notificado neste evento. Por exemplo, "em fila: A tarefa está aguardando o recursos" ou "agendada: A tarefa está pronta para começar a".|

Em que o estado da tarefa pode ser um dos valores: *em fila*, *agendada*, *processamento*, *concluído*, *erro*, *Cancelada*, *Cancelar*

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

O exemplo seguinte mostra o esquema do **LiveEventConnectionRejected** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "StreamId":"Mystream1",
      "IngestUrl": "http://abc.ingest.isml",
      "EncoderIp": "118.238.251.xxx",
      "EncoderPort": 52859,
      "ResultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| StreamId | cadeia | Identificador do stream ou da ligação. Codificador ou o cliente é responsável para adicionar este ID no URL de ingestão. |  
| IngestUrl | cadeia | Ingestão de URL fornecido pelo evento em direto. |  
| EncoderIp | cadeia | IP do codificador. |
| EncoderPort | cadeia | Porta do codificador de onde vem esse fluxo. |
| ResultCode | cadeia | O motivo pelo qual a ligação foi rejeitado. Os códigos de resultado estão listados na tabela seguinte. |

Os códigos de resultado são:

| Código de resultado | Descrição |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | URL de ingestão de incorreto |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Codificador de IP não está presente no IP configurada de lista de permissões |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Codificador não envia metadados sobre o fluxo. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Codec especificado não é suportada. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED | Foi recebido um fragmento antes que recebem e cabeçalho para esse fluxo. |
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED | Número de qualidades especificado excede o limite máximo permitido. |
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED | Velocidade de transmissão agregada excede o limite permitido máximo. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | O carimbo de hora para FLVTag de vídeo ou áudio é inválido de codificador RTMP. |

## <a name="liveeventencoderconnected"></a>LiveEventEncoderConnected

O exemplo seguinte mostra o esquema do **LiveEventEncoderConnected** eventos: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| StreamId | cadeia | Identificador do stream ou da ligação. Codificador ou o cliente é responsável por fornecer este ID no URL de ingestão. |
| IngestUrl | cadeia | Ingestão de URL fornecido pelo evento em direto. |
| EncoderIp | cadeia | IP do codificador. |
| EncoderPort | cadeia | Porta do codificador de onde vem esse fluxo. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

O exemplo seguinte mostra o esquema do **LiveEventEncoderDisconnected** eventos: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| StreamId | cadeia | Identificador do stream ou da ligação. Codificador ou o cliente é responsável para adicionar este ID no URL de ingestão. |  
| IngestUrl | cadeia | Ingestão de URL fornecido pelo evento em direto. |  
| EncoderIp | cadeia | IP do codificador. |
| EncoderPort | cadeia | Porta do codificador de onde vem esse fluxo. |
| ResultCode | cadeia | O motivo para o codificador a desligar. É possível desligar anulações normais ou de um erro. Os códigos de resultado estão listados na tabela seguinte. |

Os códigos de resultado de erro são:

| Código de resultado | Descrição |
| ----------- | ----------- |
| MPE_RTMP_SESSION_IDLE_TIMEOUT | Sessão RTMP excedido o tempo limite após inatividade para o limite de tempo permitido. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | O carimbo de hora para FLVTag de vídeo ou áudio é inválido de codificador RTMP. |
| MPE_CAPACITY_LIMIT_REACHED | Codificador de envio de dados muito rápidos. |
| Códigos de erro desconhecido | Esses códigos de erro podem variar de erro de memória a duplicação de entradas no mapa de hash. |

Os códigos de resultado de anulações normais de desligamento são:

| Código de resultado | Descrição |
| ----------- | ----------- |
| S_OK | Codificador desligado com êxito. |
| MPE_CLIENT_TERMINATED_SESSION | Codificador desligado (RTMP). |
| MPE_CLIENT_DISCONNECTED | Codificador desligado (FMP4). |
| MPI_REST_API_CHANNEL_RESET | Comando de reposição do canal é recebido. |
| MPI_REST_API_CHANNEL_STOP | Foi recebido um comando de paragem de canal. |
| MPI_REST_API_CHANNEL_STOP | Canal em manutenção. |
| MPI_STREAM_HIT_EOF | Fluxo EOF é enviado pelo codificador. |

## <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

O exemplo seguinte mostra o esquema do **LiveEventIncomingDataChunkDropped** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "TrackType": "Video",
      "TrackName": "Video",
      "Bitrate": 300000,
      "Timestamp": 36656620000,
      "Timescale": 10000000,
      "ResultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| TrackType | cadeia | Tipo da faixa (áudio / vídeo). |
| TrackName | cadeia | Nome da faixa. |
| Velocidade de transmissão | inteiro | Velocidade de transmissão da faixa. |
| Carimbo de data/hora | cadeia | Timestamp do segmento de dados removida. |
| escala temporal | cadeia | Escala temporal do período de tempo. |
| ResultCode | cadeia | Motivo da lista de segmentos de dados. **FragmentDrop_OverlapTimestamp** ou **FragmentDrop_NonIncreasingTimestamp**. |

## <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

O exemplo seguinte mostra o esquema do **LiveEventIncomingStreamReceived** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| TrackType | cadeia | Tipo da faixa (áudio / vídeo). |
| TrackName | cadeia | Nome da faixa (qualquer um dos fornecidos pelo codificador ou, em caso de RTMP, servidor gera no *TrackType_Bitrate* formato). |
| Velocidade de transmissão | inteiro | Velocidade de transmissão da faixa. |
| IngestUrl | cadeia | Ingestão de URL fornecido pelo evento em direto. |
| EncoderIp | cadeia  | IP do codificador. |
| EncoderPort | cadeia | Porta do codificador de onde vem esse fluxo. |
| Carimbo de data/hora | cadeia | Primeira timestamp do segmento de dados recebido. |
| escala temporal | cadeia | Escala temporal em que é representado timestamp. |

## <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

O exemplo seguinte mostra o esquema do **LiveEventIncomingStreamsOutOfSync** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| MinLastTimestamp | cadeia | Mínimo de carimbos de data / último entre todas as faixas (vídeos ou áudio). |
| TypeOfTrackWithMinLastTimestamp | cadeia | Tipo de controle (vídeo ou áudio) com o mínimo timestamp último. |
| MaxLastTimestamp | cadeia | Máximo de todos os carimbos entre todas as faixas (vídeos ou áudio). |
| TypeOfTrackWithMaxLastTimestamp | cadeia | Tipo de controlar (vídeo ou áudio) com o máximo timestamp último. |

## <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

O exemplo seguinte mostra o esquema do **LiveEventIncomingVideoStreamsOutOfSync** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "FirstTimestamp": "2162058216",
      "FirstDuration": "2000",
      "SecondTimestamp": "2162057216",
      "SecondDuration": "2000"
    },
    "dataVersion": "1.0"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| FirstTimestamp | cadeia | Timestamp recebido para um dos níveis de qualidade/faixas de tipo de vídeo. |
| FirstDuration | cadeia | Duração do segmento de dados com timestamp primeiro. |
| SecondTimestamp | cadeia  | Timestamp recebido para algum outro nível de controle/qualidade do tipo vídeo. |
| SecondDuration | cadeia | Duração do segmento de dados com o segundo timestamp. |

## <a name="liveeventingestheartbeat"></a>LiveEventIngestHeartbeat

O exemplo seguinte mostra o esquema do **LiveEventIngestHeartbeat** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| TrackType | cadeia | Tipo da faixa (áudio / vídeo). |
| TrackName | cadeia | Nome da faixa (qualquer um dos fornecidos pelo codificador ou, em caso de RTMP, servidor gera no *TrackType_Bitrate* formato). |
| Velocidade de transmissão | inteiro | Velocidade de transmissão da faixa. |
| IncomingBitrate | inteiro | Velocidade de transmissão calculada com base nos segmentos de dados provenientes do codificador. |
| LastTimestamp | cadeia | Mais recente timestamp recebida um Roteiro na última 20 segundos. |
| escala temporal | cadeia | Escala temporal em que são expressos carimbos. |
| OverlapCount | inteiro | Número de segmentos de dados tinha overlapped carimbos de data / no último 20 segundos. |
| DiscontinuityCount | inteiro | Número de discontinuities observados nos últimos 20 segundos. |
| NonIncreasingCount | inteiro | Número de segmentos de dados carimbos de data no passado foram recebido nos últimos 20 segundos. |
| UnexpectedBitrate | Bool | Se forem diferentes velocidades de transmissão de expected e actual ao limite permitido mais do que em última 20 segundos. É verdadeiro se e apenas se, IncomingBitrate > = 2 * velocidade de transmissão ou IncomingBitrate < = IncomingBitrate ou de velocidade de transmissão/2 = 0. |
| Estado | cadeia | Estado do evento em direto. |
| Bom estado de funcionamento | Bool | Indica se ingerir é bom estado de funcionamento com base nas contagens e sinalizadores. Bom estado de funcionamento é verdadeiro se OverlapCount = 0 & & DiscontinuityCount = 0 & & NonIncreasingCount = 0 & & UnexpectedBitrate = false. |

## <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackDiscontinuityDetected

O exemplo seguinte mostra o esquema do **LiveEventTrackDiscontinuityDetected** eventos: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| TrackType | cadeia | Tipo da faixa (áudio / vídeo). |
| TrackName | cadeia | Nome da faixa (qualquer um dos fornecidos pelo codificador ou, em caso de RTMP, servidor gera no *TrackType_Bitrate* formato). |
| Velocidade de transmissão | inteiro | Velocidade de transmissão da faixa. |
| PreviousTimestamp | cadeia | Timestamp do fragmento anterior. |
| NewTimestamp | cadeia | Timestamp do fragmento atual. |
| DiscontinuityGap | cadeia | Lacuna entre acima dois carimbos. |
| escala temporal | cadeia | Escala temporal na qual lacuna timestamp e descontinuidade são representados. |

## <a name="common-event-properties"></a>Propriedades de evento comum

Um evento tem os seguintes dados de nível superior:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| tópico | cadeia | O tópico de EventGrid. Esta propriedade tem o ID de recurso para a conta de Media Services. |
| Assunto | cadeia | O caminho de recurso para o canal de serviços de multimédia sob a conta de Media Services. Concatenar dão o tópico e o assunto é o recurso de ID da tarefa. |
| eventType | cadeia | Um dos tipos de eventos registrados para esta origem de evento. Por exemplo, "Microsoft.Media.JobStateChange". |
| eventTime | cadeia | O tempo que o evento é gerado com base no fuso horário UTC do fornecedor. |
| ID | cadeia | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos de serviços de multimédia. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de eventos define o esquema das propriedades de nível superior. Event Grid fornece este valor. |

## <a name="next-steps"></a>Passos Seguintes

[Se registrar para eventos de alteração de estado de tarefa](job-state-events-cli-how-to.md)