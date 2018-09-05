---
title: Guia de resolução de problemas para o Azure Stream Analytics
description: Este artigo descreve técnicas para solucionar suas tarefas do Azure Stream Analytics, ligações, entradas, saídas, consultas e dados.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b1b5d0af3f2b149959bcb97ddaf29ba2fe1f4668
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702226"
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Guia de resolução de problemas para o Azure Stream Analytics

Resolução de problemas do Stream Analytics do Azure pode parecer um esforço complexo à primeira vista. Depois de trabalhar com muitos usuários, criámos este guia para ajudar a simplificar o processo e remover as suposições sobre sua entradas, saídas, consultas e as funções.

## <a name="troubleshoot-your-stream-analytics-job"></a>Resolver problemas relacionados com a tarefa de Stream Analytics

Para obter melhores resultados na solução de problemas a tarefa de Stream Analytics, utilize as seguintes diretrizes:

1.  Teste a conectividade:
    - Verifique a conectividade com entradas e saídas ao utilizar o **Testar ligação** botão para cada entrada e saída.

2.  Examine os dados de entrada:
    - Para verificar que os dados de entrada é fluir para o Hub de eventos, utilize [Explorador do Service Bus](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) para ligar ao Hub de eventos do Azure (se for utilizada a entrada do Hub de eventos).  
    - Utilize o [ **dados de exemplo** ](stream-analytics-sample-data-input.md) botão para cada entrada e transferir os dados de exemplo de entrada.
    - Inspecionar os dados de exemplo para compreender a forma dos dados: o esquema e o [tipos de dados](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Teste a sua consulta:
    - Sobre o **consulta** separador, utilize o **testar** botão para testar a consulta e utilize os dados de exemplo transferidos para [testar a consulta](stream-analytics-test-query.md). Examine os erros e tentar corrigi-las.
    - Se usar [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), verifique se os eventos têm carimbos maior do que o [hora de início da tarefa](stream-analytics-out-of-order-and-late-events.md).

4.  Depure a sua consulta:
    - Reconstrua a consulta progressivamente desde a instrução select simple até agregados mais complexos utilizando passos. Para criar a lógica de consulta passo a passo, utilize [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) cláusulas.
    - Uso [SELECT INTO](stream-analytics-select-into.md) para depurar passos da consulta.

5.  Elimine armadilhas comuns, tais como:
    - R [ **onde** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) cláusula na consulta filtrou todos os eventos, impedindo que qualquer saída a ser gerado.
    - R [ **CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) falhar, fazendo com que a tarefa efetuar a ativação de função. Para evitar falhas de conversão de tipo, utilize [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) em vez disso.
    - Quando utiliza as funções de janela, aguarde que a duração de janela inteira ver o resultado da consulta.
    - O carimbo de hora para eventos precede a hora de início de tarefa e, portanto, os eventos estão a ser ignorados.

6.  Utilize a ordenação de eventos:
    - Se todos os passos anteriores, funcionou bem, vá para o **configurações** painel e selecione [ **ordenação de eventos**](stream-analytics-out-of-order-and-late-events.md). Certifique-se de que esta política está configurada para o que faz sentido no seu cenário. A política é *não* aplicada ao utilizar o **testar** botão para testar a consulta. O resultado é uma diferença entre testar no browser em oposição a executar a tarefa em produção.

7.  Depure pelo uso de métricas:
    - Se não obter nenhuma saída após a duração esperada (com base na consulta), experimente o seguinte:
        - Examinar [ **métricas de monitorização** ](stream-analytics-monitoring.md) sobre o **Monitor** separador. Uma vez que os valores são agregados, as métricas são atrasadas por alguns minutos.
            - Se eventos de entrada > 0, a tarefa é capaz de ler dados de entrada. Se os eventos de entrada não > 0, em seguida:
                - Para ver se a origem de dados tem dados válidos, verifique-lo usando [Explorador do Service Bus](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Esta verificação aplica-se se a tarefa estiver a utilizar o Hub de eventos como entrada.
                - Verifique se o formato de serialização de dados e, em seguida, codificação de dados são conforme o esperado.
                - Se a tarefa está a utilizar um Hub de eventos, verifique se o corpo da mensagem está *nulo*.
            - Se os erros de conversão de dados > 0 e aumentarem, poderá acontecer o seguinte:
                - A tarefa poderá não conseguir anular a serialização de eventos.
                - O esquema de eventos pode não corresponder ao esquema definido ou esperado dos eventos na consulta.
                - Os tipos de dados de alguns dos campos de eventos podem não corresponder às expetativas.
            - Se os erros de tempo de execução > 0, significa que a tarefa pode receber os dados, mas está a gerar erros ao processar a consulta.
                - Para localizar os erros, vá para o [registos de auditoria](../azure-resource-manager/resource-group-audit.md) e filtre *falha* estado.
            - Se InputEvents > 0 e OutputEvents = 0, isso significa que um dos seguintes é verdadeira:
                - O processamento da consulta resultou em zero eventos de saída.
                - Eventos ou os respetivos campos a poderão estar incorreto, resultando em zero saídas após o processamento de consulta.
                - A tarefa não foi possível enviar dados para o [sink de saída](stream-analytics-select-into.md) por motivos de conectividade ou autenticação.
        - Em todos os casos de erro mencionado anteriormente, as mensagens de registo de operações explicam os detalhes adicionais (incluindo o que está a acontecer), exceto em casos em que a lógica de consulta filtrou todos os eventos. Se o processamento de vários eventos gerar erros, o Stream Analytics regista as primeiras mensagens de três erro do mesmo tipo em 10 minutos nos registos de operações. Em seguida, suprime os erros idênticos adicionais com uma mensagem que diz "Erros estavam a acontecer demasiado depressa, que estes estão a ser suprimidos."

8. Depure com auditoria e registos de diagnóstico:
    - Uso [registos de auditoria](../azure-resource-manager/resource-group-audit.md)e filtre para identificar e depurar erros.
    - Uso [registos de diagnóstico da tarefa](stream-analytics-job-diagnostic-logs.md) para identificar e depurar erros.

9. Examine as saídas:
    - Quando o estado da tarefa é *em execução*, consoante a duração estipulada na consulta, pode ver a saída da origem de dados de sink.
    - Se não é possível ver saídas que vão para um tipo de saída específico, redirecioná-los para um tipo de saída que seja menos complexo, por exemplo, um Blob do Azure. Ao utilizar o Explorador de armazenamento, verifique se a saída pode ser vista. Além disso, verifique se limites de limitação a saída estão a impedir o dados de que está sendo recebido.

10. Utilize a análise de fluxo de dados com as métricas de diagrama da tarefa:
    - Para analisar o fluxo de dados e identificar problemas, utilize o [diagrama de tarefas com a métrica](stream-analytics-job-diagram-with-metrics.md).

11. Abra um pedido de suporte:
    - Por fim, se tudo o resto falhar, abra um incidente de suporte da Microsoft utilizando o SubscriptionID que contém o seu trabalho.

## <a name="get-help"></a>Obter ajuda

Para obter assistência, tente nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
