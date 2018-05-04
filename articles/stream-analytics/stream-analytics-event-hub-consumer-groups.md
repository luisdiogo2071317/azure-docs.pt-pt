---
title: Resolver problemas de recetores do Hub de eventos no Azure Stream Analytics
description: Este artigo descreve como utilizar vários grupos de consumidores para entradas de Event Hubs nas tarefas do Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: aaa8c4e8d273b44f453d3f63f0be1d4baf980649
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="troubleshoot-event-hub-receivers-in-azure-stream-analytics"></a>Resolver problemas de recetores do Hub de eventos no Azure Stream Analytics

Pode utilizar Event Hubs do Azure no Azure Stream Analytics para inserção ou dados de uma tarefa de saída. Uma melhor prática para utilizar Event Hubs é utilizar vários grupos de consumidores, para garantir a escalabilidade de tarefa. Uma razão é que o número de leitores na tarefa de Stream Analytics para uma introdução específica afetará o número de leitores num grupo de consumidores único. O número exatos de recetores baseia-se nos detalhes de implementação interno para a lógica da topologia de escalamento horizontal. O número de recetores não está exposto externamente. O número de leitores pode alterar a hora de início da tarefa ou durante as atualizações de tarefa.

O erro apresentado quando o número de recetores excede o máximo é: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Quando o número de leitores for alterada durante uma atualização de tarefa, avisos transitórios são escritos para registos de auditoria. Tarefas do Stream Analytics recuperar automaticamente a partir destes problemas transitórios.

## <a name="add-a-consumer-group-in-event-hubs"></a>Adicionar um grupo de consumidores de Event Hubs
Para adicionar um novo grupo de consumidores na sua instância de Event Hubs, siga estes passos:

1. Inicie sessão no Portal do Azure.

2. Localize os Hubs de eventos.

3. Selecione **Event Hubs** sob o **entidades** cabeçalho.

4. Selecione o Hub de eventos por nome.

5. No **instância de Hubs de eventos** página o **entidades** cabeçalho, selecione **grupos de consumidores**. Um grupo de consumidores com o nome **$Default** está listado.

6. Selecione **+ o grupo de consumidores** para adicionar um novo grupo de consumidores. 

   ![Adicionar um grupo de consumidores de Event Hubs](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Quando tiver criado a entrada na tarefa de Stream Analytics para apontar para o Hub de eventos, que especificou existe o grupo de consumidores. $Default é utilizado quando não for especificado nenhum. Depois de criar um novo grupo de consumidores, edite a entrada do Hub de eventos na tarefa de Stream Analytics e especifique o nome do novo grupo de consumidores.


## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Número de leitores por partição excede o limite de Event Hubs cinco

Se a sintaxe da consulta de transmissão em fluxo referencia o mesmo recurso de Hub de eventos de entrada várias vezes, o motor de trabalho pode utilizar vários leitores por consulta desse mesmo grupo de consumidores. Quando existem demasiadas referências para o mesmo grupo de consumidores, a tarefa pode exceder o limite de cinco e emitida um erro. Nesses casos, ainda mais pode dividir através da utilização de várias entradas em vários grupos de consumidores utilizando a solução descrita na secção seguinte. 

Cenários em que o número de leitores por partição excede o limite de Event Hubs cinco incluem o seguinte:

* Múltiplas instruções SELECIONADAS: Se utilizar várias instruções que se referem à **mesmo** hub de eventos de entrada, cada instrução SELECIONADA faz com que um recetor novo a ser criado.
* UNION: Ao utilizar uma União, é possível ter várias entradas que se referem para o **mesmo** grupo hub e consumidores de eventos.
* ASSOCIAÇÃO automática: Ao utilizar uma operação de associação automática, é possível consultar o **mesmo** hub de eventos várias vezes.

## <a name="solution"></a>Solução

As seguintes melhores práticas podem ajudar a mitigar cenários em que o número de leitores por partição excede o limite de Event Hubs cinco.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Dividir a sua consulta em vários passos, utilizando uma cláusula WITH

A cláusula WITH Especifica um conjunto de resultados com nome temporária que pode ser referenciado por uma cláusula FROM na consulta. É possível definir a cláusula WITH no âmbito da execução de uma única instrução SELECT.

Por exemplo, em vez desta consulta:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Utilize esta consulta:

```
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Certifique-se de que as entradas vincular a grupos de consumidores diferentes

Para consultas no qual os três ou mais entradas estão ligadas ao mesmo grupo de consumidores de Event Hubs, crie grupos de consumidores separado. Isto requer a criação de entradas de Stream Analytics adicionais.


## <a name="next-steps"></a>Passos Seguintes
* [Dimensionar tarefas do Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência de linguagem de consulta do Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
