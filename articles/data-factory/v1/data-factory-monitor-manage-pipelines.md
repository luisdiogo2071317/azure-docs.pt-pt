---
title: Monitorizar e gerir pipelines com o portal do Azure e o PowerShell | Microsoft Docs
description: Saiba como utilizar o portal do Azure e o Azure PowerShell para monitorizar e gerir as fábricas de dados do Azure e os pipelines que criou.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 0db357194aa4bd4d01b1d445cb407d983214b32b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054134"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>Monitorizar e gerir pipelines do Azure Data Factory com o portal do Azure e o PowerShell
> [!div class="op_single_selector"]
> * [Utilizar o portal do Azure/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Utilizar aplicações de gestão e monitorização](data-factory-monitor-manage-app.md)

> [!NOTE]
> Este artigo aplica-se a versão 1 do Data Factory. Se estiver a utilizar a versão atual do serviço Data Factory, consulte o artigo [monitorizar e gerir pipelines do Data Factory no](../monitor-visually.md).

Este artigo descreve como monitorizar, gerir e depurar pipelines com o portal do Azure e o PowerShell.

> [!IMPORTANT]
> A aplicação de monitorização e gestão fornece um melhor suporte para monitorizar e gerir os seus pipelines de dados e quaisquer problemas de resolução de problemas. Para obter mais informações sobre como utilizar a aplicação, consulte [monitorizar e gerir pipelines de fábrica de dados utilizando a aplicação de monitorização e gestão](data-factory-monitor-manage-app.md). 

> [!IMPORTANT]
> A versão do Azure Data Factory 1 agora utiliza o novo [infraestrutura de alerta de Monitor de Azure](../../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). A infraestrutura de alerta antiga foi preterida. Como resultado, os alertas existentes configuradas para a versão 1 dados fábricas deixará de funcionar. Os alertas existentes para v1 fábricas de dados não são migrados automaticamente. Tem de recriar estes alertas a nova infraestrutura de alerta. Inicie sessão no Azure portal e selecione **Monitor** para criar novos alertas nas métricas (por exemplo, executa falha ou é executado com êxito) para a sua versão fábricas de dados de 1.

## <a name="understand-pipelines-and-activity-states"></a>Compreender os pipelines e Estados de atividade
Ao utilizar o portal do Azure, pode:

* Ver a fábrica de dados como um diagrama.
* Ver as atividades num pipeline.
* Ver os conjuntos de dados de entrada e de saída.

Esta secção também descreve como um setor de conjunto de dados passa a partir de um Estado para outro Estado.   

