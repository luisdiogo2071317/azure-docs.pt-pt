---
title: "Melhores práticas para as funções do Azure | Microsoft Docs"
description: "Saiba as melhores práticas e padrões para as funções do Azure."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "as funções do Azure, padrões, melhor prática, funções, processamento, webhooks, computação dinâmica, arquitetura sem servidor de eventos"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8088a8a83bcaefce17ac2756360a46119c8eb27
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Otimizar o desempenho e fiabilidade das funções do Azure

Este artigo fornece orientação para melhorar o desempenho e fiabilidade dos seus [sem servidor](https://azure.microsoft.com/overview/serverless-computing/) funcionar aplicações. 

## <a name="general-best-practices"></a>Melhores práticas gerais

Seguem-se as melhores práticas em como compilar e architect as suas soluções sem servidor através das funções do Azure.

### <a name="avoid-long-running-functions"></a>Evitar a execução longa funções

As funções de grandes dimensões, de execução longa podem causar problemas de tempo limite inesperado. Uma função pode ficar grande devido à grande quantidade de dependências do Node.js. Importar dependências também pode provocar tempos de aumento de carga que resultam de tempos limite inesperados. As dependências são carregadas ambos explicitamente e implicitamente. Um módulo único carregado pelo seu código pode carregar os seus próprios módulos adicionais.  

Sempre que possível, as funções de grande refactorize para a função de menor define que funcionam em conjunto e devolver respostas rápida. Por exemplo, um webhook ou uma função de Acionador HTTP exijam uma resposta de confirmação dentro de um determinado limite de tempo; é comum para webhooks para exigir uma resposta imediata. Pode passar o payload de Acionador HTTP para uma fila para ser processado por uma função de Acionador de fila. Esta abordagem permite-lhe diferir o trabalho real e devolver uma resposta imediata.


### <a name="cross-function-communication"></a>Cruzada comunicação de função

[Funções duráveis](durable-functions-overview.md) e [Azure Logic Apps](../logic-apps/logic-apps-overview.md) incorporada para gerir transições de estado e a comunicação entre várias funções.

Se não utilizar funções durável ou Logic Apps para integrar com várias funções, normalmente, é uma melhor prática para utilizar as filas de armazenamento para cruzada comunicação de função.  O principal motivo é que as filas de armazenamento são mais barata e muito mais fácil para aprovisionar. 

As mensagens individuais numa fila de armazenamento são limitadas de tamanho para 64 KB. Se precisar de transmitir mensagens maior entre as funções, uma fila pode ser utilizada para a mensagem de suporte do Azure Service Bus tamanhos até 256 KB no escalão Standard e até 1 MB no escalão Premium.

Tópicos do Service Bus são úteis se necessitar de filtragem antes do processamento de mensagens.

Os Event hubs são úteis para suportarem comunicações de elevado volume.


### <a name="write-functions-to-be-stateless"></a>Escrever funções ser sem monitorização de estado 

As funções devem ser sem monitorização de estado e idempotent se possível. Associe as informações de estado necessária com os seus dados. Por exemplo, uma ordem a ser processado provavelmente teria um associados `state` membro. Uma função foi possível processar uma ordem em que estado enquanto a função próprio permanece sem monitorização de estado. 

Funções de Idempotent especialmente são recomendadas com acionadores do temporizador. Por exemplo, se tiver um problema que absolutamente necessário executar uma vez por dia, gravá-lo, pode executar qualquer altura durante o dia com os mesmos resultados. A função pode sair quando não existe nenhum trabalho durante um dia específico. Também se anterior falha na execução para concluir, deve escolher a próxima execução onde parou.


### <a name="write-defensive-functions"></a>Escrever funções defesas

Assuma que a função foi possível encontrar uma exceção em qualquer altura. Estruture as suas funções com a capacidade para continuar a partir de um ponto de falha anterior durante a execução seguinte. Considere um cenário que requer as seguintes ações:

1. Consulta para 10 000 linhas numa base de dados.
2. Criar uma mensagem de fila para cada um desses linhas a processar mais pendente a linha.
 
Dependendo de como complexas é o sistema, poderá ter: envolvidos serviços a jusante comportam incorretamente, falhas de rede ou quota limita atingiu o número, etc. Todos estes podem afetar a sua função em qualquer altura. Terá de criar as suas funções para estar preparado para o mesmo.

Como o código reagir se ocorrer uma falha depois de inserir 5.000 dos itens para uma fila para processamento? Controlar itens num conjunto de que concluiu. Caso contrário, poderá insira-as novamente a próxima vez. Isto pode ter um impacto grave no seu fluxo de trabalho. 

Se um item de fila já foi processado, permitir que a função ser sem operações.

Tire partido das medidas defesas já são fornecidos para os componentes que utilizar na plataforma as funções do Azure. Por exemplo, consulte **processamento de mensagens de filas nocivas** da documentação para [fila de armazenamento do Azure acionadores e enlaces](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="scalability-best-practices"></a>Melhores práticas de escalabilidade

Há uma série de fatores que afetam a forma como dimensionar as instâncias da sua aplicação de função. Os detalhes são fornecidos na documentação de [função dimensionamento](functions-scale.md).  Seguem-se algumas melhores práticas para garantir a escalabilidade ideal de uma aplicação de função.

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Não misture o código de teste e produção na mesma aplicação de função

As funções dentro de uma aplicação de função partilham recursos. Por exemplo, a memória é partilhada. Se estiver a utilizar uma aplicação de função na produção, não adicione funções relacionadas com o teste e de recursos ao mesmo. Pode causar overhead inesperado durante a execução de código de produção.

Seja cuidadoso ao carregar nas suas aplicações de função de produção. Memória é considerando cada função na aplicação.

Se tiver uma assemblagem partilhada referenciada em várias funções de .net, coloque-o numa pasta partilhada comuns. Referência a assemblagem com uma instrução semelhante ao seguinte exemplo, se utilizar c# Scripts (.csx): 

    #r "..\Shared\MyAssembly.dll". 

Caso contrário, é fácil acidentalmente implementar várias versões de teste do binário mesmo que se comportam de forma diferente entre funções.

Não utilize o registo verboso no código de produção. Tem um impacto negativo no desempenho.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Utilizar código async mas evitar bloquear chamadas

Programação assíncrona é uma prática recomendada. No entanto, evite sempre a referenciar o `Result` propriedade ou chamar `Wait` método num `Task` instância. Esta abordagem pode levar ao esgotamento de thread.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Receber mensagens no batch sempre que possível

Alguns acionadores como o Hub de eventos permitem receção de um lote de mensagens de uma invocação único.  Mensagens de criação de batches tem um melhor desempenho.  Pode configurar o tamanho de lote máximo no `functions.json` ficheiro conforme detalhado no [host.json documentação de referência](functions-host-json.md)

Para funções de c# pode alterar o tipo de uma matriz de tipo seguro.  Por exemplo, em vez de `EventData sensorEvent` a assinatura de método pode ser `EventData[] sensorEvent`.  Para outros idiomas, terá de definir a propriedade cardinality explicitamente seu `function.json` para `many` para activar a criação de batches [conforme mostrado aqui](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10).

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Configurar os comportamentos de anfitrião para processar melhor simultaneidade

O `host.json` ficheiro na aplicação de função permite a configuração de comportamentos de tempo de execução e acionamento do anfitrião.  Para além de comportamentos de criação de batches, pode gerir simultaneidade de um número de acionadores.  Muitas vezes, ajuste os valores existentes nestas opções pode ajudar a escala cada instância adequadamente para as exigências das funções invocadas.

As definições no ficheiro de anfitriões aplicam a todas as funções dentro da aplicação, dentro um *única instância* da função. Por exemplo, se tiver uma aplicação de função com 2 funções HTTP e pedidos simultâneos definida como 25, um pedido para o acionador HTTP seria contam para 25 partilhados pedidos em simultâneo.  Se essa aplicação de função ampliada para 10 instâncias, as 2 funções eficazmente permitiria 250 pedidos em simultâneo (10 instâncias * 25 pedidos em simultâneo por instância).

**Opções de anfitrião de simultaneidade HTTP**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Podem encontrar outras opções de configuração de anfitrião [no documento de configuração de anfitrião](functions-host-json.md).

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte os seguintes recursos:

Uma vez que as funções do Azure utiliza o App Service do Azure, deve também estar ciente de diretrizes de serviço de aplicações.
* [Padrões e práticas otimizações de desempenho de HTTP](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
