---
title: Utilizar o Pig de Hadoop com o ambiente de trabalho remoto no HDInsight - Azure
description: Saiba como utilizar o comando de Pig para executar as instruções em Pig Latin numa ligação de ambiente de trabalho remoto num cluster de Hadoop baseado em Windows no HDInsight.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 5aec07a5ebbbb56abcbaebbddc5579cf4d076b4d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590671"
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Executar tarefas do Pig a partir de uma ligação de ambiente de trabalho remoto
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Este documento fornece um passo a passo para utilizar o comando de Pig para executar instruções em Pig Latin numa ligação de ambiente de trabalho remoto para um cluster do HDInsight baseado em Windows. PIg Latin permite-lhe criar aplicações de MapReduce com a descrição de transformações de dados, em vez do mapa e reduzir as funções.

> [!IMPORTANT]
> Área de trabalho remota só está disponível em clusters do HDInsight que utilizam o Windows como o sistema operativo. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).
>
> Para o HDInsight 3.4 ou superior, consulte [utilizar o Pig com o HDInsight e SSH](apache-hadoop-use-pig-ssh.md) para obter informações sobre como executar interativamente as tarefas do Pig diretamente no cluster de uma linha de comandos.

## <a id="prereq"></a>Pré-requisitos
Para concluir os passos neste artigo, terá de fazer o seguinte.

* Um cluster do HDInsight baseado em Windows (Hadoop no HDInsight)
* Um computador cliente executando o Windows 10, Windows 8 ou Windows 7

## <a id="connect"></a>Ligue-se a área de trabalho remota
Ativar o ambiente de trabalho remoto para o cluster do HDInsight, em seguida, ligá-la ao seguir as instruções em [ligar a clusters do HDInsight através do RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Utilize o comando do Pig
1. Depois de ter uma ligação de ambiente de trabalho remoto, inicie o **linha de comandos do Hadoop** ao utilizar o ícone na área de trabalho.
2. Utilize o seguinte para iniciar o comando de Pig:

        %pig_home%\bin\pig

    Será apresentada com um `grunt>` prompt.
3. Introduza a seguinte instrução:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Este comando carrega o conteúdo do ficheiro Sample log para o ficheiro de registos. Pode ver o conteúdo do arquivo usando o seguinte comando:

        DUMP LOGS;
4. Transforme os dados através da aplicação de uma expressão regular para extrair apenas o nível de registo de cada registo:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Pode usar **DESPEJAR** para ver os dados após a transformação. Neste caso, `DUMP LEVELS;`.
5. Continue a aplicar transformações, utilizando as seguintes instruções. Utilize `DUMP` para ver o resultado da transformação após cada passo.

    <table>
    <tr>
    <th>Declaração</th><th>O que faz</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = níveis de filtro por LOGLEVEL não é nulo;</td><td>Remove as linhas que contêm um valor nulo para o nível de registo e armazena os resultados em FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS grupo por LOGLEVEL;</td><td>Agrupa as linhas por nível de registo e armazena os resultados em GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>As frequências = foreach GROUPEDLEVELS gerar grupo como LOGLEVEL, CONTAGEM (FILTEREDLEVELS. LOGLEVEL) como contar;</td><td>Cria um novo conjunto de dados que contém cada registo único valor de nível e o número de vezes que ele ocorre. Isso é armazenado em frequências</td>
    </tr>
    <tr>
    <td>RESULTADO = ordem frequências por CONTAGEM desc;</td><td>Ordena os níveis de registo por contagem (descendente) e armazena em resultado</td>
    </tr>
</table>

6. Também pode guardar os resultados de uma transformação, utilizando o `STORE` instrução. Por exemplo, o seguinte comando guarda o `RESULT` para o **/example/data/pigout** diretório no contentor de armazenamento padrão para o seu cluster:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > Os dados são armazenados no diretório especificado nos arquivos chamados **parte nnnnn**. Se já existir no diretório, receberá uma mensagem de erro.
   >
   >
   
7. Para sair da linha de comandos da assistente, introduza a seguinte instrução.

        QUIT;

### <a name="pig-latin-batch-files"></a>PIg Latin arquivos em lote
Também pode utilizar o comando de Pig para executar o Pig Latin, que está contido num arquivo.

1. Depois de sair de linha de comandos da assistente, abra **bloco de notas** e crie um novo ficheiro designado **pigbatch.pig** no **% PIG_HOME %** diretório.
2. Escreva ou cole as seguintes linhas para o **pigbatch.pig** de ficheiros e, em seguida, guarde-o:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Utilize o seguinte para executar o **pigbatch.pig** utilizando o comando de pig de ficheiros.

        pig %PIG_HOME%\pigbatch.pig

    Quando tiver concluído o trabalho do batch, deverá ver o resultado seguinte, o que deve ser a mesma quando utilizou `DUMP RESULT;` nos passos anteriores:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Resumo
Como pode ver, o comando de Pig permite-lhe executar as operações de MapReduce interativamente ou executar tarefas do Pig Latin que são armazenadas num arquivo em lotes.

## <a id="nextsteps"></a>Passos seguintes
Para obter informações gerais sobre o Pig no HDInsight:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)
