---
title: Gerir clusters do Apache Hadoop no HDInsight com o portal do Azure
description: Saiba como criar e gerir clusters do HDInsight com o portal do Azure.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: hrasheed
ms.openlocfilehash: c615188e5f198cb8707b12643e0942e8d8821983
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725187"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Gerir clusters do Apache Hadoop no HDInsight com o portal do Azure

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Utilizar o [portal do Azure][azure-portal], pode gerir [Apache Hadoop](https://hadoop.apache.org/) clusters no Azure HDInsight. Utilize o Seletor de separador acima para obter informações sobre como gerir clusters do Hadoop no HDInsight com outras ferramentas.

**Pré-requisito**

Para seguir os passos neste artigo, terá uma **subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-azure-portal"></a>Abra o portal do Azure
1. Inicie sessão em [https://portal.azure.com](https://portal.azure.com).
2. Depois de abrir o portal, pode:

   * Clique em **criar um recurso** no menu à esquerda para criar um novo cluster:

       ![novo botão de cluster do HDInsight](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)

       Introduza **HDInsight** na **pesquisar no Marketplace**, clique em **HDInsight**e, em seguida, clique em **criar**.

   * Clique em **Clusters do HDInsight** no menu à esquerda para listar os clusters existentes:

       ![Botão de cluster HDInsight portal do Azure](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Se não vir a **clusters do HDInsight** botão e, em seguida, clique em **clusters do HDInsight** sob o **inteligência + análise** secção.


## <a name="create-clusters"></a>Criar clusters
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight funciona com componentes de uma ampla gama de Hadoop. Para obter a lista dos componentes que são verificadas e suportados, consulte [qual a versão do Apache Hadoop é no Azure HDInsight?](hdinsight-component-versioning.md) Para informações de criação do cluster geral, consulte [Apache Hadoop criar clusters no HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Requisitos do controlo de acesso

Tem de especificar uma subscrição do Azure quando criar um cluster do HDInsight. O cluster pode ser criado num novo grupo de recursos do Azure ou num grupo de recursos existente. Pode utilizar os seguintes passos para verificar as suas permissões para criar clusters do HDInsight:

- Para criar um novo grupo de recursos:

    1. Inicie sessão no [portal do Azure](https://portal.azure.com).
    2. Clique em **subscrição** no menu à esquerda. Ele tem um ícone de chave amarelo. Deverá ver uma lista de subscrições.
    3. Clique na subscrição que utilizar para criar clusters. 
    4. Clique em **as minhas permissões**.  Ele mostra sua [função](../role-based-access-control/built-in-roles.md) na subscrição. Tem de, pelo menos, acesso de Contribuidor para criar o cluster do HDInsight.

- Para utilizar um grupo de recursos existente:

    1. Inicie sessão no [portal do Azure](https://portal.azure.com).
    2. Clique em **grupos de recursos** no menu à esquerda para listar os grupos de recursos.
    3. Clique no grupo de recursos que pretende utilizar para criar o cluster do HDInsight.
    4. Clique em **controlo de acesso (IAM)** e **atribuições de funções**e certifique-se de que (ou um grupo que pertencem a) tem, pelo menos, o acesso de Contribuidor ao grupo de recursos.

Se receber o erro NoRegisteredProviderFound ou o erro MissingSubscriptionRegistration, consulte [resolver erros comuns de implementação do Azure com o Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Listar e Mostrar clusters
1. Inicie sessão em [https://portal.azure.com](https://portal.azure.com).
2. Clique em **Clusters do HDInsight** no menu à esquerda para listar os clusters existentes. Se não vir **Clusters do HDInsight**, clique em **todos os serviços** primeiro.
3. Clique no nome do cluster. Se a lista de cluster for longa, pode utilizar o filtro no topo da página.
4. Clique num cluster a partir da lista para ver a página de descrição geral:

    ![O Azure essentials de cluster do HDInsight portal](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png) **menu de descrição geral:**
    * **Dashboard**: abre a IU web do Ambari do cluster.
    * **Secure Shell**: mostra as instruções para ligar ao cluster através de ligação de Secure Shell (SSH).
    * **Dimensionar Cluster**: permite-lhe alterar o número de nós de trabalho para este cluster.
    * **Mover**: move o cluster para outro grupo de recursos ou para outra subscrição.
    * **Eliminar**: elimina o cluster.

**Menu à esquerda:**
    * **Registos de atividades**: Mostrar e consulta os registos de atividades.
    * **Controlo de acesso (IAM)**: Utilize atribuições de funções.  Ver [utilize atribuições de funções para gerir o acesso aos recursos da sua subscrição do Azure](../role-based-access-control/role-assignments-portal.md).
    * **Etiquetas**: permite-lhe definir os pares de chave/valor para definir uma taxonomia personalizada dos seus serviços cloud. Por exemplo, pode criar uma chave denominada **projeto**e, em seguida, utilizar um valor para os serviços associados a um projeto específico.
* **Diagnosticar e resolver problemas**: exibir informações de resolução de problemas.
    * **Início Rápido**: apresenta informações que o ajuda a começar a utilizar o HDInsight.
    * **Ferramentas para o HDInsight**: ferramentas relacionadas de informações de ajuda para o HDInsight.
**Definições**
* **Tamanho do cluster**: verificação, aumentar e diminuir o número de nós de trabalho do cluster. Ver [dimensionar clusters](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Limites de quota**: apresentar os núcleos utilizados e disponíveis para a sua subscrição.
    * **SSH + início de sessão do Cluster**: mostra as instruções para ligar ao cluster através de ligação de Secure Shell (SSH). Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
* **Geração 1 do Data Lake Store**: configurar o acesso de geração 1 do Data Lake Store.  Ver [início rápido: configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
    * **Contas de armazenamento**: ver as contas de armazenamento e as chaves. As contas de armazenamento são configuradas durante o processo de criação do cluster.
    * **Aplicativos**: HDInsight de adicionar ou remover aplicações.  Ver [instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).
    * **Ações de script**: scripts de Bash de executar no cluster. Ver [HDInsight baseado em Linux personalizar clusters com ação de Script](hdinsight-hadoop-customize-cluster-linux.md).
    * **Parceiro do HDInsight**: Adicionar/remover o atual parceiro do HDInsight.
    * **Propriedades**: ver as propriedades do cluster.
* **Bloqueia**: adicionar um bloqueio para impedir que o cluster que está a ser modificada ou eliminada.
    * **Script de automação**: apresentar e exportar o modelo Azure Resource Manager para o cluster. Atualmente, só pode exportar a conta de armazenamento do Azure dependente. Ver [Apache Hadoop baseado em Linux criar clusters no HDInsight utilizando modelos Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
**Monitorização**
* **Alters**: gerir os alertas e ações.
    * **Métricas**: monitorizar as métricas de cluster no Azure Log Analytics.
* **Definições de diagnóstico**: as definições em onde pretende armazenar as métricas de diagnóstico **suporte + resolução de problemas**
    * **Estado de funcionamento do recurso**: veja [descrição geral do Estado de funcionamento de recursos do Azure](../service-health/resource-health-overview.md).
    * **Novo pedido de suporte**: permite-lhe criar um pedido de suporte com o suporte da Microsoft.
    
6. Clique em **propriedades**:

    As propriedades são:

   * **Nome de anfitrião**: nome do Cluster.
   * **URL do cluster**: O URL para a interface de web do Ambari.
   * **Secure shell (SSH)**: O nome de utilizador e nome do anfitrião para utilizar em aceder ao cluster através de SSH.
   * **Estado**: um dos: abortada, aceites, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operacional, executar, erro, eliminar, eliminado, excedido, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued ou ClusterCustomization.
   * **Região**: localização do Azure. Para obter uma lista das localizações do Azure suportadas, consulte a **região** caixa de lista suspensa na [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Data de criação**: A data do cluster foi implementado.
   * **Sistema operativo**: qualquer **Windows** ou **Linux**.
   * **Tipo de**: Hadoop, HBase, Storm, Spark.
   * **Versão**. Ver [versões do HDInsight](hdinsight-component-versioning.md).
   * **Subscrição**: nome da subscrição.
   * **Origem de dados predefinida**: O sistema de ficheiros de cluster predefinido.
   * **Tamanho de nós de trabalho**: O tamanho da VM selecionado de nós de trabalho.
   * **Siga o tamanho do nó**: O tamanho da VM selecionado dos nós principais.
   * **Rede virtual**: O nome da rede Virtual que o cluster é implementado, se um tiver sido selecionado no momento da implementação.

## <a name="delete-clusters"></a>Eliminar clusters
Eliminar um cluster não elimina a conta de armazenamento predefinida nem todas as contas de armazenamento ligada. Pode voltar a criar o cluster ao utilizar as mesmas contas de armazenamento e o mesmo metastores. Recomendamos que utilize um novo contentor de BLOBs predefinido quando voltar a criar o cluster.

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Clusters do HDInsight** no menu à esquerda. Se não vir **Clusters do HDInsight**, clique em **todos os serviços** primeiro.
3. Clique no cluster que pretende eliminar.
4. Clique em **eliminar** no menu superior e, em seguida, siga as instruções.

Consulte também [colocar em pausa/encerramento clusters](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Adicionar mais contas de armazenamento

Pode adicionar mais contas de armazenamento do Azure e contas de Azure Data Lake Store depois de um cluster é criado. Para obter mais informações, veja [Adicionar mais contas de armazenamento ao HDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Dimensionar clusters
O funcionalidade de dimensionamento do cluster permite-lhe alterar o número de nós de trabalho utilizado por um cluster de HDInsight do Azure, sem ter de voltar a criar o cluster.

> [!NOTE]
> Apenas clusters com o HDInsight versão 3.1.3 ou superior são suportadas. Se tiver a certeza de que a versão do seu cluster, consulte a página de propriedades.  Ver [listar e Mostrar clusters](#list-and-show-clusters).
>
>
**Dimensionar clusters**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Clusters do HDInsight** no menu à esquerda.
3. Clique no cluster que pretende dimensionar.
3. Clique em **Dimensionar Cluster**.
4. Introduza **nós de número de trabalho**. O limite no número de nós de cluster varia entre subscrições do Azure. Pode contactar o suporte de faturação para aumentar o limite.  As informações de custo refletem as alterações efetuadas para o número de nós.

    ![Escala de spark do storm de hbase do HDInsight hadoop](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

O impacto de alterar o número de nós de dados varia para cada tipo de cluster suportada pelo HDInsight:

* Apache Hadoop

    Pode facilmente aumentar o número de nós de trabalho num cluster do Hadoop que está a ser executado sem afetar todas as tarefas em execução ou pendentes. Também podem ser submetidas a novas tarefas enquanto a operação estiver em curso. Falhas numa operação de dimensionamento são processadas corretamente para que o cluster está sempre deixado no estado funcional.

    Quando um cluster do Hadoop é reduzido, reduzindo o número de nós de dados, alguns dos serviços do cluster são reiniciados. Esse comportamento faz com que tudo em execução e tarefas pendentes a falhar após a conclusão da operação de dimensionamento. Pode, no entanto, volte a submeter as tarefas depois de concluída a operação.
* Apache HBase

    Pode facilmente adicionar ou remover nós ao cluster do HBase durante a execução. Servidores regionais são balanceadas automaticamente dentro de alguns minutos a concluir a operação de dimensionamento. No entanto, também manualmente pode balancear servidores regionais iniciando sessão nó principal do cluster e executar os seguintes comandos a partir de uma janela de linha de comandos:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

    Para obter mais informações sobre como utilizar a shell de HBase, consulte [começar com um exemplo de Apache HBase no HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Pode facilmente adicionar ou remover nós de dados ao seu cluster do Storm, enquanto estiver em execução. No entanto, após a conclusão bem-sucedida da operação de dimensionamento, terá de reequilibrar a topologia.

    Reequilíbrio pode ser feito de duas formas:

  * IU da web do Storm
  * Ferramenta de interface de linha de comandos (CLI)

    Consulte a [documentação do Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) para obter mais detalhes.

    A IU da web de Storm está disponível no cluster do HDInsight:

    ![Reequilíbrio de dimensionamento do HDInsight Storm](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Eis um exemplo de comando da CLI para reequilibrar a topologia do Storm:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```


## <a name="pauseshut-down-clusters"></a>Colocar em pausa/encerramento clusters

A maioria das tarefas do Hadoop é tarefas de lote que só são executadas ocasionalmente. Para a maioria dos clusters do Hadoop, há grandes períodos de tempo que o cluster não está a ser utilizado para processamento. Com o HDInsight, os dados são armazenados no Storage do Azure, pelo que pode eliminar um cluster em segurança quando este não está a ser utilizado.
Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados.

Existem várias formas, pode programar o processo:

* Utilizador do Azure Data Factory. Ver [criar sob demanda baseados em Linux Apache Hadoop clusters no HDInsight com o Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) para a criação de HDInsight a pedido dos serviços ligados.
* Utilize o Azure PowerShell.  Ver [analisar dados de atraso de voo](hdinsight-analyze-flight-delay-data.md).
* Utilize a CLI clássica do Azure. Ver [clusters do HDInsight gerir com o CLI clássica do Azure](hdinsight-administer-use-command-line.md).
* Utilize o HDInsight .NET SDK. Ver [Apache Hadoop submeter tarefas](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Para obter informações sobre preços, consulte [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/). Para eliminar um cluster a partir do Portal, veja [eliminar clusters](#delete-clusters)

## <a name="move-cluster"></a>Mover o cluster

Pode mover um cluster do HDInsight para outro grupo de recursos do Azure ou noutra subscrição.  Ver [listar e Mostrar clusters](#list-and-show-clusters).

## <a name="upgrade-clusters"></a>Atualizar clusters

Ver [cluster de HDInsight atualizar para uma versão mais recente](./hdinsight-upgrade-cluster.md).

## <a name="open-the-apache-ambari-web-ui"></a>Abra a IU web do Apache Ambari

Ambari proporciona uma gestão web intuitivo e fácil de usar Hadoop da interface do Usuário apoiada por suas APIs RESTful. Ambari permite que os administradores de sistema gerir e monitorizar clusters do Hadoop.

1. Abra um cluster do HDInsight a partir do portal do Azure.  Ver [listar e Mostrar clusters](#list-and-show-clusters).
2. Clique em **Dashboard do Cluster**.

    ![Menu de cluster de Hadoop do HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu.png)

1. Introduza o nome de utilizador do cluster e a palavra-passe.  O nome de utilizador de cluster predefinido é _administrador_. A web do Ambari interface do Usuário é semelhante a:

    ![Interface do Usuário da Web do Ambari do HDInsight Hadoop](./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-ambari-web-ui.png)

Para obter mais informações, consulte [clusters do HDInsight gerir com a IU do Apache Ambari Web](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Alterar palavras-passe
Um cluster do HDInsight pode ter duas contas de usuário. O HDInsight (também conhecido como conta de utilizador do cluster Conta de utilizador HTTP) e a conta de utilizador SSH são criados durante o processo de criação. Pode utilizar a IU web do Ambari para alterar o nome de utilizador de conta de utilizador de cluster e palavra-passe e as ações de script para alterar a conta de utilizador SSH

### <a name="change-the-cluster-user-password"></a>Alterar a palavra-passe de utilizador do cluster
Pode utilizar a interface do Usuário da Web de Ambari para alterar a palavra-passe de utilizador do Cluster. Para iniciar sessão Ambari, tem de utilizar o nome de utilizador de cluster existentes e a palavra-passe.

> [!NOTE]
> Alterar a palavra-passe de utilizador (administrador) do cluster pode fazer com que as ações de script ser executadas em relação a este cluster para efetuar a ativação. Se tiver quaisquer ações de script persistentes que nós de trabalho de destino, esses scripts podem falhar ao adicionar nós ao cluster através de redimensionar operações. Para obter mais informações sobre as ações de script, consulte [HDInsight personalizar clusters com ações de script](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Inicie sessão na interface do usuário da Web de Ambari com as credenciais de utilizador de cluster do HDInsight. O nome de utilizador predefinido é **admin**. O URL é **https://&lt;nome do Cluster de HDInsight > azurehdinsight.net**.
2. Clique em **administrador** no menu superior e, em seguida, clique em "Gerir Ambari".
3. No menu à esquerda, clique em **utilizadores**.
4. Clique em **administrador**.
5. Clique em **alterar palavra-passe**.

Ambari, em seguida, altera a palavra-passe em todos os nós do cluster.

### <a name="change-the-ssh-user-password"></a>Alterar a palavra-passe de utilizador do SSH
1. Com um editor de texto, guarde o seguinte texto como um ficheiro denominado **changepassword.sh**.

    > [!IMPORTANT]
    > Tem de utilizar um editor que utiliza LF como o fim da linha. Se o editor de utilizar CRLF, em seguida, o script não funciona.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Carregue o ficheiro para uma localização de armazenamento que pode ser acessada a partir do HDInsight com um endereço de HTTP ou HTTPS. Por exemplo, um ficheiro pública armazenar como o armazenamento do OneDrive ou de Blobs do Azure. Guarde o URI (endereço HTTP ou HTTPS) para o ficheiro, conforme este URI é necessário no próximo passo.
3. A partir do portal do Azure, clique em **Clusters do HDInsight**.
4. Clique em seu cluster do HDInsight.
4. Clique em **ações de Script**.
4. Partir do **ações de Script** painel, selecione **submeter novo**. Quando o **submeter ação de script** painel que aparece, introduza as seguintes informações:

   | Campo | Valor |
   | --- | --- |
   | Nome |Alterar ssh palavra-passe |
   | URI do script de bash |O URI para o ficheiro de changepassword.sh |
   | Nós (Head, trabalho, Nimbus, Supervisor, Zookeeper, etc.) |✓ para todos os tipos de nó listados |
   | Parâmetros |Introduza o nome de utilizador SSH e, em seguida, a nova palavra-passe. Deve haver um espaço entre o nome de utilizador e a palavra-passe. |
   | Manter esta ação de script... |Deixe este campo desmarcada. |
5. Selecione **criar** para aplicar o script. Quando concluir o script, é possível ligar ao cluster através de SSH com a nova palavra-passe.

## <a name="grantrevoke-access"></a>Conceder/revogar acesso
Clusters do HDInsight tem os seguintes serviços da web HTTP (todos estes serviços tem pontos de extremidade RESTful):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Por predefinição, estes serviços são concedidos para o acesso. Pode revogar/conceder o acesso usando [CLI clássica do Azure](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) e [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Localizar o ID de subscrição

**Para localizar a IDs de subscrição do Azure**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **subscrições**. Cada subscrição tem um nome e um ID.

Cada cluster é associada a uma subscrição do Azure. A subscrição do ID é mostrado no cluster **essencial** mosaico. Ver [listar e Mostrar clusters](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Encontrar o grupo de recursos
No modo Azure Resource Manager, cada cluster do HDInsight é criado com um grupo do Azure Resource Manager. O grupo de Gestor de recursos que pertence um cluster é apresentado no:

* A lista de cluster tem um **grupo de recursos** coluna.
* Cluster **essencial** mosaico.  

Ver [listar e Mostrar clusters](#list-and-show-clusters).

## <a name="find-the-storage-accounts"></a>Localizar as contas de armazenamento

Clusters do HDInsight utilizam uma conta de armazenamento do Azure ou um Store do Azure Data Lake para armazenar dados. Cada cluster do HDInsight pode ter uma conta do storage predefinida e um número de contas de armazenamento ligadas. Para listar as contas de armazenamento, primeiro abrir o cluster no portal e, em seguida, clique em **contas de armazenamento**:

![Contas de armazenamento de cluster do HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-storage-accounts.png)

Na captura de ecrã anterior, existe uma __predefinição__ coluna que indica se a conta é a conta de armazenamento predefinida.

Para listar as contas do Data Lake Store, clique em **acesso do Data Lake Store** na captura de ecrã anterior.

## <a name="run-apache-hive-queries"></a>Executar consultas do Apache Hive
Não é possível executar tarefa do Hive diretamente a partir do portal do Azure, mas pode utilizar a vista do Hive na IU da Web do Ambari.

**Para executar consultas do Hive com o modo de exibição do Ambari Hive**

1. Inicie sessão na interface do usuário da Web de Ambari com as credenciais de utilizador de cluster do HDInsight. O nome de utilizador predefinido é **admin**. O URL é **https://&lt;nome do Cluster de HDInsight > azurehdinsight.net**.
2. Abrir vista do Hive, conforme mostrado na captura de ecrã seguinte:  

    ![Vista de hive do HDInsight](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)

3. Clique em **consulta** no menu superior.
4. Introduza uma consulta do Hive **Editor de consultas**e, em seguida, clique em **Execute**.

## <a name="monitor-jobs"></a>Monitorizar trabalhos
Ver [clusters do HDInsight gerir com a IU do Apache Ambari Web](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Procurar ficheiros
Utilizar o portal do Azure, pode procurar o conteúdo do contentor predefinido.

1. Inicie sessão em [https://portal.azure.com](https://portal.azure.com).
2. Clique em **Clusters do HDInsight** no menu à esquerda para listar os clusters existentes.
3. Clique no nome do cluster. Se a lista de cluster for longa, pode utilizar o filtro no topo da página.
4. Clique em **contas de armazenamento** no menu à esquerda do cluster.
5. Clique numa conta de armazenamento.
7. Clique nas **Blobs** mosaico.
8. Clique no nome de contentor predefinido.

## <a name="monitor-cluster-usage"></a>Monitorizar a utilização do cluster
O **utilização** seção do painel do cluster de HDInsight apresenta informações sobre o número de núcleos disponíveis para a sua subscrição para utilização com o HDInsight, bem como o número de núcleos alocados para este cluster e como eles são alocados para os nós dentro deste cluster. Ver [listar e Mostrar clusters](#list-and-show-clusters).

> [!IMPORTANT]
> Para monitorizar os serviços fornecidos pelo cluster do HDInsight, tem de utilizar a Web do Ambari ou a API de REST do Ambari. Para obter mais informações sobre a utilização do Ambari, consulte [clusters do HDInsight gerir com o Apache Ambari](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Ligar a um cluster

* [Utilizar o Apache Hive com o HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu algumas funções administrativas básicas. Para obter mais informações, consulte os artigos seguintes:

* [Administrar o HDInsight com o Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Administrar o HDInsight com a CLI clássica do Azure](hdinsight-administer-use-command-line.md)
* [Criar clusters do HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Leia mais sobre como utilizar a interface do Usuário do Apache Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Detalhes sobre como utilizar a API de REST do Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Utilizar o Apache Hive no HDInsight](hadoop/hdinsight-use-hive.md)
* [Utilizar o Apache Pig no HDInsight](hadoop/hdinsight-use-pig.md)
* [Utilizar o Apache Sqoop no HDInsight](hadoop/hdinsight-use-sqoop.md)
* [Introdução ao Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Qual a versão do Apache Hadoop é no Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Linha de comandos do Hadoop"
