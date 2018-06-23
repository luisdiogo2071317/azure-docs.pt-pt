---
title: Chamar a API de emoções para as vídeo | Microsoft Docs
description: Saiba como chamar a API de emoções para as vídeo em serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 0875013b2061a84e3e23ae90c1106382672fdca6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352447"
---
# <a name="how-to-call-emotion-api-for-video"></a>Como chamar emoções API de vídeo

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este guia demonstra como chamar a API de emoções de vídeo. Os exemplos são escritos em c# utilizando a API de emoções da biblioteca de cliente de vídeo.

### <a name="Prep">Preparação</a> 
Para utilizar a API de emoções para as vídeo, é necessário um vídeo que inclui as pessoas, vídeos, de preferência, em que as pessoas estão a enfrentar a câmara.

### <a name="Step1">Passo 1: Autorizar a chamada de API</a> 
Cada chamada à API do emoções para as vídeo requer uma chave de subscrição. Esta chave tem de ser transmitidos através de um parâmetro de cadeia de consulta ou especificado no cabeçalho do pedido. Para passar a chave de subscrição através de uma cadeia de consulta, consulte o URL do pedido abaixo para a API de emoções de vídeo como exemplo:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Como alternativa, a chave de subscrição pode ser também especificada no cabeçalho do pedido HTTP:

```
ocp-apim-subscription-key: <Your subscription key>
```

Quando utilizar uma biblioteca de cliente, a chave de subscrição é transmitida através do construtor de classe VideoServiceClient. Por exemplo:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Para obter uma chave de subscrição, consulte [subscrições] (https://azure.microsoft.com/try/cognitive-services/). 

### <a name="Step2">Passo 2: Carregar um vídeo para o serviço e verificar o Estado</a>
É a forma mais básica Efetue qualquer uma da API de emoções para chamadas de vídeo através do carregamento de um vídeo diretamente. Isto é feito ao enviar um pedido de "POST" com o tipo de conteúdo application/octet-stream juntamente com os dados de ler a partir de um ficheiro de vídeo. O tamanho máximo do vídeo é 100MB.

Utilizar a biblioteca de clientes, é feita estabilização através do carregar mediante a transmissão num objecto stream. Consulte o exemplo abaixo:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Tenha em atenção que o método de CreateOperationAsync de VideoServiceClient é assíncrona. O método de chamada deverá ser marcado como async, bem como para utilizar a cláusula await.
Se as vídeo já se encontra na web e tem um URL público, API de emoções para as vídeo podem ser acedido ao fornecer o URL. Neste exemplo, o corpo do pedido irá ser uma cadeia JSON que contém o URL.

Utilizar a biblioteca de clientes, estabilização através de um URL pode facilmente ser executada utilizando outra sobrecarga do método CreateOperationAsync.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Este método de carregamento será a mesma para todas as API de emoções para chamadas de vídeo. 

Assim que tenha carregado um vídeo, a próxima operação de que pretende executar é Verifique o respetivo estado. Porque os ficheiros de vídeo são normalmente maiores e mais diversificada que outros ficheiros, os utilizadores podem esperar algum tempo de processamento neste passo. O tempo depende do tamanho e o comprimento do ficheiro.

Utilizar a biblioteca de clientes, pode obter o estado da operação e os resultados utilizando o método GetOperationResultAsync.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Normalmente, no lado do cliente deve obter periodicamente o estado da operação enquanto o estado é mostrado como "Com êxito" ou "Falhado".

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

Quando o estado de VideoOperationResult é apresentado como "Foi concluída com êxito" o resultado pode ser obtido pelo conversão VideoOperationResult para um VideoOperationInfoResult<VideoAggregateRecognitionResult> e o campo de ProcessingResult a aceder.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Passo 3: Obter e compreender o reconhecimento de emoções e ao controlar a saída JSON</a>

O resultado de saída contém os metadados do faces dentro do ficheiro especificado no formato JSON.

Conforme explicado no passo 2, a saída JSON está disponível no campo ProcessingResult de OperationResult, quando o estado é apresentado como "Com êxito".

A deteção de rostos em e controlo JSON inclui os seguintes atributos:

Atributo | Descrição
-------------|-------------
Versão | Refere-se para a versão da API emoções para JSON de vídeo.
Escala temporal | "Ticks" por segundo do vídeo.
Desvio  |O desvio da hora para carimbos de hora. Na versão 1.0 do emoções API para vídeos, este será sempre 0. Cenários suportados futuras, pode alterar este valor.
Framerate | Fotogramas por segundo do vídeo.
Fragmentos   | Os metadados é cortar cópias de segurança em diferentes partes mais pequenas denominadas fragmentos. Cada fragmento contém um início, a duração, o número do intervalo e o evento (s).
Iniciar   | A hora de início do primeiro evento, na ticks.
Duração |  O comprimento do fragmento, no ticks.
Intervalo |  O comprimento de cada evento dentro de fragmento, no ticks.
Eventos  | Uma matriz de eventos. A matriz externa representa um intervalo de tempo. A matriz interna é constituído por 0 ou mais eventos que ocorreram neste ponto no tempo.
windowFaceDistribution |    Percentagem enfrenta ter uma determinado emoções durante o evento.
windowMeanScores |  Média pontuações das classificações para cada emoções de faces na imagem.

O motivo para a formatação JSON desta forma consiste em configurar as APIs para cenários de futuros, onde vai ser importante obter metadados rapidamente e gerir um grande fluxo de resultados. Este formatação está a utilizar ambas as técnicas de fragmentação (permitindo-lhe dividir os metadados em partes baseados no tempo, onde pode transferir apenas o que precisa de) e a segmentação de (, permitindo-lhe dividir os eventos se demasiado grandes). Alguns cálculos simples podem ajudar a transformar os dados. Por exemplo, se um evento foi iniciado no 6300 (ticks), com uma escala temporal de 2997 (ticks/seg) e um framerate de 29.97 (fotogramas por segundo), em seguida:

*   Iniciar/escala temporal = segundos 2.1
*   Segundos x (Framerate/escala temporal) = 63 frames

Segue-se um exemplo simples para extrair o JSON para um por formato do pacote para a deteção de rostos em e controlo:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Porque emotions são smoothed ao longo do tempo, se alguma vez criar uma visualização de sobreposição os resultados na parte superior o vídeo original, subtrair 250 milissegundos dos carimbos fornecidos.

### <a name="Summary">Resumo</a>
Neste guia aprendeu sobre as funcionalidades da API emoções para as vídeo: a forma como pode carregar um vídeo, verifique o respetivo estado, obter metadados de reconhecimento de emoções.

Para obter mais informações sobre os detalhes de API, consulte o guia de referência de API "[emoções API para referência de vídeo](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)".