### <a name="navigate-to-your-data-factory"></a>Navegue para a fábrica de dados
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Clique em **fábricas de dados** no menu à esquerda. Se não o vir, clique em **mais serviços >** e, em seguida, clique em **fábricas de dados** sob o **INTELLIGENCE + análise** categoria.

   ![Procurar Tudo > fábricas de dados](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. No **fábricas de dados** painel, selecione a fábrica de dados que está interessado em.

    ![Selecionar fábrica de dados](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Deverá ver a home page da fábrica de dados.

   ![Painel Fábrica de dados](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Vista de diagrama de fábrica de dados
O **diagrama** vista de uma fábrica de dados fornece um painel único de transparência para monitorizar e gerir a fábrica de dados e os respetivos recursos. Para ver o **diagrama** ver da fábrica de dados, clique em **diagrama** na home page da fábrica de dados.

![Vista de diagrama](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Pode ampliar, reduzir, ampliar para ajustar, ampliar para 100%, o esquema do diagrama de bloqueio e posicione automaticamente pipelines e conjuntos de dados. Também pode ver as informações de linhagem de dados (ou seja, Mostrar itens a montante e a jusante dos itens selecionados).

### <a name="activities-inside-a-pipeline"></a>Atividades dentro de um pipeline
1. O pipeline com o botão direito e, em seguida, clique em **abrir pipeline** para ver todas as atividades no pipeline, juntamente com conjuntos de dados de entrada e de saída para as atividades. Esta funcionalidade é útil quando o pipeline inclui mais de uma atividade e pretender compreender a linhagem operacional de um pipeline único.

    ![Menu Abrir pipeline](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. No exemplo seguinte, verá uma atividade de cópia no pipeline com uma entrada e um resultado. 

    ![Atividades dentro de um pipeline](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Pode navegar para a home page da fábrica de dados clicando a **fábrica de dados** ligação no trilho no canto superior esquerdo.

    ![Navegue de volta para a fábrica de dados](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>Ver o estado de cada atividade dentro de um pipeline
Pode ver o estado atual de uma atividade ao visualizar o estado de qualquer um dos conjuntos de dados que são produzidos pela atividade.

Fazendo duplo clique em de **OutputBlobTable** no **diagrama**, pode ver todos os setores que são produzidos pelo execuções de atividade diferente dentro de um pipeline. Pode ver que a atividade de cópia foi executada com êxito para as último oito horas e produzido os setores no **pronto** estado.  

![Estado do pipeline](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Os setores de conjunto de dados na fábrica de dados podem ter um dos seguintes Estados:

<table>
<tr>
    <th align="left">Estado</th><th align="left">Subestado</th><th align="left">Descrição</th>
</tr>
<tr>
    <td rowspan="8">A aguardar</td><td>ScheduleTime</td><td>A hora não são fornecidos para o setor ser executado.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>As dependências a montante não estão prontas.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Os recursos de computação não estão disponíveis.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Todas as instâncias de atividade estão ocupadas a executar outros setores.</td>
</tr>
<tr>
<td>ActivityResume</td><td>A atividade está em pausa e não é possível executar os setores até que a atividade é retomada.</td>
</tr>
<tr>
<td>Repetir</td><td>Execução da atividade está a ser repetida.</td>
</tr>
<tr>
<td>Validação</td><td>Validação ainda não começou.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Validação está a aguardar para ser repetida.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Em Curso</td><td>A Validar</td><td>Validação está em curso.</td>
</tr>
<td>-</td>
<td>O setor está a ser processado.</td>
</tr>
<tr>
<td rowspan="4">Com Falhas</td><td>ServiceHost</td><td>A execução da atividade demorou mais do que as permitidas pela atividade.</td>
</tr>
<tr>
<td>Cancelado</td><td>O setor foi cancelado pelo ação do utilizador.</td>
</tr>
<tr>
<td>Validação</td><td>A validação falhou.</td>
</tr>
<tr>
<td>-</td><td>O setor falhou ser gerado e/ou validar.</td>
</tr>
<td>Pronto</td><td>-</td><td>O setor está pronto para consumo.</td>
</tr>
<tr>
<td>Ignorada</td><td>Nenhuma</td><td>O setor não está a ser processado.</td>
</tr>
<tr>
<td>Nenhuma</td><td>-</td><td>Um setor existia com um Estado diferente, mas foi reposto.</td>
</tr>
</table>



Pode ver os detalhes sobre um setor clicando uma entrada do setor no **atualizado setores recentemente** painel.

![Detalhes de setor](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Se o setor foi executado múltiplas vezes, verá várias linhas no **atividade é executada** lista. Pode ver detalhes sobre uma atividade executar clicando a entrada na execução de **atividade é executada** lista. A lista mostra todos os ficheiros de registo, juntamente com uma mensagem de erro se existir um. Esta funcionalidade é útil para ver e registos de depuração sem ter de deixar a fábrica de dados.

![Detalhes da execução da atividade](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Se o setor não se encontra no **pronto** Estado, pode ver os setores a montante que não estão preparadas e estão a bloquear o execução do setor atual o **setores a montante que não estão prontos** lista. Esta funcionalidade é útil quando o setor estiver no **a aguardar** estado e pretender compreender as dependências a montante que o setor está a aguardar no.

![Setores a montante que não estão prontos](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Diagrama de estado do conjunto de dados
Depois de implementar uma fábrica de dados e pipelines de ter um período válido de Active Directory, o conjunto de dados setores de transição de um Estado para outro. Atualmente, o estado do setor segue o seguinte diagrama de estado:

![Diagrama de estado](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

O fluxo de transição de estado de conjunto de dados no data factory é o seguinte: a aguardar -> no-progresso/em curso (validação) -> pronto/com falhas.

O setor é iniciado num **a aguardar** Estado, a aguardar que as condições prévias ser cumpridos antes de ser executada. Em seguida, a atividade começa a ser executada e o setor entra num **em curso** estado. A execução da atividade pode ou não bem-sucedidos. O setor está marcado como **pronto** ou **falha**, com base no resultado da execução.

Pode repor o setor para voltar atrás do **pronto** ou **falha** estado para o **a aguardar** estado. Também pode marcar o estado do setor **ignorar**, que impede a atividade executar e não a processar o setor.

## <a name="pause-and-resume-pipelines"></a>Colocar em pausa e retomar pipelines
Pode gerir os seus pipelines de utilizando o Azure PowerShell. Por exemplo, pode colocar em pausa e retomar pipelines ao executar os cmdlets do PowerShell do Azure. 

> [!NOTE] 
> A vista de diagrama não suporta a colocação em pausa e retomar a pipelines. Se pretender utilizar uma interface de utilizador, utilize a aplicação de monitorização e gestão. Para obter mais informações sobre como utilizar a aplicação, consulte [monitorizar e gerir pipelines de fábrica de dados utilizando a aplicação de monitorização e gestão](data-factory-monitor-manage-app.md) artigo. 

Pode colocar em pausa/suspender pipelines utilizando o **Suspend-AzureRmDataFactoryPipeline** cmdlet do PowerShell. Este cmdlet é útil quando não pretende executar os seus pipelines de até um problema ser corrigido. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Por exemplo:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Depois de corrigir o problema com o pipeline, pode retomar o pipeline suspenso, executando o seguinte comando do PowerShell:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Por exemplo:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Depurar pipelines
O Azure Data Factory fornece capacidades avançadas de depuração e resolução de problemas de pipelines utilizando o portal do Azure e o Azure PowerShell.

> [!NOTE] 
> É muito mais fáceis de erros troubleshot com a aplicação de gestão e monitorização. Para obter mais informações sobre como utilizar a aplicação, consulte [monitorizar e gerir pipelines de fábrica de dados utilizando a aplicação de monitorização e gestão](data-factory-monitor-manage-app.md) artigo. 

### <a name="find-errors-in-a-pipeline"></a>Encontrar erros num pipeline
Se falhar a execução de atividades num pipeline, o conjunto de dados é produzido pelo pipeline está em estado de erro devido à falha. Pode depurar e resolver erros no Azure Data Factory através dos seguintes métodos.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>Utilizar o portal do Azure para um erro de depuração
1. No **tabela** painel, clique o setor de problema que tenha o **estado** definido como **falha**.

   ![Painel de tabela com o setor do problema](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. No **setor de dados** painel, clique em que a atividade executada que falhou.

   ![Setor de dados com um erro](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. No **detalhes da execução da atividade** painel, pode transferir os ficheiros que estão associados com o processamento do HDInsight. Clique em **transferir** para Estado/stderr transferir o ficheiro de registo de erros que contém detalhes sobre o erro.

   ![Execução do painel de detalhes com o erro da atividade](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>Utilize o PowerShell para depurar um erro
1. Inicie o **Azure PowerShell**.
2. Execute o **Get-AzureRmDataFactorySlice** comando para ver os setores e os respetivos Estados. Deverá ver um setor com o estado **falha**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Por exemplo:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Substitua **StartDateTime** com a hora de início da sua pipeline. 
3. Agora, execute o **Get-AzureRmDataFactoryRun** cmdlet para obter detalhes sobre a atividade executar para o setor.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Por exemplo:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    O valor de StartDateTime é a hora de início para o setor de erro/problema que anotou no passo anterior. A data / hora deve estar entre aspas.
4. Deverá ver um resultado com detalhes sobre o erro que é semelhante ao seguinte:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Pode executar o **guardar AzureRmDataFactoryLog** cmdlet com o valor de Id, o resultado e transferir os ficheiros de registo utilizando o **- DownloadLogsoption** para o cmdlet.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Falhas de voltar a executar num pipeline

> [!IMPORTANT]
> É mais fácil resolver erros e execute novamente setores falhadas utilizando a aplicação de gestão e monitorização. Para obter mais informações sobre como utilizar a aplicação, consulte [monitorizar e gerir pipelines de fábrica de dados utilizando a aplicação de monitorização e gestão](data-factory-monitor-manage-app.md). 

### <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
Depois de resolver problemas e depuração de falhas num pipeline, pode voltar a executar falhas ao navegar para o setor de erro e clicando no **executar** botão na barra de comandos.

![Voltar a executar um setor falhado](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Caso o setor falhou validação devido a uma falha de política (por exemplo, se os dados não estiver disponíveis), pode corrigir a falha e validar novamente clicando a **validar** botão na barra de comandos.

![Corrija os erros e validar](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell
Pode voltar a executar falhas utilizando o **Set-AzureRmDataFactorySliceStatus** cmdlet. Consulte o [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) tópico para a sintaxe e outros detalhes sobre o cmdlet.

**Exemplo:**

O exemplo a seguir define o estado de todos os setores para a tabela 'DAWikiAggregatedData' para 'A aguardar' no Azure data factory 'WikiADF'.

'Tipodeatualização' está definida como 'UpstreamInPipeline', que significa que os Estados de cada setor para a tabela e todas as tabelas de (montante) dependentes estão definidos 'A aguardar'. O valor de possíveis para este parâmetro é 'Indivíduo'.

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```
## <a name="create-alerts-in-the-azure-portal"></a>Criar alertas no portal do Azure

1.  Inicie sessão no Azure portal e selecione **Monitor -> alertas** para abrir a página de alertas.

    ![Abra a página de alertas.](media/data-factory-monitor-manage-pipelines/v1alerts-image1.png)

2.  Selecione **+ nova regra de alerta** para criar um novo alerta.

    ![Criar um novo alerta](media/data-factory-monitor-manage-pipelines/v1alerts-image2.png)

3.  Definir o **condição do alerta**. (Certifique-se de que seleciona **fábricas de dados** no **filtrar por tipo de recurso** campo.) Também pode especificar valores para **dimensões**.

    ![Definir a condição do alerta - destino selecione](media/data-factory-monitor-manage-pipelines/v1alerts-image3.png)

    ![Definir a condição de alerta – adicionar critérios de alerta](media/data-factory-monitor-manage-pipelines/v1alerts-image4.png)

    ![Definir a condição de alerta – Adicionar alerta lógica](media/data-factory-monitor-manage-pipelines/v1alerts-image5.png)

4.  Definir o **detalhes do alerta**.

    ![Definir os detalhes do alerta](media/data-factory-monitor-manage-pipelines/v1alerts-image6.png)

5.  Definir o **grupo ação**.

    ![Definir o grupo de ação - criar um novo grupo de ação](media/data-factory-monitor-manage-pipelines/v1alerts-image7.png)

    ![Definir o grupo de ação - defina as propriedades](media/data-factory-monitor-manage-pipelines/v1alerts-image8.png)

    ![Definir o grupo de ação - criado novo grupo de ação](media/data-factory-monitor-manage-pipelines/v1alerts-image9.png)

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Mover uma fábrica de dados para um grupo de recursos diferente ou uma subscrição
Pode mover uma fábrica de dados para um grupo de recursos diferente ou uma subscrição diferente, utilizando o **mover** comando barra botão na home page da fábrica de dados.

![Mover a fábrica de dados](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Pode também mover quaisquer recursos relacionados (tais como alertas que estão associados a fábrica de dados), juntamente com a fábrica de dados.

![Mover a caixa de diálogo de recursos](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
