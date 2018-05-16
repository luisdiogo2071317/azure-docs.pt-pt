---
title: 'Início Rápido: Introdução ao Hadoop e ao Hive no Azure HDInsight através de um modelo do Resource Manager | Microsoft Docs'
description: Saiba como criar clusters do HDInsight e consultar dados com o Hive.
keywords: guia de introdução do hadoop, hadoop linux, início rápido do hadoop, introdução do hive, início rápido do hive
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc
ms.devlang: na
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: 4cf20dacf66ee334dcd455ce1770609c175d3b88
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="quickstart-get-started-with-hadoop-and-hive-in-azure-hdinsight-using-resource-manager-template"></a>Início Rápido: Introdução ao Hadoop e ao Hive no Azure HDInsight através de um modelo do Resource Manager

Neste artigo, vai aprender a criar clusters do [Hadoop](http://hadoop.apache.org/) no HDInsight através de um modelo do Resource Manager e, em seguida, executar tarefas do Hive no HDInsight. A maioria das tarefas do Hadoop são tarefas de lote. Cria um cluster, executa algumas tarefas e, em seguida, elimina o cluster. Neste artigo, vai realizar as três tarefas.

Neste início rápido, vai utilizar um modelo do Resource Manager para criar um cluster do Hadoop no HDInsight. Também pode criar um cluster com o [Portal do Azure](apache-hadoop-linux-create-cluster-get-started-portal.md).

Atualmente, o HDInsight inclui [sete tipos diferentes de cluster](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Cada tipo de cluster suporta um conjunto diferente de componentes. Todos os tipos de cluster suportam o Hive. Para obter uma lista dos componentes suportados no HDInsight, consulte [Quais são as novidades nas versões de cluster do Hadoop fornecidas pelo HDInsight?](../hdinsight-component-versioning.md)  

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

<a name="create-cluster"></a>
## <a name="create-a-hadoop-cluster"></a>Criar um cluster do Hadoop

Nesta secção, vai criar um cluster do Hadoop no HDInsight com um modelo do Azure Resource Manager. Não precisa de ter experiência no modelo do Resource Manager para seguir este artigo. 

1. Clique no botão **Implementar no Azure** abaixo para iniciar sessão no Azure abrir o modelo do Resource Manager no portal do Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Introduza ou selecione os valores conforme sugerido na seguinte captura de ecrã:

    > [!NOTE]
    > Os valores que forem fornecidos têm de ser exclusivos e devem seguir as diretrizes de nomenclatura. O modelo não realiza verificações de validação. Caso os valores fornecidos já estejam a ser utilizados ou não sigam as diretrizes, receberá um erro depois de ter submetido o modelo.       
    > 
    >
    
    ![Introdução ao modelo do Resource Manager no portal no HDInsight com Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Implementar o cluster do Hadoop no HDInsight com o portal do Azure e um modelo do gestor do grupo de recursos")

    Introduza ou selecione os seguintes valores:
    
    |Propriedade  |Descrição  |
    |---------|---------|
    |**Subscrição**     |  Selecione a sua subscrição do Azure. |
    |**Grupo de recursos**     | Crie um grupo de recursos ou selecione crie um existente.  Um grupo de recursos é um contentor de componentes do Azure.  Neste caso, o grupo de recursos contém o cluster do HDInsight e a conta de armazenamento do Azure dependente. |
    |**Localização**     | Selecione uma localização do Azure onde quer criar o cluster.  Selecione uma localização mais próxima de si para obter um melhor desempenho. |
    |**Tipo de Cluster**     | Selecione **hadoop**. |
    |**Nome do Cluster**     | Introduza um nome para o cluster do Hadoop. Uma vez que todos os clusters no HDInsight partilham o mesmo espaço de nomes DNS, este nome tem de ser único. O nome pode ser composto por um máximo de 59 carateres, incluindo letras, números e hífenes. O primeiro e o último carateres do nome não podem ser hífenes. |
    |**Nome e palavra-passe de início de sessão no cluster**     | O nome de início de sessão predefinido é **admin**. A palavra-passe tem de ter no mínimo 10 carateres e tem de conter, pelo menos, um número, uma letra maiúscula e uma letra minúscula, e um caráter não alfanumérico (exceto os carateres ' " `\). Certifique-se de que **não escolhe** uma palavra-passe comum, tal como "Pass@word1".|
    |**Nome de utilizador e palavra-passe de SSH**     | O nome de utilizador predefinido é **sshuser**.  Pode mudar o nome de utilizador do SSH.  A palavra-passe de utilizador do SSH tem os mesmos requisitos que a palavra-passe de início de sessão do cluster.|
       
    Algumas propriedades foram codificadas no modelo.  Pode configurar estes valores a partir do modelo. Para obter mais explicações sobre estas propriedades, veja [Criar clusters Hadoop no HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

3. Selecione **Concordo com os termos e condições indicados acima** e **Afixar ao dashboard**. Em seguida, clique em **Comprar**. Irá ver um novo mosaico intitulado **A submeter a implementação** no dashboard do portal. A criação de um cluster demora cerca de 20 minutos.

    ![Progresso da implementação do modelo](./media/apache-hadoop-linux-tutorial-get-started/deployment-progress-tile.png "Progresso da implementação do modelo do Azure")

4. Assim que o cluster for criado, a legenda do mosaico é alterada para o nome do grupo de recursos que especificou. O mosaico também indica o cluster do HDInsight que é criado no grupo de recursos. 
   
    ![Introdução ao grupo de recursos no HDInsight com Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Grupo de recursos de cluster no Azure HDInsight")
    
5. O mosaico indica igualmente o armazenamento predefinido associado ao cluster. Cada cluster tem uma dependência de [conta do Armazenamento do Azure](../hdinsight-hadoop-use-blob-storage.md) ou de [conta do Azure Data Lake](../hdinsight-hadoop-use-data-lake-store.md). É designada de conta de armazenamento predefinida. O cluster do HDInsight e a respetiva conta do Storage predefinida devem estar colocalizados na mesma região do Azure. A eliminação dos clusters não elimina a conta de armazenamento.
    

> [!NOTE]
> Para conhecer outros métodos de criação de clusters e compreender as propriedades utilizadas neste tutorial, consulte [Criar clusters do HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).       
> 
>

## <a name="run-hive-queries"></a>Executar consultas do Hive

O [Apache Hive](hdinsight-use-hive.md) é o componente mais popular utilizado no HDInsight. Existem várias formas de executar tarefas do Hive no HDInsight. Neste tutorial, vai utilizar a vista Ambari Hive do portal. Para conhecer outros métodos de submissão de tarefas do Hive, consulte [Utilizar o Hive no HDInsight](hdinsight-use-hive.md).

1. Para abrir o Ambari, a partir da captura de ecrã anterior, selecione **Dashboard de Clusters**.  Também pode procurar em **https://&lt;ClusterName>.azurehdinsight.net**, onde &lt;ClusterName> é o cluster que criou na secção anterior.

    ![Introdução ao dashboard de clusters no HDInsight com Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "Introdução ao dashboard de clusters no HDInsight com Linux")

2. Introduza o nome de utilizador e a palavra-passe do Hadoop que especificou ao criar o cluster. O nome de utilizador predefinido é **admin**.

3. Abra a **Vista do Hive** conforme mostrado na captura de ecrã seguinte:
   
    ![Selecionar vistas Ambari](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "Menu do Ambari Hive Viewer")

4. No separador **CONSULTAS**, cole as seguintes declarações HiveQL na folha de cálculo:
   
        SHOW TABLES;

    ![Vistas do Hive do HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "Editor de Consulta do Hive View do HDInsight")
   
   > [!NOTE]
   > O Hive requer a utilização do ponto e vírgula.       
   > 
   > 

5. Selecione **Executar**. O separador **RESULTADOS** aparece por baixo do separador **CONSULTA** e apresenta informações sobre a tarefa. 
   
    Assim que a consulta estiver concluída, o separador **CONSULTA** apresenta os resultados da operação. Deverá ver uma tabela denomizada **hivesampletable**. Esta tabela do Hive de exemplo inclui todos os clusters do HDInsight.
   
    ![Vistas do Hive do HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "Editor de Consulta do Hive View do HDInsight")

6. Repita os passos 4 e 5 para executar a seguinte consulta:
   
        SELECT * FROM hivesampletable;
   
7. Também pode guardar os resultados da consulta. Selecione o botão de menu à direita e especifique se quer transferir os resultados como um ficheiro CSV ou armazená-los na conta de armazenamento associada ao cluster.

    ![Guardar o resultado da consulta do Hive](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Guardar o resultado da consulta do Hive")

Depois de concluir uma tarefa deo Hive, pode [exportar os resultados para a SQL Database do Azure ou a base de dados do SQL Server](apache-hadoop-use-sqoop-mac-linux.md). Do mesmo modo, pode [visualizar os resultados com o Excel](apache-hadoop-connect-excel-power-query.md). Para obter mais informações sobre como utilizar o Hive no HDInsight, consulte [Utilizar o Hive e o HiveQL com o Hadoop no HDInsight para analisar um ficheiro Apache log4j de exemplo](hdinsight-use-hive.md).

## <a name="troubleshoot"></a>Resolução de problemas

Caso se depare com problemas com a criação de clusters do HDInsight, veja [aceder aos requisitos de controlo](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="clean-up-resources"></a>Limpar recursos
Depois de concluir o artigo, pode achar conveniente eliminar o cluster. Com o HDInsight, os dados são armazenados no Storage do Azure, pelo que pode eliminar um cluster em segurança quando este não está a ser utilizado. Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados. 

> [!NOTE]
> Se tenciona avançar *imediatamente* para o tutorial seguinte para saber como executar operações de ETL com o Hadoop no HDInsight, talvez seja conveniente manter o cluster em execução. Isto porque tem de criar novamente um cluster do Hadoop no tutorial. No entanto, se não tenciona avançar já para o tutorial seguinte, tem de eliminar o cluster agora.
> 
> 

**Para eliminar o cluster e/ou a conta de armazenamento predefinida**

1. Volte ao separador do browser onde tem o portal do Azure. Deverá estar na página de descrição geral do cluster. Se apenas quiser eliminar o cluster, mas quiser manter a conta de armazenamento predefinida, selecione **Eliminar**.

    ![Eliminação de cluster do HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "Eliminar cluster do HDInsight")

2. Se quiser eliminar o cluster e a conta de armazenamento predefinida, selecione o nome do grupo de recursos (realçado na captura de ecrã anterior) para abrir a página do grupo de recursos.

3. Selecione **Eliminar grupo de recursos** para eliminar o grupo de recursos, o qual contém o cluster e a conta de armazenamento predefinida. Tenha em atenção que a eliminação do grupo de recursos elimina a conta de armazenamento. Se pretender manter a conta do Storage, opte por eliminar apenas o cluster.

## <a name="next-steps"></a>Passos seguintes
Neste artigo, aprendeu a criar um cluster do HDInsight baseado em Linux com um modelo do Resource Manager, bem como a realizar consultas básicas do Hive. No artigo seguinte, vai aprender a realizar uma operação de ETL (extração, transformação e carregamento) com o Hadoop no HDInsight.

> [!div class="nextstepaction"]
>[Extrair, transformar e carregar dados com o Apache Hive no HDInsight](../hdinsight-analyze-flight-delay-data-linux.md)

Se estiver pronto para começar a trabalhar com os seus próprios dados e precisar de saber mais sobre a forma como o HDInsight armazena os dados ou como carregar dados para o HDInsight, consulte os seguintes artigos:

* Para obter informações sobre como o HDInsight utiliza o Armazenamento do Azure, consulte [Utilizar o Armazenamento do Azure com o HDInsight](../hdinsight-hadoop-use-blob-storage.md).
* Para obter informações sobre como carregar dados para o HDInsight, veja [Upload data to HDInsight](../hdinsight-upload-data.md) (Carregar dados para o HDInsight).

Saiba mais sobre como analisar os dados com o HDInsight, veja os seguintes artigos:

* Para saber mais sobre como utilizar o Hive com o HDInsight, incluindo como executar consultas do Hive a partir do Visual Studio, veja [Use Hive with HDInsight](hdinsight-use-hive.md) (Utilizar o Hive com o HDInsight).
* Para saber mais sobre o Pig, uma linguagem utilizada para transformar dados, veja [Use Pig with HDInsight](hdinsight-use-pig.md) (Utilizar o Pig com o HDInsight).
* Para saber mais sobre o MapReduce, uma forma de escrever programas que processam dados no Hadoop, veja [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md) (Utilizar o MapReduce com o HDInsight).
* Para saber mais sobre como utilizar as Ferramentas do HDInsight para o Visual Studio para analisar dados no HDInsight, consulte [Começar a utilizar as ferramentas Hadoop do Visual Studio para o HDInsight](apache-hadoop-visual-studio-tools-get-started.md).



Se gostaria de saber mais sobre como criar ou gerir um cluster do HDInsight, consulte os seguintes artigos:

* Para saber mais sobre a gestão do seu cluster do HDInsight baseado em Linux, consulte [Gerir clusters do HDInsight com o Ambari](../hdinsight-hadoop-manage-ambari.md).
* Para saber mais sobre as opções que pode selecionar ao criar um cluster do HDInsight, consulte [Criar HDInsight no Linux utilizando opções personalizadas](../hdinsight-hadoop-provision-linux-clusters.md).


[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


