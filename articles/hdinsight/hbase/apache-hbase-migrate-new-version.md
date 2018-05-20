---
title: Migrar um cluster HBase para uma nova versão - Azure HDInsight | Microsoft Docs
description: Como migrar clusters do HBase para uma nova versão.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 2ab0bc792777b73f878bc1728c0d8310ecf41167
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="migrate-an-hbase-cluster-to-a-new-version"></a>Migrar um cluster HBase para uma nova versão

Clusters baseados em tarefas, tais como Spark e o Hadoop, são simples atualizar - consulte [cluster do HDInsight atualizar para uma versão mais recente](../hdinsight-upgrade-cluster.md):

1. Cópia de segurança de dados (armazenados localmente) transitórios.
2. Elimine o cluster existente.
3. Crie um novo cluster na mesma sub-rede da VNET.
4. Importar dados transitórios.
5. Iniciar tarefas e continuar o processamento no novo cluster.

Para atualizar um cluster HBase são necessários alguns passos adicionais, conforme descrito neste artigo.

> [!NOTE]
> O período de indisponibilidade durante a atualização deve ser mínimo, na ordem dos minutos. Este período de indisponibilidade causado pelos passos para esvaziar todos os dados na memória, em seguida, o tempo para configurar e reinicie os serviços no novo cluster. Os resultados variam, dependendo do número de nós, a quantidade de dados e outras variáveis.

## <a name="review-hbase-compatibility"></a>Rever a compatibilidade de HBase

Antes de atualizar o HBase, certifique-se de que as versões de HBase em clusters de origem e de destino são compatíveis. Para obter mais informações, consulte [Hadoop componentes e as versões disponíveis com o HDInsight](../hdinsight-component-versioning.md).

