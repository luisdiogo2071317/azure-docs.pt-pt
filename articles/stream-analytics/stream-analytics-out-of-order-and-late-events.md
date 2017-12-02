---
title: Tratamento de ordem de eventos e lateness Azure Stream Analytics | Microsoft Docs
description: Saiba mais sobre como funciona o Stream Analytics com eventos fora de ordem ou enlace tardio em fluxos de dados.
keywords: fora de ordem, eventos
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: 208dfa14d5d18e106d654539cd80bafdeb90cdf8
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Consideração de ordem de eventos do Azure Stream Analytics

## <a name="understand-arrival-time-and-application-time"></a>Compreenda a hora da chegada e hora da aplicação.

Um fluxo de dados temporal de eventos, cada evento é atribuído a um carimbo. O Azure Stream Analytics atribui timestamp para cada evento utilizando a hora da chegada ou hora da aplicação. A coluna "Timestamp" tem o timestamp atribuído ao evento. Hora da chegada é atribuída na origem de entrada quando o evento chega a origem. Hora da chegada é EventEnqueuedTime para a entrada do Hub de eventos e [blob hora da última modificação](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) para a entrada de blob. Hora da aplicação é atribuída quando o evento é gerado e faz parte do payload. Processar os eventos por hora da aplicação, utilize a cláusula "Timestamp por" a consulta select. Se a cláusula "Timestamp por" estiver ausente, são processados por hora da chegada de eventos. Hora da chegada pode ser acedida utilizando a propriedade de EventEnqueuedTime para o hub de eventos e utilizando a propriedade de BlobLastModified para a entrada de blob. O Azure Stream Analytics produz saída de ordem timestamp e fornece algumas definições para lidar com dados fora de ordem.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Azure Stream Analytics processamento de vários fluxos

Tarefa de Stream Analytics do Azure combina eventos a partir de várias linhas cronológicas em alguns casos, incluindo,

* Produzir a saída de várias partições. As consultas que não tenham um explícita "partição por PartitionId" teria combinar eventos a partir de todas as partições.
* União de dois ou mais origens de entrada diferentes.
* Associar origens de entrada.

Em cenários onde são combinadas várias linhas cronológicas, Azure Stream Analytics produzirá um resultado para um carimbo *t1* apenas depois de todas as origens que estão combinadas, pelo menos, momento *t1*.
Por exemplo, se a consulta que lê a partir de um *Hub de eventos* partição tem duas partições e uma partição *P1* tem eventos até à hora *t1* e outra partição  *P2* tem eventos até à hora *t1 + x*, saída é produzida até à hora *t1*.
No entanto se tiver ocorrido explícita *"Partição por PartitionId"* cláusula, ambas as partições de forma independente avança.
Enlace tardio tolerância de chegada definição é utilizada para lidar com a ausência de dados em algumas partições.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Configuração de enlace tardio tolerância de chegada e tolerância fora de ordem
Fluxos de entrada que não estão na ordem são:
* Ordenada (e, por isso **atrasada**).
* Ajustada pelo sistema, de acordo com as políticas de utilizador especificado.

Do Stream Analytics tolerates eventos de enlace tardio e fora de ordem durante o processamento por **hora da aplicação**. As seguintes definições estão disponíveis no **eventos ordenação** opção no portal do Azure: 

