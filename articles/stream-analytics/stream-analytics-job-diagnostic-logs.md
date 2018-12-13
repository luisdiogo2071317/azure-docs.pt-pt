---
title: Resolver problemas com os registos de diagnóstico do Azure Stream Analytics
description: Este artigo descreve como analisar os registos de diagnóstico no Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: db3c9874676e3240f6896c1e1ff8f873360c20d5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090827"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Resolver problemas relacionados com o Azure Stream Analytics, utilizando os registos de diagnóstico

Ocasionalmente, uma tarefa do Azure Stream Analytics inesperadamente interrompe o processamento. É importante ser capaz de resolver esse tipo de evento. O evento poderá ser causado por um resultado de consulta inesperada, por conectividade a dispositivos ou por uma falha inesperada do serviço. Os registos de diagnóstico do Stream Analytics podem ajudar a identificar a causa dos problemas quando estas ocorrem e reduzem o tempo de recuperação.

## <a name="log-types"></a>Tipos de registo

Stream Analytics oferece dois tipos de registos: 
* [Registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (sempre ativo). Registos de atividades fornecem informações sobre as operações executadas em tarefas.
* [Os registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (configurável). Registos de diagnóstico fornecem informações mais ricas sobre tudo o que acontece com uma tarefa. Registos do diagnóstico início quando a tarefa é criada e de fim quando a tarefa é eliminada. Eles abrangem eventos quando a tarefa é atualizada e durante a execução.

> [!NOTE]
> Pode utilizar serviços como o armazenamento do Azure, os Hubs de eventos do Azure e Azure Log Analytics para analisar dados nonconforming. É cobrado com base no modelo de preços para os serviços.
>

## <a name="turn-on-diagnostics-logs"></a>Ativar registos de diagnóstico

Os registos de diagnóstico são **desativar** por predefinição. Para ativar os registos de diagnóstico, conclua estes passos:

1.  Inicie sessão no portal do Azure e aceda ao painel de tarefa de transmissão em fluxo. Sob **monitorização**, selecione **os registos de diagnóstico**.

    ![Navegação do painel para os registos de diagnóstico](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  Selecione **ativar os diagnósticos**.

    ![Ativar registos de diagnóstico do Stream Analytics](./media/stream-analytics-job-diagnostic-logs/turn-on-diagnostic-logs.png)

3.  Sobre o **as definições de diagnóstico** página, para **estado**, selecione **no**.

    ![Alterar Estado para os registos de diagnóstico](./media/stream-analytics-job-diagnostic-logs/save-diagnostic-log-settings.png)

4.  Configure o arquivo de destino (conta de armazenamento, hub de eventos do Log Analytics) que pretende. Em seguida, selecione as categorias de registos que pretende recolher (execução, criação de conteúdos). 

5.  Guarde a nova configuração de diagnósticos.

A configuração de diagnósticos demora cerca de 10 minutos para entrar em vigor. Depois disso, os registos de começar a aparecer no destino de arquivo configurado (pode ver isso no **registos de diagnóstico** página):

![Navegação do painel para os registos de diagnóstico - destinos de arquivos](./media/stream-analytics-job-diagnostic-logs/view-diagnostic-logs-page.png)

Para obter mais informações sobre como configurar diagnósticos, consulte [recolher e consumir dados de diagnóstico de recursos do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Categorias de registo de diagnóstico

Atualmente, podemos capturar duas categorias de registos de diagnóstico:

* **Criação**. Capturas de registar eventos relacionados com operações de criação da tarefa: criação, adicionar e eliminar entradas e saídas, adicionar e atualizar a consulta, iniciar e parar a tarefa da tarefa.
* **Execução**. Captura de eventos que ocorrem durante a execução da tarefa:
    * Erros de conectividade
    * Erros de processamento de dados, incluindo:
        * Eventos que não estão em conformidade com a definição de consulta (tipos de campo não correspondentes e valores em falta campos e assim por diante)
        * Erros de avaliação de expressão
    * Outros eventos e erros

## <a name="diagnostics-logs-schema"></a>Esquema de registos de diagnóstico

Todos os registos são armazenados no formato JSON. Cada entrada tem os seguintes campos de cadeia de caracteres comuns:

Nome | Descrição
------- | -------
hora | Timestamp (em UTC) do registo.
resourceId | ID do recurso que a operação ocorreu, em maiúsculas. Ele inclui o ID de subscrição, o grupo de recursos e o nome da tarefa. Por exemplo,   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
categoria | Inicie sessão categoria, optar por **execução** ou **criação**.
operationName | Nome da operação que é registado. Por exemplo, **enviar eventos: falha de escrita de saída SQL para mysqloutput**.
status | Estado da operação. Por exemplo, **falhada** ou **Succeeded**.
nível | Nível de registo. Por exemplo, **erro**, **aviso**, ou **informativo**.
propriedades | Detalhes de entrada específico do registo, serializado como uma cadeia de caracteres do JSON. Para obter mais informações, consulte as secções seguintes.

### <a name="execution-log-properties-schema"></a>Esquema de propriedades de registo de execução

Registos de execução tem informações sobre eventos que ocorreram durante a execução de tarefa do Stream Analytics. O esquema das propriedades varia consoante o tipo de evento. Atualmente, temos os seguintes tipos de registos de execução:

### <a name="data-errors"></a>Erros de dados

Qualquer erro que ocorre durante a tarefa está a processar dados está nesta categoria de registos. Estes registos com mais frequência são criados durante a dados lidos, serialização e operações de escrita. Estes registos não incluem erros de conectividade. Erros de conectividade são tratados como eventos genéricos.

Nome | Descrição
------- | -------
Origem | Nome da tarefa de entrada ou saída onde ocorreu o erro.
Mensagem | Mensagem associada com o erro.
Tipo | Tipo de erro. Por exemplo, **DataConversionError**, **CsvParserError**, ou **ServiceBusPropertyColumnMissingError**.
Dados | Contém dados que são útil para localizar com precisão a origem do erro. Sujeito a truncagem, dependendo do tamanho.

Consoante a **operationName** valor, erros de dados com o esquema seguinte:
* **Serialização de eventos**. Serialização de eventos que ocorrem durante operações de leitura de evento. Que ocorrem quando os dados de entrada não satisfazem o esquema de consulta para um dos seguintes motivos:
    * *Incompatibilidade de tipo durante o evento (de) serializar*: identifica o campo que está causando o erro.
    * *Não é possível ler um evento, a serialização inválido*: lista as informações sobre a localização dos dados de entrada onde ocorreu o erro. Inclui o nome do blob de entrada de BLOBs, desvio e um exemplo dos dados.
* **Enviar eventos**. Envie eventos ocorrem durante operações de escrita. Identificam o evento de transmissão em fluxo que causou o erro.

### <a name="generic-events"></a>Eventos genéricos

Eventos genéricos abrangem todo o resto.

Nome | Descrição
-------- | --------
Erro | (opcional) Informações de erro. Normalmente, isto é informações de exceção, se estiver disponível.
Mensagem| Mensagem do registo.
Tipo | Tipo de mensagem. É mapeado para interno categorização de erros. Por exemplo, **JobValidationError** ou **BlobOutputAdapterInitializationFailure**.
ID de Correlação | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) que identifica exclusivamente a execução de tarefa. Todas as entradas de registo de execução desde o momento em que a tarefa é iniciada até que o pára de tarefa tem o mesmo **ID de correlação** valor.

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Stream Analytics](stream-analytics-introduction.md)
* [Introdução ao Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Dimensionar tarefas do Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência de linguagem de consulta do Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência da REST API de gestão de análise de Stream](https://msdn.microsoft.com/library/azure/dn835031.aspx)
