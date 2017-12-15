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
ms.openlocfilehash: 71929b449f2a0fa55327fd3f9741208506859e85
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="azure-stream-analytics-event-order-considerations"></a>Considerações de ordem de eventos do Azure Stream Analytics

## <a name="arrival-time-and-application-time"></a>Hora da chegada e hora da aplicação

Um fluxo de dados temporal de eventos, cada evento é atribuído a um carimbo de data / hora. O Azure Stream Analytics atribui um carimbo de hora para cada evento utilizando o tempo de chegada ou a hora da aplicação. O **Timestamp** coluna tem o carimbo de hora atribuído ao evento. 

Hora da chegada é atribuída na origem de entrada quando o evento chega a origem. Pode aceder a hora da chegada ao utilizar o **EventEnqueuedTime** propriedade de entrada do hub de eventos e utilizar o [BlobProperties.LastModified](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) propriedade para a entrada de blob. 

Hora da aplicação é atribuída quando o evento é gerado e faz parte do payload. Para processar os eventos por hora da aplicação, utilize o **Timestamp por** cláusula na consulta select. Se o **Timestamp por** cláusula está ausente, são processados eventos por hora da chegada. 

O Azure Stream Analytics produz saída de ordem de carimbo de hora e fornece as definições para lidar com dados fora de ordem.


## <a name="handling-of-multiple-streams"></a>Processamento de vários fluxos

Uma tarefa do Azure Stream Analytics combina eventos a partir de várias linhas cronológicas em casos como o seguinte:

* Produzir a saída de várias partições. As consultas que não tenham explícita **partição por PartitionId** cláusula têm de combinar eventos a partir de todas as partições.
* União de dois ou mais origens de entrada diferentes.
* Associar origens de entrada.

Em cenários onde são combinadas várias linhas cronológicas, o Azure Stream Analytics produz saída de carimbo de hora *t1* apenas depois de todas as origens que estão combinadas, pelo menos, momento *t1*. Por exemplo, suponha que a consulta lê a partir de uma partição do hub de eventos que tem duas partições. Uma das partições de *P1*, tem de eventos até à hora *t1*. A partição, *P2*, tem de eventos até à hora *t1 + x*. Saída é produzida, em seguida, até à hora *t1*. Mas se não houver explícita **partição por PartitionId** cláusula, ambas as partições de forma independente progresso.

A definição de enlace tardio tolerância de chegada é utilizada para lidar com a ausência de dados em algumas partições.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Configuração de enlace tardio tolerância de chegada e tolerância fora de ordem
Fluxos de entrada que não estão na ordem são:
* Ordenada (e, por conseguinte, atrasado)
* Ajustado pelo sistema, de acordo com as políticas de utilizador especificado

Do Stream Analytics tolerates eventos de enlace tardio e fora de ordem quando estiver a processar por hora da aplicação. As seguintes definições estão disponíveis no **eventos ordenação** opção no portal do Azure: 

