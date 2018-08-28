---
title: MapReduce e o ambiente de trabalho remoto com o Hadoop no HDInsight - Azure
description: Saiba como utilizar o ambiente de trabalho remoto para ligar ao Hadoop no HDInsight e executar tarefas de MapReduce.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 2ff0677117f67c63ab0dbf050d81db0b3b75d86b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051692"
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Utilizar o MapReduce no Hadoop no HDInsight com o ambiente de trabalho remoto
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Neste artigo, irá aprender como ligar a um Hadoop no HDInsight cluster utilizando o ambiente de trabalho remoto e, em seguida, executar tarefas de MapReduce com o comando de Hadoop.

> [!IMPORTANT]
> Área de trabalho remota só está disponível em clusters do HDInsight baseado em Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).
>
> Para o HDInsight 3.4 ou superior, consulte [utilizar o MapReduce com SSH](apache-hadoop-use-mapreduce-ssh.md) para obter informações sobre a ligação ao HDInsight cluster e executar tarefas de MapReduce.

## <a id="prereq"></a>Pré-requisitos
Para concluir os passos neste artigo, precisará do seguinte:

* Um cluster do HDInsight baseado em Windows (Hadoop no HDInsight)
* Um computador cliente executando o Windows 10, Windows 8 ou Windows 7

## <a id="connect"></a>Ligue-se a área de trabalho remota
Ativar o ambiente de trabalho remoto para o cluster do HDInsight, em seguida, ligá-la ao seguir as instruções em [ligar a clusters do HDInsight através do RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Utilize o comando de Hadoop
Quando estiver ligado ao ambiente de trabalho para o cluster do HDInsight, utilize os seguintes passos para executar uma tarefa de MapReduce com o comando do Hadoop:

1. A partir da área de trabalho do HDInsight, inicie o **linha de comandos do Hadoop**. É aberta numa nova linha de comandos do **c:\apps\dist\hadoop-&lt;número de versão >** diretório.

   > [!NOTE]
   > O número da versão alterado conforme Hadoop é atualizado. O **HADOOP_HOME** variável de ambiente pode ser utilizada para localizar o caminho. Por exemplo, `cd %HADOOP_HOME%` altera os diretórios para o diretório de Hadoop, sem necessidade de saber o número de versão.
   >
   >
2. Para utilizar o **Hadoop** comando para executar uma tarefa de MapReduce de exemplo, utilize o seguinte comando:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Esta ação inicia o **wordcount** classe, que está contido no **hadoop-mapreduce-examples** ficheiro no diretório atual. Como entrada, ele usa o **wasb://example/data/gutenberg/davinci.txt** documentos e de saída são armazenados em: **wasb: / / / exemplo/dados/WordCountOutput**.

   > [!NOTE]
   > Para obter mais informações sobre esta tarefa de MapReduce e os dados de exemplo, consulte <a href="hdinsight-use-mapreduce.md">utilizar o MapReduce no HDInsight Hadoop</a>.
   >
   >
3. A tarefa emite detalhes à medida que é processada, e devolve informações semelhantes ao seguinte quando a tarefa estiver concluída:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Quando a tarefa estiver concluída, utilize o seguinte comando para listar os ficheiros de saída armazenados no **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Isto deve mostrar dois ficheiros, **_SUCCESS** e **parte-r-00000**. O **parte-r-00000** ficheiro contém a saída para esta tarefa.

   > [!NOTE]
   > Algumas tarefas de MapReduce podem dividir os resultados em vários **parte-r-# # #** ficheiros. Se assim for, utilize o # # # sufixo para indicar a ordem dos ficheiros.
   >
   >
5. Para ver o resultado, utilize o seguinte comando:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Esta ação apresenta uma lista das palavras contidas no **wasb://example/data/gutenberg/davinci.txt** arquivo, juntamente com o número de vezes que ocorreu cada palavra. Segue-se um exemplo dos dados que estará contidos no ficheiro:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Resumo
Como pode ver, o comando de Hadoop fornece uma forma fácil de executar tarefas de MapReduce num cluster do HDInsight e, em seguida, ver o resultado da tarefa.

## <a id="nextsteps"></a>Passos seguintes
Para obter informações gerais sobre tarefas de MapReduce no HDInsight:

* [Utilizar o MapReduce no HDInsight Hadoop](hdinsight-use-mapreduce.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)
* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)