> [!NOTE]
> Recomendamos vivamente que reveja a matriz de compatibilidade da versão no [HBase livro](https://hbase.apache.org/book.html#upgrading).

Eis um exemplo versão matriz de compatibilidade, onde Y indica compatibilidade e N indica uma incompatibilidade potencial:

| Tipo de compatibilidade | Versão principal| Versão secundária | Correção |
| --- | --- | --- | --- |
| Compatibilidade de transmissão de cliente-servidor | N | S | S |
| Compatibilidade de servidor servidor | N | S | S |
| Compatibilidade de formato de ficheiro | N | S | S |
| Compatibilidade de cliente de API | N | S | S |
| Compatibilidade de binária de cliente | N | N | S |
| **Compatibilidade de API de limitada do lado do servidor** |  |  |  |
| Estável | N | S | S |
| Evolução | N | N | S |
| Instável | N | N | N |
| Compatibilidade de dependência | N | S | S |
| Compatibilidade operacional | N | N | S |

> [!NOTE]
> Quaisquer incompatibilidades de quebra devem descritas as notas de lançamento da versão de HBase.

## <a name="upgrade-with-same-hbase-major-version"></a>Atualizar com a mesma versão de principais de HBase

O cenário seguinte é para atualizar do HDInsight 3.4 para 3.6 (ambos são fornecidos com Apache HBase 1.1.2) com a mesma versão de principais de HBase. Outra atualização de versão é semelhantes, desde que existem não existem problemas de compatibilidade entre versões de origem e de destino.

1. Certifique-se de que a aplicação é compatível com a nova versão, conforme mostrado nas notas de lançamento e de matriz de compatibilidade do HBase. Teste a aplicação num cluster com a versão de destino do HDInsight e HBase.

2. [Configurar um novo cluster de HDInsight destino](../hdinsight-hadoop-provision-linux-clusters.md) com a mesma conta de armazenamento, mas com um nome de contentor diferente:

    ![Utilizar a mesma conta de armazenamento, mas criar um contentor diferente](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Esvaziar o cluster de HBase de origem. Este é o cluster a partir do qual está a atualizar. HBase escreve dados de entrada para um arquivo de dentro da memória, chamado um _memstore_. Depois do memstore atinge um determinado tamanho, do memstore é removida da cache em disco para armazenamento de longa duração na conta de armazenamento do cluster. Ao eliminar o cluster antigo, os memstores sejam reciclados, potencial perda de dados. Descarregar manualmente memstore para cada tabela para o disco, execute o seguinte script. A versão mais recente deste script está do Azure [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh).

    ```bash
    #!/bin/bash
    
    #-------------------------------------------------------------------------------#
    # SCRIPT TO FLUSH ALL HBASE TABLES.
    #-------------------------------------------------------------------------------#
    
    LIST_OF_TABLES=/tmp/tables.txt
    HBASE_SCRIPT=/tmp/hbase_script.txt
    TARGET_HOST=$1
    
    usage ()
    {
        if [[ "$1" == "-h" ]] || [[ "$1" == "--help" ]]
        then
            cat << ...
    
    Usage: 
    
    $0 [hostname]
    
    Providing hostname is optional and not required when the script is executed within HDInsight cluster with access to 'hbase shell'.
    
    However hostname should be provided when executing the script as a script-action from HDInsight portal.
    
    For Example:
    
        1.  Executing script inside HDInsight cluster (where 'hbase shell' is 
            accessible):
    
            $0 
    
            [No need to provide hostname]
    
        2.  Executing script from HDinsight Azure portal:
    
            Provide Script URL.
    
            Provide hostname as a parameter (i.e. hn0, hn1 or wn2 etc.).
    ...
            exit
        fi
    }
    
    validate_machine ()
    {
        THIS_HOST=`hostname`
    
        if [[ ! -z "$TARGET_HOST" ]] && [[ $THIS_HOST  != $TARGET_HOST* ]]
        then
            echo "[INFO] This machine '$THIS_HOST' is not the right machine ($TARGET_HOST) to execute the script."
            exit 0
        fi
    }
    
    get_tables_list ()
    {
    hbase shell << ... > $LIST_OF_TABLES 2> /dev/null
        list
        exit
    ...
    }
    
    add_table_for_flush ()
    {
        TABLE_NAME=$1
        echo "[INFO] Adding table '$TABLE_NAME' to flush list..."
        cat << ... >> $HBASE_SCRIPT
            flush '$TABLE_NAME'
    ...
    }
    
    clean_up ()
    {
        rm -f $LIST_OF_TABLES
        rm -f $HBASE_SCRIPT
    }
    
    ########
    # MAIN #
    ########
    
    usage $1
    
    validate_machine
    
    clean_up
    
    get_tables_list
    
    START=false
    
    while read LINE 
    do 
        if [[ $LINE == TABLE ]] 
        then
            START=true
            continue
        elif [[ $LINE == *row*in*seconds ]]
        then
            break
        elif [[ $START == true ]]
        then
            add_table_for_flush $LINE
        fi
    
    done < $LIST_OF_TABLES
    
    cat $HBASE_SCRIPT
    
    hbase shell $HBASE_SCRIPT << ... 2> /dev/null
    exit
    ...
    
    ```
    
4. Pare ingestão para o cluster antigo do HBase.
5. Para garantir que quaisquer dados recentes do memstore é descarregado, execute novamente o script anterior.
6. Inicie sessão no Ambari no cluster antigo (https://OLDCLUSTERNAME.azurehdidnsight.net) e parar os serviços de HBase. Quando lhe for pedido para confirmar que pretende parar os serviços, selecione a caixa para ativar o modo de manutenção para o HBase. Para obter mais informações sobre ligar e com o Ambari, consulte [gerir clusters do HDInsight utilizando a IU da Web do Ambari](../hdinsight-hadoop-manage-ambari.md).

    ![No Ambari, separador de serviços, em seguida, HBase no menu da esquerda, clique em Parar em ações de serviço](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Verifique a ativar no modo de manutenção para a caixa de verificação do HBase, em seguida, confirmar](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Inicie sessão no Ambari no novo cluster de HDInsight. Alterar o `fs.defaultFS` definição HDFS para apontar para o nome do contentor utilizado pelo original cluster. Esta definição está sob **HDFS > folhas > Avançadas > Avançadas de site principal**.

    ![No Ambari, clique no separador de serviços, em seguida, HDFS no menu da esquerda, em seguida, no separador de cálculo de configurações, em seguida, no separador avançado por baixo](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Na Ambari, altere o nome do contentor](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. Guarde as alterações.
9. Reinicie os serviços todos requeridos conforme indicado pelo Ambari.
10. Ponto de aplicação para o novo cluster.

    > [!NOTE]
    > O DNS estático para as alterações de aplicação ao atualizar. Em vez de pré-programar este DNS, pode configurar um CNAME nas definições do seu nome de domínio DNS que aponta para o nome do cluster. Outra opção consiste em utilizar um ficheiro de configuração para a sua aplicação que pode atualizar se desejar.

11. Inicie a ingestão de para ver se tudo está a funcionar conforme esperado.
12. Se o novo cluster for satisfatório, elimine o cluster original.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o HBase e atualizar clusters do HDInsight, consulte os artigos seguintes:

* [Atualizar um cluster do HDInsight para uma versão mais recente](../hdinsight-upgrade-cluster.md)
* [Monitorizar e gerir o Azure HDInsight utilizando a IU da Web do Ambari](../hdinsight-hadoop-manage-ambari.md)
* [Componentes do Hadoop e versões](../hdinsight-component-versioning.md)
* [Otimizar as configurações com o Ambari](../hdinsight-changing-configs-via-ambari.md#hbase-optimization-with-the-ambari-web-ui)
