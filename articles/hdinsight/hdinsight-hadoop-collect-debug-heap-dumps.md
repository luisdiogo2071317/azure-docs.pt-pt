---
title: Depurar e analisar os serviços do Apache Hadoop com informações de estado do heap - Azure
description: Recolher registos da área dinâmica para serviços do Apache Hadoop e colocar dentro da conta de armazenamento de Blobs do Azure para depuração e análise automaticamente.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 2a399899c93addf966d3f2ec0e36d4b1c76b686f
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038312"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Área dinâmica para dados recolher informações do Estado no armazenamento de BLOBs para depurar e analisar os serviços do Apache Hadoop
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Capturas de área dinâmica para dados contém um instantâneo de memória do aplicativo, incluindo os valores das variáveis no momento que a captura foi criada. Portanto, eles são úteis para diagnosticar problemas que ocorrem no tempo de execução. Registos da área dinâmica podem ser automaticamente recolhidos para serviços do Apache Hadoop e colocados dentro da conta de armazenamento de Blobs do Azure de um utilizador em HDInsightHeapDumps /.

A coleção de registos da área dinâmica para vários serviços tem de estar ativada para os serviços em clusters individuais. A predefinição para esta funcionalidade é ter um para um cluster. Estes registos da área dinâmica podem ser grandes, portanto, é aconselhável para monitorizar a conta de armazenamento de BLOBs onde eles são guardados depois da coleção foi ativada.

> [!IMPORTANT]
> O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows). As informações neste artigo aplica-se apenas ao HDInsight baseado em Windows. Para obter informações sobre o HDInsight baseado em Linux, consulte [registos da área dinâmica de ativar para serviços do Hadoop no HDInsight baseado em Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Serviços elegíveis para capturas de área dinâmica para dados
Pode ativar os registos da área dinâmica para os seguintes serviços:

* **O Apache hcatalog** -tempelton
* **Ramo de registo do Apache** -hiveserver2, metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **Apache yarn** -resourcemanager nodemanager, timelineserver
* **Apache hdfs** -datanode secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Elementos de configuração que ativar capturas de área dinâmica para dados
Para ativar registos da área dinâmica para um serviço, tem de definir os elementos de configuração adequado na secção para esse serviço, o que é especificado por **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

O valor de **service_name** pode ser qualquer um dos serviços listados aqui: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, ou namenode.

## <a name="enable-using-azure-powershell"></a>Ativar com o Azure PowerShell
Por exemplo, para ativar capturas de área dinâmica para dados com o Azure PowerShell para jobhistoryserver, pode usar o seguinte script:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Ativar com o .NET SDK
Por exemplo, para ativar capturas de área dinâmica para dados ao utilizar o Azure HDInsight .NET SDK para jobhistoryserver, pode usar o seguinte código:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
