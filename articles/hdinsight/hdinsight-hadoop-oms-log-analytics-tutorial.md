---
title: Utilizar o Log Analytics para monitorizar clusters do HDInsight do Azure
description: Saiba como utilizar o Azure Log Analytics para monitorizar tarefas em execução num cluster do HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 2af68c0b9deb9d5b065f5fae42cf4bf927f0c671
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53386459"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Utilizar o Azure Log Analytics para monitorizar clusters do HDInsight

Saiba como ativar o Azure Log Analytics para monitorizar as operações de cluster de Hadoop no HDInsight e como adicionar uma solução de monitorização de Hdinsight.

[Log Analytics](../log-analytics/log-analytics-overview.md) é um serviço no Azure Monitor, que monitoriza a sua cloud e ambientes para manter a disponibilidade e desempenho no local. Recolhe dados gerados por recursos nos seus ambientes na cloud e no local e de outras ferramentas de monitorização, para disponibilizar análises relativas a várias origens.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma área de trabalho do Log Analytics**. Pode pensar nesta área de trabalho como um ambiente do Log Analytics exclusivo com seu próprio repositório de dados, origens de dados e soluções. Para obter instruções, consulte [criar uma área de trabalho do Log Analytics](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* **Um cluster do Azure HDInsight**. Atualmente, pode utilizar o Log Analytics com os seguintes tipos de cluster do HDInsight:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Para instruções sobre como criar um cluster do HDInsight, consulte [introdução ao Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

> [!NOTE]  
> Recomenda-se para colocar o cluster do HDInsight e a área de trabalho do Log Analytics na mesma região para um melhor desempenho. O Azure Log Analytics não está disponível em todas as regiões do Azure.

## <a name="enable-log-analytics-by-using-the-portal"></a>Ativar o Log Analytics com o portal

Nesta secção, vai configurar um cluster de Hadoop do HDInsight existente para utilizar uma área de trabalho do Log Analytics do Azure para monitorizar tarefas, os registos de depuração, etc.

1. Abra um cluster do HDInsight no portal do Azure.
2. No painel esquerdo, selecione **monitorização**.
3. No painel direito, selecione **habilitar**, selecione uma área de trabalho do Log Analytics existente e, em seguida, selecione **guardar**.

    ![Ativar a monitorização para os clusters do HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "ativar a monitorização para os clusters do HDInsight")

    Demora alguns instantes para guardar a definição.

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Ativar o Log Analytics com o Azure PowerShell

Pode habilitar o Log Analytics com o Azure PowerShell. O cmdlet é:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

Ver [Enable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0).

Para desativar, o cmdlet é:

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

Ver [Disable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0).

## <a name="install-hdinsight-cluster-management-solutions"></a>Instalar as soluções de gerenciamento de cluster do HDInsight

HDInsight fornece soluções de gerenciamento de cluster específicos que podem ser adicionados para o Azure Log Analytics. [Soluções de gestão](../log-analytics/log-analytics-add-solutions.md) acrescentam funcionalidades ao Log Analytics, proporcionando dados e ferramentas de análise adicionais. Estas soluções recolher métricas de desempenho importantes de seus clusters do HDInsight e fornecem as ferramentas para as métricas de pesquisa. Essas soluções também oferecem a visualizações e dashboards para a maioria dos tipos de cluster suportadas no HDInsight. Ao utilizar as métricas que recolhe com a solução, pode criar regras personalizadas de monitorização e alertas.

Estas são as soluções disponíveis do HDInsight:

* Monitorização do HDInsight Hadoop
* Monitorização do HDInsight HBase
* Monitorização do HDInsight Interactive Query
* Monitorização HDInsight Kafka
* Monitorização HDInsight Spark
* Monitorização do HDInsight Storm

Para as instruções para instalar uma solução de gestão, consulte [soluções de gestão do Azure](../azure-monitor/insights/solutions.md#install-a-management-solution). Para experimentar, instale uma solução de Monotiring de Hadoop do HDInsight. Quando estiver concluído, verá uma **HDInsightHadoop** mosaico listado na **resumo**. Selecione o **HDInsightHadoop** mosaico. A solução de HDInsightHadoop é semelhante a:

![Vista de solução de monitorização HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Uma vez que o cluster é um cluster totalmente novo, o relatório não mostra todas as atividades.

## <a name="next-steps"></a>Passos Seguintes

* [Consultar o Azure Log Analytics para monitorizar clusters do HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)
