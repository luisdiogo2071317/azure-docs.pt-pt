---
title: Executar programas personalizados do MapReduce - Azure HDInsight | Microsoft Docs
description: Quando e como executar programas personalizados do MapReduce no HDInsight.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: 8e65c946d2cfcc830a1b9fa59b3f7886857f4f7d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="run-custom-mapreduce-programs"></a>Executar programas MapReduce personalizados

Ativar a sistemas de macrodados baseado no Hadoop, tais como o HDInsight utilizando uma vasta gama de tecnologias e ferramentas de processamento de dados. A tabela seguinte descreve os principais vantagens e as considerações para cada um deles.

| Mecanismo de consulta | Vantagens | Considerações |
| --- | --- | --- |
| **Utilizar o HiveQL de ramo de registo** | <ul><li>Uma excelente solução para processamento em lote e análise de grandes quantidades de dados imutáveis de resumo de dados e na consulta do pedido. Utiliza uma sintaxe familiar do SQL Server semelhante.</li><li>Pode ser utilizada para produzir persistentes tabelas de dados que podem ser facilmente particionados e indexadas.</li><li>Podem ser criadas várias vistas de tabelas externas e sobre os mesmos dados.</li><li>Suporta uma implementação do armazém de dados simples que fornece capacidades de escalamento horizontal e tolerância a falhas em grande escala para processamento e armazenamento de dados.</li></ul> | <ul><li>Requer a origem de dados ter, pelo menos, algumas estrutura identificável.</li><li>Não é adequado para consultas em tempo real e atualizações de nível de linha. É melhor utilizada para tarefas do batch através de grandes conjuntos de dados.</li><li>Poderá não ser capaz de realizar alguns tipos de tarefas de processamento complexas.</li></ul> |
| **Utilizar o Pig Latin do PIg** | <ul><li>Uma excelente solução para manipulação de dados como define, intercalar e filtragem de conjuntos de dados, aplicar funções a registos ou grupos de registos e para reestruturar dados através da definição de colunas, pelos valores de agrupamento ou convertendo colunas para linhas.</li><li>Pode utilizar uma abordagem baseada no fluxo de trabalho como uma sequência de operações nos dados.</li></ul> | <ul><li>Utilizadores SQL poderão encontrar que PIg Latin está menos familiares e mais difícil de utilizar que HiveQL.</li><li>A saída predefinida é, normalmente, um ficheiro de texto e, por isso, pode ser mais difícil de utilizar ferramentas de visualização, tais como o Excel. Normalmente, irá camada uma tabela do Hive através de saída.</li></ul> |
| **Mapa/reduzir personalizado** | <ul><li>Disponibiliza controlo total sobre o mapa e reduzir fases e execução.</li><li>Permite que as consultas para otimizar o desempenho máximo do cluster ou para minimizar a carga nos servidores e a rede.</li><li>Os componentes podem ser escritos num intervalo de idiomas bem conhecidos.</li></ul> | <ul><li>É mais difícil de utilizar o Pig ou Hive porque tem de criar o seus próprios mapa e reduzir os componentes.</li><li>Os processos que necessitam de conjuntos de dados de associação são mais difíceis de implementar.</li><li>Apesar de existirem estruturas de teste disponíveis, a depuração de código é mais complexo do que um normal da aplicação porque o código é executado como uma tarefa de lote sob o controlo do Programador de tarefas do Hadoop.</li></ul> |
| **HCatalog** | <ul><li>Deduz-os detalhes do caminho de armazenamento, facilitando a administração e remover a necessidade dos utilizadores saber onde os dados são armazenados.</li><li>Permite que a notificação de eventos, tais como a disponibilidade de dados, permitindo que outras ferramentas, como Oozie para detetar quando ocorreram operações.</li><li>É também expõe uma vista de dados, incluindo a criação de partições pela chave, relacional e facilita os dados para o acesso.</li></ul> | <ul><li>Suporta RCFile, texto em formato CSV, o texto JSON, SequenceFile e ORC formatos de ficheiro por predefinição, mas poderá ter de escrever um SerDe personalizado para outros formatos.</li><li>HCatalog não é seguro para thread.</li><li>Existem algumas restrições sobre os tipos de dados para colunas ao utilizar o carregador do HCatalog em Pig scripts. Para obter mais informações, consulte [tipos de dados de HCatLoader](http://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) na documentação do Apache HCatalog.</li></ul> |

Normalmente, utiliza mais simples, uma destas abordagens que podem fornecer os resultados que necessita. Por exemplo, poderá conseguir alcançar esses resultados utilizando o Hive apenas, mas para cenários mais complexos poderá ter de utilizar o Pig, ou escrever o seus próprios mapa ainda e reduzir os componentes. Também pode decidir, após conseguirmos uma com o ramo de registo ou o Pig, que o mapa personalizado e reduzir componentes podem fornecer um melhor desempenho, permitindo-lhe ajustar e otimizar o processamento.

## <a name="custom-mapreduce-components"></a>Componentes de mapa personalizado/reduzir

Código de mapa/reduzir consiste em duas funções separadas implementadas como **mapa** e **reduzir** componentes. O **mapa** componente é executado em paralelo em vários nós de cluster, cada nó de aplicar o mapeamento para o subconjunto do nó dos dados. O **reduzir** componente collates e resume os resultados de todas as funções de mapa. Para obter mais informações sobre estes dois componentes, consulte [MapReduce de utilização no Hadoop no HDInsight](hdinsight-use-mapreduce.md).

Na maioria dos cenários de processamento do HDInsight é mais simples e eficiente para utilizar uma abstração de nível mais elevada, tais como o Pig ou Hive. Também pode criar o mapa personalizado e reduzir componentes para utilização em scripts de ramo de registo para efetuar o processamento de mais sofisticado.

Componentes de mapa personalizado/reduzir normalmente são escritos em Java. Hadoop fornece uma interface de transmissão em fluxo que também permite que os componentes a ser utilizado, que são desenvolvidos noutras linguagens, como c#, F #, Visual Basic, Python e JavaScript.

* Para obter instruções sobre como desenvolver programas de Java MapReduce personalizados, consulte [programas desenvolver MapReduce de Java para o Hadoop no HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md).
* Para ver um exemplo com o Python, consulte [Python desenvolver MapReduce programas transmissão em fluxo para o HDInsight](apache-hadoop-streaming-python.md).

Considere criar o seus próprios mapa e reduzir componentes para as seguintes condições:

* Tem de processar os dados que é completamente não estruturados pela análise dos dados e lógica personalizada a utilizar para obter informações estruturadas do mesmo.
* Pretende executar as tarefas complexas que são difíceis (ou impossível) para expressar no Pig ou sem resorting para a criação de um UDF de ramo de registo. Por exemplo, poderá ter de utilizar um serviço de codificação geográfica externo para converter a latitude e longitude coordenadas ou endereços IP na origem de dados para os nomes de localização geográfica.
* Pretende reutilizar o código de .NET, Python ou JavaScript existente no mapa/reduzir componentes utilizando o Hadoop interface de transmissão em fluxo.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Carregar e executar o programa de MapReduce personalizado

Os programas de MapReduce mais comuns são escritos em Java e compilados para um ficheiro jar.

1. Depois de ter desenvolvidas, compilado e testar o programa de MapReduce, utilize o `scp` comando para carregar o ficheiro jar para o headnode.

    ```bash
    scp mycustomprogram.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Substitua **USERNAME** com a conta de utilizador do SSH para o cluster. Substitua **CLUSTERNAME** com o nome do cluster. Se utilizou uma palavra-passe para proteger a conta do SSH, lhe for pedido que introduza a palavra-passe. Se utilizou um certificado, poderá ter de utilizar o `-i` parâmetro para especificar o ficheiro de chave privada.

2. Ligar para o cluster utilizando [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Da sessão SSH, execute o programa de MapReduce através de YARN.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Este comando submete a tarefa de MapReduce para YARN. O ficheiro de entrada é `/example/data/sample.log`, e o diretório de saída é `/example/data/logoutput`. O ficheiro de entrada e de quaisquer ficheiros de saída são armazenados no armazenamento de predefinido para o cluster.

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar c# com o MapReduce, transmissão em fluxo do Hadoop no HDInsight](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Desenvolver programas de Java MapReduce para o Hadoop no HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Desenvolver Python MapReduce programas transmissão em fluxo para o HDInsight](apache-hadoop-streaming-python.md)
* [Utilize o Toolkit de Azure do Eclipse para criar Spark aplicações para um cluster do HDInsight](../spark/apache-spark-eclipse-tool-plugin.md)
* [Utilize Python funções definidas pelo utilizador (UDF) com o Hive e Pig no HDInsight](python-udf-hdinsight.md)
