---
title: Personalizar clusters do HDInsight com ações de script - Azure
description: Adicione componentes personalizados para clusters do HDInsight baseado em Linux utilizar ações de script. Ações de script são scripts de Bash que podem ser utilizados para personalizar a configuração de cluster ou adicionar serviços adicionais e utilitários, como Hue, Solr ou R.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: e655624a30332630c28cbd555dac26098adeb68b
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976924"
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-actions"></a>Personalizar clusters do HDInsight baseado em Linux utilizar ações de script

HDInsight fornece um método de configuração chamado **ações de script** que invoca scripts personalizados para personalizar o cluster. Estes scripts são utilizados para instalar componentes adicionais e alterar as definições de configuração. Ações de script podem ser utilizadas durante ou após a criação do cluster.

> [!IMPORTANT]  
> A capacidade de utilizar as ações de script num cluster já esteja sendo executado só está disponível para clusters do HDInsight baseado em Linux.
>
> O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

Ações de script também podem ser publicadas no Azure Marketplace como uma aplicação do HDInsight. Para obter mais informações sobre aplicações do HDInsight, consulte [HDInsight publicar aplicações no Azure Marketplace](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Permissões

Se estiver a utilizar um cluster do HDInsight associados a um domínio, existem dois Ambari permissões necessárias ao utilizar as ações de script com o cluster:

* **AMBARI. EXECUTE\_PERSONALIZADA\_COMANDO**: A função de administrador do Ambari tenha esta permissão, por predefinição.
* **CLUSTER. EXECUTE\_PERSONALIZADA\_COMANDO**: O administrador de Cluster do HDInsight e o administrador de Ambari têm esta permissão por predefinição.

Para obter mais informações sobre como trabalhar com permissões com o HDInsight associado a um domínio, consulte [gerir clusters do HDInsight associados a um domínio](./domain-joined/apache-domain-joined-manage.md).

## <a name="access-control"></a>Controlo de acesso

Se não for o proprietário/administrador da sua subscrição do Azure, a conta tem de ter, pelo menos, **contribuinte** acesso ao grupo de recursos que contém o cluster do HDInsight.

Além disso, se estiver a criar um cluster do HDInsight, alguém com, pelo menos, **contribuinte** acesso à subscrição do Azure tem de ter registado previamente o fornecedor para HDInsight. O registo do fornecedor acontece quando um utilizador com o acesso Contribuidor para a subscrição cria um recurso na mesma pela primeira vez. Também pode ser feito sem a criação de um recurso, ao [registar o fornecedor com REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Para obter mais informações sobre como trabalhar com a gestão do acesso, veja os documentos seguintes:

* [Get started with access management in the Azure portal (Introdução à gestão do acesso no portal do Azure)](../role-based-access-control/overview.md)
* [Utilize atribuições de funções para gerir o acesso aos recursos de subscrição do Azure](../role-based-access-control/role-assignments-portal.md)

## <a name="understanding-script-actions"></a>Noções básicas sobre as ações de script

Uma ação de script é o script de Bash que é executado em nós num cluster do HDInsight. Seguem-se características e funcionalidades de ações de script.

* Devem ser armazenados num URI que é acessível a partir do cluster do HDInsight. Seguem-se as localizações de armazenamento possíveis:

    * Uma **armazenamento do Azure Data Lake** conta que seja acessível pelo cluster do HDInsight. Para obter informações sobre como utilizar o armazenamento do Azure Data Lake com o HDInsight, consulte [início rápido: Configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

        Quando utilizar um script armazenado no armazenamento do Data Lake, o formato URI é `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]  
        > O principal de serviço que HDInsight utiliza para aceder ao armazenamento do Data Lake tem de ter acesso de leitura para o script.

    * Um blob numa **conta de armazenamento do Azure** ou seja qualquer um dos principais ou adicionais conta de armazenamento para o cluster do HDInsight. HDInsight é concedido acesso a ambos os tipos de contas de armazenamento durante a criação do cluster.

    * Um serviço, como BLOBs do Azure, GitHub, OneDrive, Dropbox, etc de partilha de ficheiros público.

        Por exemplo URIs, consulte a [scripts de ações de script de exemplo](#example-script-action-scripts) secção.

        > [!WARNING]  
        > HDInsight suporta apenas BLOBs em contas de armazenamento do Azure com o escalão de desempenho padrão. 

* Pode ser restringido a **executar em determinados tipos de nó**, para nós principais do exemplo ou nós de trabalho.

* Pode ser **persistentes** ou **ad hoc**.

    **Persistentes** scripts são utilizados para personalizar novos nós de trabalho adicionados ao cluster através de operações de dimensionamento. Um script persistente também pode aplicar as alterações a outro tipo de nó, como um nó principal, quando ocorrem operações de dimensionamento.

  > [!IMPORTANT]  
  > Ações de script persistente tem de ter um nome exclusivo.

    **Ad hoc** scripts não são mantidos. Não são aplicadas a nós de trabalho adicionados ao cluster depois do script foi executado. Em seguida, pode promover um script ad hoc para um script persistente ou despromover um script persistente para um script ad hoc.

  > [!IMPORTANT]  
  > Ações de script utilizadas durante a criação do cluster automaticamente são mantidas.
  >
  > Scripts que falhas não são mantidos, mesmo que indica especificamente que deve ser.

* Pode aceitar **parâmetros** que são utilizados pelo script durante a execução.

* Executar com **privilégios no nível de raiz** em nós do cluster.

* Pode ser utilizada através da **portal do Azure**, **Azure PowerShell**, **CLI clássica do Azure**, ou **HDInsight .NET SDK**

O cluster mantém um histórico de todos os scripts que tenham sido executado. O histórico é útil quando precisa localizar o ID de um script para operações de promoção ou despromoção.

> [!IMPORTANT]  
> Não existe nenhuma forma automática para anular as alterações feitas por uma ação de script. Manual reverter as alterações ou fornecer um script que inverta-los.

### <a name="script-action-in-the-cluster-creation-process"></a>Ação de script do processo de criação do cluster

Ações de script utilizadas durante a criação do cluster são ligeiramente diferentes do script foi executadas ações num cluster existente:

* O script é **automaticamente persistente**.

* R **falha** no script a pode fazer com que o processo de criação de cluster efetuar a ativação.

O diagrama seguinte ilustra quando a ação de script é executada durante o processo de criação:

![Personalização de cluster do HDInsight e fases durante a criação do cluster][img-hdi-cluster-states]

O script é executado enquanto o HDInsight está a ser configurado. O script é executado em paralelo em todos os nós do cluster especificados e é executado com privilégios de raiz em nós.

> [!NOTE]  
> Pode efetuar operações como parar e iniciar serviços, incluindo serviços relacionados com o Apache Hadoop. Se parar os serviços, certifique-se de que o serviço de Ambari e outros serviços relacionados com o Hadoop em execução antes do script seja concluído. Estes serviços são necessários para determinar com êxito o estado de funcionamento e o estado do cluster enquanto ele está a ser criado.


Durante a criação do cluster, pode utilizar várias ações de script ao mesmo tempo. Estes scripts são invocados na ordem em que foram especificados.

> [!IMPORTANT]  
> Ações de script devem ser concluído em 60 minutos, ou o tempo limite. Durante o aprovisionamento do cluster, o script é executado simultaneamente com outros processos de instalação e configuração. Concorrência de recursos, tais como a largura de banda de CPU, rede ou de tempo pode fazer com que o script a demorar mais tempo a concluir do que o no seu ambiente de desenvolvimento.
>
> Para minimizar o tempo que demora a executar o script, evite tarefas como a transferência e compilar aplicativos de origem. Pré-compilar aplicativos e armazenar o binário no armazenamento do Azure.


### <a name="script-action-on-a-running-cluster"></a>Ação de script num cluster em execução

Uma falha num script foi executado numa execução já cluster não causa automaticamente o cluster alterar para um Estado com falhas. Uma vez concluída um script, deverá devolver o cluster para um Estado "em execução".

> [!IMPORTANT]  
> Mesmo que o cluster tem um Estado "em execução", o script com falha pode ter dividido coisas. Por exemplo, um script foi possível eliminar ficheiros necessários para o cluster.
>
> Executam as ações de scripts com privilégios de raiz. Certifique-se de que compreende o que faz um script antes de aplicá-lo ao seu cluster.

Ao aplicar um script para um cluster, o estado do cluster é alterado de **em execução** para **aceites**, em seguida, **configuração do HDInsight**e finalmente novamente para  **Executar** para scripts com êxito. O estado de script é registado no histórico de ações de script e pode usar essas informações para determinar se o script foi concluída com êxito ou falha. Por exemplo, o `Get-AzureRmHDInsightScriptActionHistory` cmdlet do PowerShell pode ser utilizado para ver o estado de um script. Devolve informações semelhantes ao seguinte texto:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> Se tiver alterado a palavra-passe de utilizador (administrador) do cluster depois do cluster foi criado, o script foi executadas ações contra este cluster poderá falhar. Se tiver quaisquer ações de script persistentes que nós de trabalho de destino, esses scripts podem falhar quando dimensionar o cluster.

## <a name="example-script-action-scripts"></a>Scripts de ações de script de exemplo

Scripts de ações de script podem ser usados por meio dos utilitários a seguir:

* Portal do Azure
* Azure PowerShell
* CLI clássica do Azure
* SDK de .NET do HDInsight

HDInsight fornece scripts para instalar os seguintes componentes em clusters do HDInsight:

| Nome | Script |
| --- | --- |
| **Adicionar uma conta de armazenamento do Azure** |https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. Ver [adicionar armazenamento adicional a um cluster do HDInsight](hdinsight-hadoop-add-storage.md). |
| **Instalar a Hue** |https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Ver [instalar e utilizar Hue em HDInsight clusters](hdinsight-hadoop-hue-linux.md). |
| **Instalar prontamente** |https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. Ver [instalar e utilizar o Presto no HDInsight clusters](hdinsight-hadoop-install-presto.md). |
| **Instalar o Solr** |https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. Ver [instalar e utilizar Solr nos HDInsight clusters](hdinsight-hadoop-solr-install-linux.md). |
| **Instalar o Giraph** |https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. Ver [instalar e utilizar Giraph no HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md). |
| **Pré-carregar bibliotecas do Hive** |https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. Ver [bibliotecas de adicionar o Hive em clusters do HDInsight](hdinsight-hadoop-add-hive-libraries.md). |
| **Instalar ou atualizar o Mono** | https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash. Ver [instalar ou atualizar o Mono no HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Utilize uma ação de script durante a criação do cluster

Esta seção fornece exemplos, sobre as diferentes formas que pode utilizar ações de script ao criar um cluster do HDInsight.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Utilize uma ação de script durante a criação do cluster no portal do Azure

1. Começar a criar um cluster, conforme descrito em [Apache Hadoop criar clusters no HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Durante a criação do cluster, vai deparar-se numa __resumo do Cluster__ página. Do __resumo do Cluster__ página, selecione a __editar__ ligar para __definições avançadas__.

    ![Ligação de definições avançadas](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Partir do __definições avançadas__ secção, selecione __ações de Script__. Partir do __ações de Script__ secção, selecione __+ submeter novo__

    ![Submeter uma ação de script nova](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Utilize o __selecionar um script__ entrada para selecionar um script criado previamente. Para utilizar um script personalizado, selecione __personalizados__ e, em seguida, forneça o __nome__ e __URI do script de Bash__ para o seu script.

    ![Adicionar um script sob a forma de script selecione](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    A tabela seguinte descreve os elementos no formulário:

    | Propriedade | Valor |
    | --- | --- |
    | Selecionar um script | Para utilizar o seu próprio script, selecione __personalizado__. Caso contrário, selecione um dos scripts fornecidos. |
    | Nome |Especifique um nome para a ação de script. |
    | URI do script de bash |Especifique o URI do script. |
    | HEAD/trabalho/Zookeeper |Especifique os nós (**Head**, **Worker**, ou **ZooKeeper**) no qual o script é executado. |
    | Parâmetros |Especifique os parâmetros, se necessário, o script. |

    Utilize o __manter esta ação de script__ entrada para se certificar de que o script é aplicado durante operações de dimensionamento.

5. Selecione __criar__ para guardar o script. Em seguida, pode utilizar __+ submeter novo__ para adicionar outro script.

    ![Várias ações de script](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Quando tiver terminado a adição de scripts, utilize o __selecionar__ botão e, em seguida, o __próxima__ botão para retornar ao __resumo do Cluster__ secção.

3. Para criar o cluster, selecione __Create__ partir do __resumo do Cluster__ seleção.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Utilize uma ação de script a partir de modelos do Azure Resource Manager

Ações de script podem ser utilizadas com modelos Azure Resource Manager. Por exemplo, veja [ https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/ ](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/).

Neste exemplo, a ação de script é adicionada usando o seguinte código:

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

Para obter informações sobre como implementar um modelo, veja os documentos seguintes:

* [Implementar recursos com modelos e o Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Implementar recursos com modelos e a CLI do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Utilize uma ação de script durante a criação do cluster do Azure PowerShell

Nesta secção, vai utilizar o [Add-AzureRmHDInsightScriptAction](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightscriptaction) cmdlet para invocar scripts para personalizar um cluster. Antes de continuar, certifique-se de ter instalado e configurado o Azure PowerShell. Para obter informações sobre como configurar uma estação de trabalho para executar os cmdlets do PowerShell do HDInsight, consulte [instalar e configurar o Azure PowerShell](/powershell/azure/overview).

O script a seguir demonstra como aplicar uma ação de script ao criar um cluster com o PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Pode demorar vários minutos antes do cluster ser criado.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>Utilize uma ação de script durante a criação do cluster do HDInsight .NET SDK

O SDK de .NET do HDInsight fornece bibliotecas de cliente que torna mais fácil trabalhar com o HDInsight a partir de uma aplicação .NET. Para obter um exemplo de código, consulte [baseado em Linux criar clusters no HDInsight com o SDK .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Aplicar uma ação de script para um cluster em execução

Nesta secção, saiba como aplicar ações de script para um cluster em execução.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Aplicar uma ação de script para um cluster em execução a partir do portal do Azure

Partir do [portal do Azure](https://portal.azure.com):

1. No menu à esquerda, selecione **todos os serviços**.

1. Sob **ANALYTICS**, selecione **clusters do HDInsight**.

1. Selecione o cluster na lista, que irá abrir a vista predefinida.

1. A vista predefinida, em **configurações**, selecione **ações de Script**.

1. Na parte superior do **ações de Script** página, selecione **+ submeter novo**.

    ![Adicionar um script para um cluster em execução](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Utilize o __selecionar um script__ entrada para selecionar um script criado previamente. Para utilizar um script personalizado, selecione __personalizados__ e, em seguida, forneça o __nome__ e __URI do script de Bash__ para o seu script.

    ![Adicionar um script sob a forma de script selecione](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    A tabela seguinte descreve os elementos no formulário:

    | Propriedade | Valor |
    | --- | --- |
    | Selecionar um script | Para utilizar o seu próprio script, selecione __personalizado__. Caso contrário, selecione um script fornecido. |
    | Nome |Especifique um nome para a ação de script. |
    | URI do script de bash |Especifique o URI do script. |
    | HEAD/trabalho/Zookeeper |Especifique os nós (**Head**, **Worker**, ou **ZooKeeper**) no qual o script é executado. |
    | Parâmetros |Especifique os parâmetros, se necessário, o script. |

    Utilize o __manter esta ação de script__ entrada para se certificar de que o script é aplicado durante operações de dimensionamento.

5. Por último, utilize o **criar** botão para aplicar o script para o cluster.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Aplicar uma ação de script para um cluster em execução a partir do Azure PowerShell

Antes de continuar, certifique-se de ter instalado e configurado o Azure PowerShell. Para obter informações sobre como configurar uma estação de trabalho para executar os cmdlets do PowerShell do HDInsight, consulte [instalar e configurar o Azure PowerShell](/powershell/azure/overview).

O exemplo seguinte demonstra como aplicar uma ação de script para um cluster em execução:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Quando a operação for concluída, receberá informações semelhantes ao seguinte texto:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Aplicar uma ação de script para um cluster em execução a partir da CLI do Azure

Antes de continuar, certifique-se de ter instalado e configurado a CLI do Azure. Para obter mais informações, consulte [instalar a CLI do Azure clássico](../cli-install-nodejs.md).

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. Para mudar para modo Azure Resource Manager, utilize o seguinte comando na linha de comandos:

    ```bash
    azure config mode arm
    ```

2. Utilize o seguinte para autenticar a sua subscrição do Azure.

    ```bash
    azure login
    ```

3. Utilize o seguinte comando para aplicar uma ação de script para um cluster em execução

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    Se omitir os parâmetros para este comando, lhe for pedido para os mesmos. Se o script especificar com `-u` aceita parâmetros, pode especificá-las usando o `-p` parâmetro.

    São tipos de nó válido `headnode`, `workernode`, e `zookeeper`. Se o script deve ser aplicado a vários tipos de nó, especificar os tipos separados por um ';'. Por exemplo, `-n headnode;workernode`.

    Para manter o script, adicione o `--persistOnSuccess`. Pode também manter o script mais tarde, utilizando `azure hdinsight script-action persisted set`.

    Depois de concluída a tarefa, recebe um resultado semelhante ao seguinte texto:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-using-rest-api"></a>Aplicar uma ação de script para um cluster em execução com a REST API

Ver [executar ações de script num cluster em execução](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Aplicar uma ação de script para um cluster em execução a partir do SDK de .NET do HDInsight

Para obter um exemplo com o SDK .NET para aplicar os scripts a um cluster, consulte [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Ver o histórico, promover e despromover ações de script

### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure

1. Inicie sessão para o [portal do Azure](https://portal.azure.com).

1. No menu à esquerda, selecione **todos os serviços**.

1. Sob **ANALYTICS**, selecione **clusters do HDInsight**.

1. Selecione o cluster na lista, que irá abrir a vista predefinida.

1. A vista predefinida, em **configurações**, selecione **ações de Script**.

4. Um histórico de scripts para este cluster é apresentado na secção de ações de script. Estas informações incluem uma lista de scripts persistentes. A captura de ecrã abaixo, pode ver que o Solr script foi executado neste cluster. A captura de ecrã não mostra todos os scripts persistentes.

    ![Secção de ações de script](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. A seleção de um script do histórico de apresenta a secção de propriedades para este script. Na parte superior do ecrã, pode voltar a executar o script ou promovê-lo.

    ![Propriedades de ações de script](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Também pode utilizar o **...**  à direita de entradas na seção de ações de script para efetuar ações.

    ![Ações de script... utilização](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Utilizar o Azure PowerShell

| Utilize o seguinte... | Para... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Obter informações sobre ações de script persistentes |
| Get-AzureRmHDInsightScriptActionHistory |Obter um histórico das ações de script aplicados ao cluster, ou de detalhes de um script específico |
| Set-AzureRmHDInsightPersistedScriptAction |Promove uma ação de script ad hoc para uma ação de script persistentes |
| Remove-AzureRmHDInsightPersistedScriptAction |Despromove de uma ação de script persistentes a uma ação ad hoc |

> [!IMPORTANT]  
> Usando `Remove-AzureRmHDInsightPersistedScriptAction` não anular as ações executadas por um script. Este cmdlet Remove apenas o sinalizador persistente.

O script de exemplo seguinte demonstra como utilizar os cmdlets para promover, em seguida, despromover um script.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-the-azure-classic-cli"></a>Com a CLI clássica do Azure

| Utilize o seguinte... | Para... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Obter uma lista de ações de script persistentes |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Obter informações sobre uma ação de script persistentes específico |
| `azure hdinsight script-action history list <clustername>` |Obter um histórico das ações de script aplicadas ao cluster |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Obter informações sobre uma ação de script específico |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Promove uma ação de script ad hoc para uma ação de script persistentes |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Despromove de uma ação de script persistentes a uma ação ad hoc |

> [!IMPORTANT]  
> Usando `azure hdinsight script-action persisted delete` não anular as ações executadas por um script. Este cmdlet Remove apenas o sinalizador persistente.

### <a name="using-the-hdinsight-net-sdk"></a>Com o SDK de .NET do HDInsight

Para obter um exemplo com o SDK .NET para recuperar o histórico de script a partir de um cluster, promover ou despromover scripts, consulte [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]  
> Este exemplo também demonstra como instalar uma aplicação do HDInsight com o SDK .NET.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Suporte para o software de código aberto utilizada nos clusters do HDInsight

O serviço do Microsoft Azure HDInsight utiliza um ecossistema de tecnologias de open source formado em torno do Apache Hadoop. Microsoft Azure fornece um nível geral de suporte para tecnologias de código aberto. Para obter mais informações, consulte a **âmbito de suporte** secção a [site FAQ de suporte do Azure](https://azure.microsoft.com/support/faq/). O serviço HDInsight fornece um nível adicional de suporte para componentes internos.

Existem dois tipos de componentes de código-fonte aberto que estão disponíveis no serviço HDInsight:

* **Componentes internos** -esses componentes estão pré-instaladas em clusters do HDInsight e fornecer a funcionalidade principal do cluster. Por exemplo, [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) ResourceManager, o ramo do idioma de consulta ([HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)) e o [Apache Mahout](https://mahout.apache.org/) biblioteca pertencem a essa categoria. Uma lista completa dos componentes de cluster está disponível no [quais são as novidades nas versões de cluster do Hadoop fornecidas pelo HDInsight](hdinsight-component-versioning.md).
* **Componentes personalizados** -, como um utilizador do cluster, pode instalar ou utilizar na sua carga de trabalho qualquer componente disponível na Comunidade ou criado por si.

> [!WARNING]  
> Componentes fornecidos com o cluster do HDInsight são totalmente suportadas. Support da Microsoft ajuda a isolar e resolver problemas relacionados com esses componentes.
>
> Componentes personalizados recebem suporte comercialmente razoável para ajudá-lo a resolver o problema. Suporte da Microsoft poderá conseguir resolver o problema ou eles poderão pedir-lhe para interagir com os canais disponíveis para as tecnologias de código-fonte aberto, onde os conhecimentos aprofundados para essa tecnologia é encontrado. Por exemplo, há muitos sites de Comunidade que podem ser utilizados, como: [Fórum do MSDN para o HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Também projetos Apache tem sites de projeto no [ https://apache.org ](https://apache.org), por exemplo: [Hadoop](https://hadoop.apache.org/).

O serviço HDInsight fornece várias formas de utilizar componentes personalizados. O mesmo nível de suporte aplica-se, independentemente de como um componente utilizado ou instalado no cluster. A lista seguinte descreve a mais comum formas que componentes personalizados podem ser utilizados em clusters do HDInsight:

1. Submissão de tarefas - Hadoop ou outros tipos de tarefas que executarem ou usam componentes personalizados pode ser submetida para o cluster.

2. Personalização de cluster - durante a criação do cluster, pode especificar definições adicionais e componentes personalizados que são instalados em nós do cluster.

3. Exemplos - para populares componentes personalizados, Microsoft e outras pessoas podem fornecer exemplos de como esses componentes podem ser usados nos clusters do HDInsight. Estes exemplos são fornecidos sem suporte.

## <a name="troubleshooting"></a>Resolução de problemas

Pode utilizar a IU web do Ambari para ver informações registadas pelo ações de script. Se o script falhar durante a criação do cluster, os registos também estão disponíveis na conta de armazenamento predefinida associada ao cluster. Esta seção fornece informações sobre como obter os registos com ambas estas opções.

### <a name="using-the-apache-ambari-web-ui"></a>Com a IU do Apache Ambari Web

1. No seu browser, navegue até https://CLUSTERNAME.azurehdinsight.net. Substitua CLUSTERNAME com o nome do cluster do HDInsight.

    Quando lhe for pedido, introduza o nome de conta de administrador (administrador) e a palavra-passe para o cluster. Poderá ter de reintroduzir as credenciais de administrador num formulário da web.

2. Na barra de na parte superior da página, selecione o **ops** entrada. É apresentada uma lista de operações atuais e anteriores, executadas no cluster através do Ambari.

    ![Barra de interface do Usuário de web de Ambari com ops selecionado](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Encontre as entradas que tenham **execute\_customscriptaction** no **operações** coluna. Estas entradas são criadas quando executam as ações de script.

    ![Captura de ecrã das operações](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    Para ver o resultado STDOUT e STDERR, selecione a entrada de run\customscriptaction e faça uma busca as ligações. Esta saída é gerada quando o script é executado e pode conter informações úteis.

### <a name="access-logs-from-the-default-storage-account"></a>Registos de acesso da conta de armazenamento predefinida

Se a criação do cluster falhar devido a um erro de script, os registos são mantidos na conta de armazenamento de cluster.

* Os registos de armazenamento estão disponíveis em `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Captura de ecrã das operações](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    Neste diretório, os registos são organizados em separado para o nó principal, workernode e nós zookeeper. Alguns exemplos incluem:

    * **Nó principal** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Nó de trabalho** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Nó do zookeeper** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Todos os stdout e stderr do anfitrião correspondente é carregado para a conta de armazenamento. Há um **saída -\*. txt** e **erros -\*. txt** para cada ação de script. O ficheiro de saída-*. txt contém informações sobre o URI do script que tem de ser executado no anfitrião. O texto seguinte é um exemplo dessas informações:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* É possível que repetidamente criar um cluster de ação de script com o mesmo nome. Nesse caso, pode distinguir os registos relevantes com base no nome da pasta data. Por exemplo, a estrutura de pastas para um cluster (mycluster) criado em datas diferentes é semelhante às seguintes entradas de registo:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Se criar um cluster de ação de script com o mesmo nome no mesmo dia, pode utilizar o prefixo exclusivo para identificar os ficheiros de registo relevantes.

* Se criar um cluster perto 12:00 AM (meia-noite), é possível que os ficheiros de registo que abrangem dois dias. Nesses casos, verá duas pastas de data diferentes para o mesmo cluster.

* Carregar ficheiros de registo para o contentor predefinido poderá demorar até 5 minutos, especialmente para grandes clusters. Então, se pretender aceder aos registos, não deve imediatamente excluir o cluster se uma ação de script falhar.

### <a name="ambari-watchdog"></a>Watchdog de Ambari

> [!WARNING]  
> Não altere a palavra-passe para o Watchdog de Ambari (hdinsightwatchdog) no seu cluster do HDInsight baseado em Linux. Alterar a palavra-passe para esta conta quebra a capacidade de executar novas ações de script no cluster do HDInsight.

### <a name="cant-import-name-blobservice"></a>Não é possível importar o nome do BlobService

__Os sintomas__: A ação de script falhar. Texto semelhante ao seguinte erro é exibido quando vê a operação num Ambari:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Causa__: Este erro ocorre se atualizar o cliente de armazenamento do Azure de Python que está incluído com o cluster do HDInsight. HDInsight espera que o cliente de armazenamento do Azure 0.20.0.

__Resolução__: Para resolver este erro, se conectar manualmente para cada nó de cluster com `ssh` e utilize o seguinte comando para reinstalar a versão de cliente de armazenamento correto:

```bash
sudo pip install azure-storage==0.20.0
```

Para obter informações sobre como ligar ao cluster com SSH, veja [utilizar o SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Histórico não mostra os scripts utilizados durante a criação do cluster

Se o seu cluster tiver sido criado antes de 15 de Março de 2016, poderá não ver uma entrada no histórico de ações de script. Redimensionar o cluster faz com que os scripts sejam apresentados no histórico de ações de script.

Existem duas exceções:

* Se o cluster tiver sido criado antes de 1 de Setembro de 2015. Esta data é quando as ações de script foram introduzidas. Qualquer cluster criado antes desta data poderia não ter usado a ações de script para a criação do cluster.

* Se utilizou várias ações de script durante a criação do cluster e utilizado o mesmo nome para vários scripts, ou o mesmo nome, mesmo URI, mas diferentes parâmetros para vários scripts. Nestes casos, recebe o erro seguinte:

    Nenhum script novas ações podem ser executado neste cluster devido a nomes de script em conflito em scripts existentes. Os nomes de script fornecidos em cluster criar tem de ser exclusivos. Scripts existentes são executadas no redimensionamento.

## <a name="next-steps"></a>Passos Seguintes

* [Desenvolver scripts de ações de script para o HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Instalar e utilizar o Apache Solr nos clusters do HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Instalar e usar o Apache Giraph nos clusters do HDInsight](hdinsight-hadoop-giraph-install-linux.md)
* [Adicionar armazenamento adicional a um cluster do HDInsight](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fases durante a criação do cluster"
