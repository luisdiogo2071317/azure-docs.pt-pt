---
title: Utilizar o Pig de Hadoop com o PowerShell no HDInsight - Azure
description: Aprenda a submeter tarefas do Pig a um cluster do Hadoop no HDInsight com o Azure PowerShell.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: ff08e632c1bfd8eb4040e4e746ce08335eba8b08
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047379"
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>Utilizar o Azure PowerShell para executar tarefas do Pig com o HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Este documento fornece um exemplo de como utilizar o Azure PowerShell para submeter tarefas do Pig a um Hadoop no HDInsight cluster. PIg permite-lhe escrever tarefas de MapReduce com um idioma (Pig Latin) que transformações de dados de modelos, em vez do mapa e reduzir as funções.

> [!NOTE]
> Este documento não fornece uma descrição detalhada do que fazem as instruções em Pig Latin usadas nos exemplos. Para obter informações sobre o Pig Latin utilizado neste exemplo, consulte [utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Pré-requisitos

* **Um cluster do Azure HDInsight**

  > [!IMPORTANT]
  > O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

* **Uma estação de trabalho com o Azure PowerShell**.

## <a id="powershell"></a>Executar uma tarefa Pig

O Azure PowerShell disponibiliza *cmdlets* que permitem executar remotamente tarefas do Pig no HDInsight. Internamente, o PowerShell utiliza chamadas REST para [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) em execução no cluster do HDInsight.

Os cmdlets seguintes são utilizados quando executar tarefas do Pig num cluster do HDInsight remoto:

* **Connect-AzureRmAccount**: autentica o Azure PowerShell para a sua subscrição do Azure.
* **Novo AzureRmHDInsightPigJobDefinition**: cria um *definição de tarefa* utilizando as instruções em Pig Latin especificadas.
* **Start-AzureRmHDInsightJob**: envia a definição de tarefa ao HDInsight e inicia a tarefa. R *tarefa* objeto é devolvido.
* **Espera-AzureRmHDInsightJob**: usa o objeto de tarefa para verificar o estado da tarefa. Ele aguarda até que a tarefa for concluída, ou o tempo de espera foi excedido.
* **Get-AzureRmHDInsightJobOutput**: utilizado para obter o resultado da tarefa.

Os passos seguintes demonstram como utilizar estes cmdlets para executar uma tarefa no seu cluster do HDInsight.

1. Com um editor, guarde o código a seguir como **pigjob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Abra uma nova linha de comandos do Windows PowerShell. Altere os diretórios para a localização do **pigjob.ps1** de ficheiros, em seguida, utilize o seguinte comando para executar o script:

        .\pigjob.ps1

    São-lhe pedido para iniciar sessão na sua subscrição do Azure. Em seguida, é-lhe perguntado para o nome da conta de administrador/HTTPs e a palavra-passe para o cluster do HDInsight.

2. Quando a tarefa estiver concluída, deverá devolver informações semelhantes ao seguinte texto:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Resolução de Problemas

Se nenhuma informação é retornada quando a tarefa é concluída, ver os registos de erros. Para ver informações de erro para esta tarefa, adicione o seguinte comando ao final dos **pigjob.ps1** de ficheiros, guarde-o e, em seguida, execute-o novamente.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Este cmdlet devolve as informações de que foi escritas para STDERR durante o processamento da tarefa.

## <a id="summary"></a>Resumo
Como pode ver, o Azure PowerShell fornece uma forma fácil de executar tarefas do Pig num cluster do HDInsight, monitorizar o estado da tarefa e obter o resultado.

## <a id="nextsteps"></a>Passos seguintes
Para obter informações gerais sobre o Pig no HDInsight:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)
