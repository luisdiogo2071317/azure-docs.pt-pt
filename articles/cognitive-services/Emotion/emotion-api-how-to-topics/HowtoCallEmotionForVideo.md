---
title: 'Exemplo: Chamar a API de emoções para vídeo'
titlesuffix: Azure Cognitive Services
description: Saiba como chamar a API de Emoções para Vídeo nos Serviços Cognitivos.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: sample
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 191e20ba608adfdfd1ea4e5479cf1d326640d378
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862790"
---
# <a name="example-call-emotion-api-for-video"></a>Exemplo: Chamar a API de emoções para vídeo

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Este guia demonstra como chamar a API de Emoções para Vídeo. Os exemplos são escritos em C# com a biblioteca de cliente da API de Emoções para Vídeo.

### <a name="Prep">Preparação</a>
Para utilizar a API de Emoções para Vídeo, precisa de um vídeo que inclua pessoas, preferencialmente um vídeo onde as pessoas estejam viradas para a câmara.

### <a name="Step1">Passo 1: Autorizar a chamada de API</a>
Todas as chamadas à API de Emoções para Vídeo necessitam de uma chave de subscrição. Esta chave tem de ser transmitida através de um parâmetro de cadeia de consulta ou pode ser especificada no cabeçalho do pedido. Para transmitir a chave de subscrição através de uma cadeia de consulta, veja o URL do pedido abaixo da API de Emoções para Vídeo como exemplo:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Como alternativa, a chave de subscrição também pode ser especificada no cabeçalho do pedido HTTP:

```
ocp-apim-subscription-key: <Your subscription key>
```

Ao utilizar uma biblioteca de cliente, a chave de subscrição é transmitida através do construtor da classe VideoServiceClient. Por exemplo:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Para obter uma chave de subscrição, veja [Subscrições](https://azure.microsoft.com/try/cognitive-services/).

### <a name="Step2">Passo 2: Carregar um vídeo para o serviço e verificar o Estado</a>
A forma mais básica de efetuar qualquer uma das chamadas à API de Emoções para Vídeo é ao carregar um vídeo diretamente. Isto é feito ao enviar um pedido "POST" com o tipo de conteúdo application/octet-stream, juntamente com os dados lidos num ficheiro de vídeo. O tamanho máximo do vídeo são 100 MB.

Com a biblioteca de cliente, a estabilização por meio de carregamento é feita ao transmitir um objeto de transmissão em fluxo. Veja o exemplo abaixo:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Tenha em atenção que o método CreateOperationAsync VideoServiceClient é assíncrono. O método de chamada deve ser marcado como async (assíncrono), para poder utilizar a cláusula wait (aguardar).
Se o vídeo já estiver na Web e tiver um URL público, a API de Emoções para Vídeo pode ser acedida ao fornecer o URL. Neste exemplo, o corpo do pedido será uma cadeia JSON, que contém o URL.

Com a biblioteca de cliente, a estabilização por meio de um URL pode ser executada facilmente ao utilizar outra sobrecarga do método CreateOperationAsync.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Este método de carregamento será o mesmo para todas as chamadas à API de Emoções para Vídeo.

Depois de carregar um vídeo, a operação seguinte que deverá executar é verificar o respetivo estado. Como os ficheiros de vídeo são normalmente maiores e mais diversos do que outros ficheiros, os utilizadores podem esperar um tempo de processamento longo neste passo. O tempo depende do tamanho do ficheiro.

Com a biblioteca de cliente, pode obter o estado e o resultado da operação com o método GetOperationResultAsync.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Normalmente, o lado do cliente deve obter periodicamente o estado da operação até o estado ser mostrado como "Succeeded" (Com êxito) ou "Failed" (Falhado).

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

Quando o estado de VideoOperationResult é mostrado como "Succeeded" (Com êxito), o resultado pode ser obtido ao converter VideoOperationResult em VideoOperationInfoResult<VideoAggregateRecognitionResult> e ao aceder ao campo ProcessingResult.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Passo 3: A obter e entender o reconhecimento de emoções e controlar a saída JSON</a>

O resultado de saída contém os metadados dos rostos no ficheiro especificado no formato JSON.

Conforme explicado no Passo 2, a saída JSON está disponível no campo ProcessingResult de OperationResult, quando o estado é mostrado como "Succeeded" (Com êxito).

O JSON de controlo e deteção de rostos inclui os seguintes atributos:

Atributo | Descrição
-------------|-------------
Versão | Refere-se à versão do JSON da API de Emoções para Vídeo.
Escala temporal | "Tiques" por segundo do vídeo.
Desvio  |Desvio de tempo para carimbos de data/hora. Na versão 1.0 da API de Emoções para Vídeo, será sempre 0. Nos futuros cenários suportados, pode alterar este valor.
Taxa de fotogramas | Fotogramas por segundo do vídeo.
Fragmentos   | Os metadados são cortados em diferentes partes mais pequenas chamadas fragmentos. Cada fragmento contém um início, duração, número de intervalos e evento(s).
Iniciar   | Hora de início do primeiro evento, em tiques.
Duração |  Duração do fragmento, em tiques.
Intervalo |  Duração de cada evento no fragmento, em tiques.
Eventos  | Matriz de eventos. A matriz externa representa um intervalo de tempo. A matriz interna é constituída por 0 ou mais eventos que ocorreram nesse ponto no tempo.
windowFaceDistribution |    Percentagem de rostos que terão uma determinada emoção durante o evento.
windowMeanScores |  Pontuações médias para cada emoção dos rostos na imagem.

O motivo de formatar o JSON desta forma é configurar as APIs para cenários futuros, onde será importante recuperar metadados rapidamente e gerir um grande fluxo de resultados. Esta formatação está a utilizar ambas as técnicas de fragmentação (o que permite dividir os metadados em partes baseadas no tempo, em que pode transferir apenas o que precisa) e segmentação (o que permite dividir os eventos se ficarem demasiado grandes). Alguns cálculos simples podem ajudá-lo a transformar os dados. Por exemplo, se um evento iniciou em 6300 (tiques), com uma escala temporal de 2997 (tiques/segundo) e uma taxa de fotogramas de 29,97 (fotogramas/segundo):

*   Início/Escala temporal = 2,1 segundos
*   Segundos x (Taxa de fotogramas/Escala temporal) = 63 fotogramas

Segue-se um exemplo simples de extração do JSON para um formato por fotograma para controlo e deteção de rostos:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Como as emoções são suavizadas ao longo do tempo, se alguma vez criar uma visualização para sobrepor os resultados no vídeo original, subtraia 250 milissegundos dos carimbos de data/hora fornecidos.

### <a name="Summary">Resumo</a>
Neste guia, ficou a conhecer as funcionalidades da API de Emoções para Vídeo: como pode carregar um vídeo, verificar o respetivo estado e obter os metadados de reconhecimento de emoções.

Para obter mais informações sobre os detalhes da API, veja o guia de referência da API "[Referência da API de Emoções para Vídeo](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)".
