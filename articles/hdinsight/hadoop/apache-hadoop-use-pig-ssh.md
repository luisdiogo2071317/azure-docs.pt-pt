---
title: Utilizar o Pig de Hadoop com SSH num cluster do HDInsight - Azure | Microsoft Docs
description: "Saiba como ligar a um cluster de Hadoop baseado em Linux com SSH e, em seguida, utilize o comando de Pig para executar instruções Pig Latin interativamente, ou como uma tarefa de lote."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: fa19913928bad8b91777c0904324ff5983f6472c
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Executar tarefas do Pig num cluster baseado em Linux com o comando de Pig (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Saiba como executar interativamente tarefas do Pig a partir de uma ligação SSH ao cluster do HDInsight. A linguagem de programação do Pig Latin permite-lhe descrever transformações que são aplicadas aos dados de entrada para produzir o resultado pretendido.

> [!IMPORTANT]
> Os passos neste documento exigem um cluster do HDInsight baseado em Linux. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

## <a id="ssh"></a>Ligar com SSH

Utilize o SSH para ligar ao cluster do HDInsight. O exemplo seguinte liga a um cluster com o nome **myhdinsight** como a conta de chamada **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Para obter mais informações, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Utilize o comando de Pig

1. Assim que estiver ligado, inicie a interface de linha de comandos (CLI) do Pig utilizando o seguinte comando:

    ```bash
    pig
    ```

    Depois de um momento, a linha é alterado para`grunt>`.

2. Introduza a seguinte instrução:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Este comando carrega o conteúdo do ficheiro sample.log para registos. Pode ver o conteúdo do ficheiro, utilizando a seguinte instrução:

    ```piglatin
    DUMP LOGS;
    ```

3. Em seguida, transforme os dados através da aplicação de uma expressão regular para extrair apenas o nível de registo de cada registo utilizando a seguinte instrução:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    Pode utilizar **informação do estado** para ver os dados depois da transformação. Neste caso, utilize `DUMP LEVELS;`.

4. Continue a aplicar as transformações utilizando as instruções na seguinte tabela:

    | Declaração de PIg Latin | O que faz a instrução |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Remove linhas que contêm um valor nulo para o nível de registo e armazena os resultados em `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Agrupa as linhas por nível de registo e armazena os resultados em `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Cria um conjunto de dados que contém cada registo exclusivo ocorre valor nível e quantas vezes-lo. O conjunto de dados é armazenado em `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Ordena os níveis de registo por contagem (descendente) e armazena para `RESULT`. |

    > [!TIP]
    > Utilize `DUMP` para ver o resultado da transformação depois de cada passo.

5. Também pode guardar os resultados de uma transformação utilizando o `STORE` instrução. Por exemplo, a seguinte instrução guarda o `RESULT` para o `/example/data/pigout` diretório no armazenamento de predefinido para o cluster:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]
   > Os dados são armazenados no diretório especificado em ficheiros com o nome `part-nnnnn`. Se já existe um diretório, receberá um erro.

6. Para sair da linha de comandos da grunt, introduza a seguinte instrução:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Ficheiros de batch de PIg Latin

Também pode utilizar o comando de Pig para executar o Pig Latin contidos num ficheiro.

1. Depois de sair da linha de comandos grunt, utilize o seguinte comando para criar o ficheiro denominado `pigbatch.pig`:

    ```bash
    nano ~/pigbatch.pig
    ```

2. Escreva ou cole as seguintes linhas:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    Quando terminar, utilize __Ctrl__ + __X__, __Y__e, em seguida, __Enter__ para guardar o ficheiro.

3. Utilize o seguinte comando para executar o `pigbatch.pig` ficheiro utilizando o comando de Pig.

    ```bash
    pig ~/pigbatch.pig
    ```

    Após a conclusão da tarefa de lote, consulte o seguinte resultado:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Passos seguintes

Para obter informações gerais sobre o Pig no HDInsight, consulte o documento seguinte:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)

Para obter mais informações sobre outras formas de trabalhar com o Hadoop no HDInsight, consulte os seguintes documentos:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)