![Processamento de eventos do Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

**Tolerância de chegada dinâmico**
* Esta definição é aplicável apenas durante o processamento do tempo de aplicação, caso contrário será ignorada.
* Esta é a diferença máxima entre a hora da chegada e hora da aplicação. Se a hora da aplicação é antes (hora da chegada - tarde chegada janela), é definido como (hora da chegada - tarde chegada janela)
* Quando várias partições do mesmo fluxo de entrada ou de vários fluxos de entrada são combinados em conjunto, o enlace tardio tolerância de chegada é a quantidade máxima de tempo que cada partição espera novos dados. 

Resumidamente, a janela de chegada de enlace tardio é o máximo de atraso entre a geração de eventos e de receção do evento na origem de entrada.
Ajuste com base na tolerância de chegada de enlace tardio é efetuada pela primeira vez e fora de ordem é feito seguinte. O **Timestamp** coluna terão o final timestamp atribuído para o evento.

**Tolerância fora de ordem**
* Os eventos que chegam desordenados mas dentro do conjunto de "período de tolerância fora de ordem" são **reordenados por timestamp**. 
* Os eventos que chegam posterior tolerância são **removida ou ajustado**.
    * **Tendo em conta**: tendo em conta parece ter chegou o momento mais recente aceitável. 
    * **Removido**: rejeitados.

Para reordenar os eventos recebidos "período de tolerância fora de ordem", o resultado da consulta é **atrasada pelo período de tolerância fora de ordem**.

**Exemplo**

* A tolerância de chegada enlace tardio = 10 minutos<br/>
* Fora de tolerância a ordem = 3 minutos<br/>
* Processamento do tempo de aplicação<br/>
* Eventos:
   * Evento 1 _hora da aplicação_ = 00:00:00, _hora da chegada_ = 00:10:01, _Timestamp_ = 00:00:01, tendo em conta porque (_hora da chegada_  -  _Hora da aplicação_) é maior do que a tolerância de chegada enlace tardio.
   * Evento 2 _hora da aplicação_ = 00:00:01 _hora da chegada_ = 00:10:01, _Timestamp_ = 00:00:01, não tendo em conta porque o tempo de aplicação é entre a chegada de enlace tardio janela.
   * Evento 3 _hora da aplicação_ = 00:10:00, _hora da chegada_ = 00:10:02, _Timestamp_ = 00:10:00, não tendo em conta porque o tempo de appliation está dentro da janela de chegada de enlace tardio .
   * Evento 4 _hora da aplicação_ = 00:09:00, _hora da chegada_ = 00:10:03, _Timestamp_ = 00:09:00, aceite com original timestamp como hora da aplicação está dentro do limite de tolerância de ordem.
   * Evento 5 _hora da aplicação_ = 00:06:00, _hora da chegada_ = 00:10:04, _Timestamp_ = 00:07:00, tendo em conta porque a hora da aplicação é mais antiga do que o fora de ordem tolerância.

## <a name="practical-considerations"></a>Considerações práticas
Tal como mencionado acima, *tolerância de chegada de enlace tardio* é a diferença entre a hora da aplicação e a hora da chegada máxima.
Também quando processamento pela aplicação de tempo, os eventos que são posteriores à configurada *tolerância de chegada de enlace tardio* são tendo em conta antes do *tolerância fora de ordem* definição é aplicada. Por isso, eficaz fora de ordem é o mínimo de enlace tardio tolerância de chegada e tolerância fora de ordem.

Eventos fora de ordem dentro de um fluxo ocorrem motivos, incluindo,
* Desfasamento de relógio entre os remetentes.
* Latência variável entre remetente e origem de evento de entrada.

Chegada de enlace tardio acontece por motivos, incluindo,
* Os remetentes de lote e enviar eventos para um intervalo mais tarde, após o intervalo.
* Latência entre a enviar o evento pelo remetente e recebe o evento na origem de entrada.

Ao configurar *tolerância de chegada de enlace tardio* e *tolerância fora de ordem* para uma tarefa específica, que está correto, requisitos de latência e acima fatores devem ser considerados.

Seguem-se alguns exemplos

### <a name="example-1"></a>Exemplo 1: 
Consulta tem a cláusula "Partição por PartitionId" e dentro de uma única partição, os eventos são enviados através de métodos de envio síncronas. Bloco de métodos de envio síncrona até que os eventos são enviados.
Neste caso, fora de ordem for zero porque os eventos são enviados pela ordem com confirmação explícita antes de enviar o evento seguinte. Chegada de enlace tardio é um atraso máximo entre o evento a gerar e enviar o evento + latência máxima entre o remetente e origem de entrada

### <a name="example-2"></a>Exemplo 2:
Consulta tem a cláusula "Partição por PartitionId" e dentro de uma única partição, os eventos são enviados utilizando o método de envio assíncrono. Métodos de envio assíncrono podem iniciar vários envia ao mesmo tempo, que pode fazer com que os eventos fora de ordem.
Neste caso, a chegada de fora de ordem e enlace tardio são atraso máximo, pelo menos, entre o evento a gerar e enviar o evento + latência máxima entre o remetente e origem de entrada.

### <a name="example-3"></a>Exemplo 3:
Consulta não tem "Partição por PartitionId" e não existirem, pelo menos, duas partições.
Configuração de é igual ao exemplo 2. No entanto, ausência de uma das partições de dados pode atrasar o resultado pelo adicional * tolerância de chegada de enlace tardio "janela.

## <a name="to-summarize"></a>Para resumir
* Enlace tardio tolerância de chegada e janela fora de ordem devem ser configurados com base no que está correto, requisitos de latência e também devem considerar a forma como os eventos são enviados.
* Recomenda-se que é inferior a tolerância de chegada de enlace tardio tolerância fora de ordem.
* Ao combinar várias linhas cronológicas, falta de um dos partições ou origens de dados pode atrasar a saída por um período de tolerância de chegada enlace tardio adicionais.

## <a name="get-help"></a>Obter ajuda
Para obter assistência adicional, experimente a nossa [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos seguintes
* [Introdução ao Stream Analytics](stream-analytics-introduction.md)
* [Introdução ao Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Dimensionar tarefas do Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência de linguagem de consulta do Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência da REST API de gestão de análise de fluxo](https://msdn.microsoft.com/library/azure/dn835031.aspx)