![Processamento de eventos do Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

### <a name="late-arrival-tolerance"></a>Tolerância de chegada dinâmico
A tolerância de chegada enlace tardio é aplicável apenas quando está a processar por hora da aplicação. Caso contrário, a definição é ignorada.

A tolerância de chegada enlace tardio é a diferença máxima entre a hora da chegada e hora da aplicação. Se um evento chegar posterior a tolerância de chegada de enlace tardio (por exemplo, tempo de aplicação *app_t* < hora da chegada *arr_t* -enlace tardio tolerância de política de chegada *late_t*), o evento é ajustado para o número máximo de tolerância de chegada enlace tardio (*arr_t* - *late_t*). A janela de chegada de enlace tardio é o máximo de atraso entre a geração de eventos e de receção do evento na origem de entrada. 

Quando são combinados várias partições do mesmo fluxo de entrada ou de vários fluxos de entrada, o enlace tardio tolerância de chegada é a quantidade máxima de tempo que cada partição aguarda novos dados. 

Ajuste com base na tolerância de chegada de enlace tardio ocorre primeiro. Com base na tolerância fora de ordem de ajuste ocorre a seguinte. O **Timestamp** coluna tem o carimbo de hora final atribuído ao evento.

### <a name="out-of-order-tolerance"></a>Tolerância fora de ordem
Eventos que chegam desordenados mas dentro do período de tolerância de fora de ordem do conjunto são reordenados pelo carimbo de data / hora. Eventos que chegam posterior ao período de tolerância são:
* **Tendo em conta**: tendo em conta parece ter chegou o momento mais recente aceitável. 
* **Removido**: rejeitados.

Quando o Stream Analytics reordena eventos que são recebidos o período de tolerância fora de ordem, o resultado da consulta está atrasado, o período de tolerância fora de ordem.

### <a name="example"></a>Exemplo

* A tolerância de chegada enlace tardio = 10 minutos<br/>
* Tolerância fora de ordem = 3 minutos<br/>
* Processamento do tempo de aplicação<br/>
* Eventos:
   1. **Hora da aplicação** = 00:00:00, **hora da chegada** = 00:10:01, **Timestamp** = 00:00:01, tendo em conta porque (**hora da chegada - hora da aplicação**) é mais do que a tolerância de chegada de enlace tardio.
   2. **Hora da aplicação** = 00:00:01 **hora da chegada** = 00:10:01, **Timestamp** = 00:00:01, não tendo em conta porque a hora da aplicação é o período de chegada de enlace tardio.
   3. **Hora da aplicação** = 00:10:00, **hora da chegada** = 00:10:02, **Timestamp** = 00:10:00, não tendo em conta porque a hora da aplicação é o período de chegada de enlace tardio.
   4. **Hora da aplicação** = 00:09:00, **hora da chegada** = 00:10:03, **Timestamp** = 00:09:00, aceite com o carimbo de hora original, porque a hora da aplicação está dentro do fora de ordem tolerância.
   5. **Hora da aplicação** = 00:06:00, **hora da chegada** = 00:10:04, **Timestamp** = 00:07:00, tendo em conta porque a hora da aplicação é mais antiga que a tolerância fora de ordem.

### <a name="practical-considerations"></a>Considerações práticas
Conforme mencionado anteriormente, enlace tardio tolerância de chegada é a diferença entre a hora da aplicação e a hora da chegada máxima. Quando estiver a processar por hora da aplicação, eventos que são posteriores à tolerância de chegada de enlace tardio configurada são tendo em conta antes da definição de fora de ordem de tolerância é aplicada. Por isso, eficaz fora de ordem é o mínimo de enlace tardio tolerância de chegada e tolerância fora de ordem.

Razões para fora de ordem eventos dentro de um fluxo incluem:
* Relógio dissimetrias entre os remetentes.
* Latência variável entre o remetente e a origem de evento de entrada.

Por motivos de enlace tardio chegada incluem:
* Remetentes de criação de batches e o envio de eventos para um intervalo mais tarde, após o intervalo.
* Latência entre a enviar o evento pelo remetente e recebe o evento na origem de entrada.

Quando estiver a configurar o enlace tardio tolerância de chegada e tolerância fora de ordem para uma tarefa específica, considere correcção, requisitos de latência e os fatores anteriores.

Seguem-se alguns exemplos.

#### <a name="example-1"></a>Exemplo 1 
A consulta tem um **partição por PartitionId** cláusula. Dentro de uma única partição, os eventos são enviados através de métodos de envio síncronas. Bloco de métodos de envio síncrona até que os eventos são enviados.

Neste caso, fora de ordem for zero porque os eventos são enviados pela ordem com confirmação explícita antes do envio de evento seguinte. Chegada de enlace tardio é o atraso máximo entre o evento a gerar e enviar o evento, mais a latência máxima entre o remetente e a origem de entrada.

#### <a name="example-2"></a>Exemplo 2
A consulta tem um **partição por PartitionId** cláusula. Dentro de uma única partição, os eventos são enviados através de métodos de envio assíncrono. Métodos de envio assíncrono podem iniciar vários envia ao mesmo tempo, que pode fazer com que os eventos de fora de ordem.

Neste caso, a chegada de enlace tardio tanto fora de ordem, pelo menos, são o atraso máximo entre o evento a gerar e enviar o evento, mais a latência máxima entre o remetente e a origem de entrada.

#### <a name="example-3"></a>Exemplo 3
A consulta não tem um **partição por PartitionId** cláusula, e existem, pelo menos, duas partições.

A configuração é igual ao exemplo 2. No entanto, a ausência de uma das partições de dados pode atrasar a saída por um período de tolerância de chegada enlace tardio adicionais.

## <a name="handling-event-producers-with-differing-timelines"></a>Produtores de eventos de processamento com linhas cronológicas diferentes
Um fluxo de eventos de entrada único frequentemente contém eventos provenientes de vários produtores de evento, tais como os dispositivos individuais. Estes eventos poderão chegarem fora de ordem devido a razões debatidas anteriormente. Nestes cenários, embora disorder em produtores de evento poderá ser grande, disorder dentro de eventos a partir de um produtor único é pequeno (ou mesmo atribui).

O Azure Stream Analytics fornece gerais mecanismos para lidar com eventos fora de ordem. Resultado desses mecanismos atrasos de processamento (enquanto aguardava a existência de eventos straggling alcançar o sistema), remover ou ajustado, ou ambos.

Ainda em vários cenários, a consulta pretendida está a processar eventos de produtores de eventos diferentes independentemente. Por exemplo, este poderá ser agregar eventos por janela, por dispositivo. Nestes casos, não é necessário para atrasar a saída que corresponde ao produtor de um evento ao aguardar que os outros produtores de eventos detetar a cópia de segurança. Por outras palavras, é necessário para lidar com a hora dissimetrias entre produtores. Pode ignorá-lo.

Obviamente, isto significa que os eventos de saída próprios estão fora de ordem no que respeita à respetiva carimbos. O consumidor a jusante tem de ser capaz de lidar com esse comportamento. Mas cada evento na saída está correto.

O Azure Stream Analytics implementa esta funcionalidade utilizando o [TIMESTAMP BY OVER](https://msdn.microsoft.com/library/azure/mt573293.aspx) cláusula.

## <a name="summary"></a>Resumo
* Configure o enlace tardio tolerância de chegada e a janela de fora de ordem com base nos requisitos de latência e estão correctos. Considere também a forma como os eventos são enviados.
* Recomendamos que é inferior a tolerância de chegada de enlace tardio tolerância fora de ordem.
* Quando está a combinar várias linhas cronológicas, falta de um dos partições ou origens de dados pode atrasar a saída por um período de tolerância de chegada enlace tardio adicionais.

## <a name="get-help"></a>Obter ajuda
Para obter assistência adicional, experimente o [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos seguintes
* [Introdução ao Stream Analytics](stream-analytics-introduction.md)
* [Introdução ao Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Dimensionar tarefas do Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência de linguagem de consulta do Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência da REST API de gestão de análise de fluxo](https://msdn.microsoft.com/library/azure/dn835031.aspx)
