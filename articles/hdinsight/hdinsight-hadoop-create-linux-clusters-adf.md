---
title: 'Tutorial: Criar clusters do Hadoop de demanda no HDInsight do Azure com o Data Factory | Documentos da Microsoft'
description: Saiba como criar clusters do Hadoop de demanda no HDInsight com o Azure Data Factory.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: a9e2e38c044501fdbc94d110c7ee46d6d5b2b3bb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440076"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Tutorial: Criar clusters do Hadoop de demanda no HDInsight com o Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Neste artigo, saiba como criar um cluster do Hadoop, a pedido, no HDInsight do Azure com o Azure Data Factory. Utilize, em seguida, pipelines de dados no Azure Data Factory para executar tarefas do Hive e eliminar o cluster. No final deste tutorial, irá aprender a operacionalizar um trabalho de grandes volumes de dados, executar em que a criação do cluster, a execução de tarefa e a exclusão de cluster são executadas com base numa agenda.

Este tutorial abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Criar uma conta do Storage do Azure
> * Compreender a atividade do Azure Data Factory
> * Criar uma fábrica de dados com o portal do Azure
> * Criar serviços ligados
> * Criar um pipeline
> * Acionar um pipeline
> * Monitorizar um pipeline
> * Verificar a saída

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Azure PowerShell. Para obter instruções, consulte [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Um principal de serviço do Azure Active Directory. Depois de criar o principal de serviço, certifique-se obter o **ID da aplicação** e **chave de autenticação** com as instruções no artigo ligado. Vai precisar destes valores mais tarde neste tutorial. Além disso, certifique-se de que o principal de serviço é um membro do *contribuinte* função da subscrição ou o grupo de recursos no qual o cluster é criado. Para obter instruções obter os valores necessários e atribua as funções certas, consulte [criar um Azure Active Directory, principal de serviço](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

Nesta secção, vai criar uma conta de armazenamento que será utilizada como armazenamento predefinido para o cluster do HDInsight que cria a pedido. Esta conta de armazenamento também contém o exemplo de script de HiveQL (**hivescript. hql**) que usar para simular uma tarefa do Hive de exemplo que é executada no cluster.

Esta secção utiliza um script do Azure PowerShell para criar a conta de armazenamento e copie os ficheiros necessários na conta de armazenamento. O script de exemplo do Azure PowerShell nesta secção, executa as seguintes tarefas:

1. Registos no Azure.
1. Cria um grupo de recursos do Azure.
1. Cria uma Conta de armazenamento do Azure.
1. Cria um contentor de BLOBs na conta de armazenamento
1. Copia o exemplo de script de HiveQL (**hivescript. hql**) o contentor de Blobs. O script está disponível em [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). O script de exemplo já está disponível em outro contentor de BLOBs público. O script do PowerShell abaixo efetua uma cópia desses arquivos para a conta de armazenamento do Azure cria.


**Para criar uma conta de armazenamento e copie os ficheiros com o Azure PowerShell:**
> [!IMPORTANT]
> Especifique nomes para o grupo de recursos do Azure e a conta de armazenamento do Azure que será criada pelo script.
> Anote **nome do grupo de recursos**, **nome da conta de armazenamento**, e **chave da conta de armazenamento** produzido pelo script. Precisa na próxima seção.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzureRmAccount
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Para verificar a criação de conta de armazenamento**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Selecione **grupos de recursos** no painel da esquerda.
1. Faça duplo clique o nome do grupo de recursos que criou no script do PowerShell. Se tiver muitos grupos de recursos listados, utilize o filtro.
1. Sobre o **recursos** mosaico, verá um recurso listado, a menos que partilha o grupo de recursos com outros projetos. Esse recurso é a conta de armazenamento com o nome que especificou anteriormente. Selecione o nome de conta de armazenamento.
1. Selecione o **Blobs** mosaicos.
1. Selecione o **adfgetstarted** contentor. Verá uma pasta denominada **hivescripts**.
1. Abra a pasta e certifique-se de que contém o ficheiro de script de exemplo **hivescript. hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Compreender a atividade do Azure Data Factory

[O Azure Data Factory](../data-factory/introduction.md) orquestra e automatiza o movimento e transformação de dados. O Azure Data Factory, pode criar um Hadoop do HDInsight cluster just-in-time para processar um setor de dados de entrada e eliminar o cluster quando o processamento é concluído. 

No Azure Data Factory, uma fábrica de dados pode ter um ou mais pipelines de dados. Um pipeline de dados tem uma ou mais atividades. Existem dois tipos de atividades:

* [Atividades de movimento de dados](../data-factory/copy-activity-overview.md) -utilizar atividades de movimento de dados para mover dados de um arquivo de dados de origem para um arquivo de dados de destino.
* [Atividades de transformação de dados](../data-factory/transform-data.md). Utilize atividades de transformação de dados para transformar/processar dados. Atividade do Hive do HDInsight é uma das atividades de transformação Data Factory suportadas. Utilize a atividade de transformação do ramo de registo neste tutorial.

Neste artigo, vai configurar a atividade de ramo de registo para criar um cluster de Hadoop do HDInsight a pedido. Quando a atividade é executada para processar dados, eis o que acontece:

1. Um cluster de Hadoop do HDInsight é criado automaticamente para just-in-time processar o setor. 

1. Os dados de entrada são processados ao executar um script de HiveQL no cluster. Neste tutorial, o script de HiveQL associado com a atividade do hive executa as seguintes ações:

    * Utiliza a tabela existente (*hivesampletable*) para criar outra tabela **HiveSampleOut**.
    * Preenche o **HiveSampleOut** tabela com apenas as colunas específicas em relação ao original *hivesampletable*.

1. O cluster de Hadoop do HDInsight é eliminado após o processamento é concluído e o cluster estar inativo durante o período de tempo (definição de timeToLive) configurado. Se o setor de dados seguinte está disponível para processamento com no tempo de inatividade timeToLive, do mesmo cluster é utilizado para processar o setor.  

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).

1. No portal do Azure, selecione **criar um recurso** > **dados + análise** > **Data Factory**.

    ![O Azure Data Factory no portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "no portal do Azure Data Factory")

1. Introduza ou selecione os valores, conforme mostrado na captura de ecrã seguinte:

    ![Criar fábrica de dados do Azure através do portal do Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "criar do Azure Data Factory com o portal do Azure")

    Introduza ou selecione os seguintes valores:
    
    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome** |  Introduza um nome para a fábrica de dados. Este nome tem de ser globalmente exclusivo.|
    |**Subscrição**     |  Selecione a sua subscrição do Azure. |
    |**Grupo de recursos**     | Selecione **utilizar existente** e, em seguida, selecione o grupo de recursos que criou com o script do PowerShell. |
    |**Versão**     | Selecione **V2 (pré-visualização)** |
    |**Localização**     | A localização é automaticamente definida para a localização que especificou ao criar o grupo de recursos anteriormente. Para este tutorial, a localização está definida como **E.U.A. Leste 2**. |
    

1. Selecione **afixar ao dashboard**e, em seguida, selecione **criar**. Irá ver um novo mosaico intitulado **A submeter a implementação** no dashboard do portal. Criar uma fábrica de dados pode demorar entre 2 a 4 minutos.

    ![Progresso da implementação de modelo](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "progresso da implementação de modelo") 
 
1. Depois de criar a fábrica de dados, o portal mostra a visão geral da fábrica de dados.

    ![Descrição geral do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "descrição geral da fábrica de dados do Azure")

1. Selecione **criar e monitorizar** para iniciar a fábrica de dados do Azure, criação e monitorização do portal.

## <a name="create-linked-services"></a>Criar serviços ligados

Nesta secção, vai criar dois serviços ligados na sua fábrica de dados.

- Um **serviço ligado do Armazenamento do Azure** que liga uma conta de armazenamento do Azure à fábrica de dados. Este armazenamento é utilizado pelo cluster do HDInsight a pedido. Também contém o script de ramo de registo que é executado no cluster.
- Um **serviço ligado do HDInsight a pedido**. O Azure Data Factory cria um cluster do HDInsight e executa o script de ramo de registo automaticamente. Em seguida, elimina o cluster do HDInsight depois de o cluster estar inativo durante um período de tempo pré-configurado.

###  <a name="create-an-azure-storage-linked-service"></a>Criar um serviço ligado do Armazenamento do Azure

1. No painel à esquerda do **Vamos começar** página, selecione a **editar** ícone.

    ![Criar um serviço ligado do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "criar um serviço ligado do Azure Data Factory")

1. Selecione **conexões** no canto inferior esquerdo da janela e em seguida, selecione **+ novo**.

    ![Criar ligações no Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "criar ligações no Azure Data Factory")

1. Na **novo serviço ligado** caixa de diálogo, selecione **armazenamento de Blobs do Azure** e, em seguida, selecione **continuar**.

    ![Serviço do Data Factory ligado do armazenamento do Azure de criar](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "serviço fábrica de dados ligado do armazenamento do Azure de criar")

1. Forneça um nome para o serviço ligado do armazenamento, selecione a conta de armazenamento do Azure que criou como parte do script do PowerShell e, em seguida, selecione **concluir**.

    ![Serviço ligado de fornecer o nome do armazenamento do Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "serviço ligado de fornecer o nome do armazenamento do Azure")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Crie um serviço ligado do HDInsight a pedido

1. Selecione o botão **+ Novo** novamente para criar outro serviço ligado.

1. Na janela **Novo Serviço Ligado**, selecione **Computação** > **Azure HDInsight** e, em seguida, **Continuar**.

    ![HDInsight criar serviço ligado para Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "HDInsight criar serviço ligado para Azure Data Factory")

1. Na **novo serviço ligado** janela, forneça os valores necessários.

    ![Serviço ligado de fornecer valores para o HDInsight](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "serviço ligado de fornecer valores para o HDInsight")

    Introduza os seguintes valores e deixe o resto como predefinição.

    | Propriedade | Descrição |
    | --- | --- |
    | Nome | Introduza um nome para o serviço ligado do HDInsight |
    | Tipo | Selecione **HDInsight a pedido** |
    | Serviço Ligado do Storage do Azure | Selecione o serviço ligado de armazenamento que criou anteriormente. |
    | Tipo de cluster | Selecione **hadoop** |
    | TTL | Forneça a duração para o qual pretende que o cluster do HDInsight para estar disponível antes de ser eliminado automaticamente.|
    | ID do principal de serviço | Forneça o ID da aplicação do principal de serviço do Azure Active Directory que criou como parte dos pré-requisitos |
    | Chave de principal de serviço | Forneça a chave de autenticação para o principal de serviço do Azure Active Directory |
    | Prefixo do nome do cluster | Forneça um valor que terá o prefixo para todos os tipos de cluster que são criados pela fábrica de dados |
    | Grupo de recursos | Selecione o grupo de recursos que criou como parte do script do PowerShell que utilizou anteriormente| 
    | Nome de utilizador SSH do cluster | Introduza um nome de utilizador SSH |
    | Palavra-passe SSH do cluster | Forneça uma palavra-passe do utilizador SSH |

    Selecione **Concluir**.

## <a name="create-a-pipeline"></a>Criar um pipeline

1. Selecione o botão **+** (mais) e, em seguida, selecione **Pipeline**.

    ![Criar um pipeline no Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "criar um pipeline na fábrica de dados do Azure")

1. Na **atividades** caixa de ferramentas, expanda **HDInsight**e arraste o **Hive** atividade para a superfície de desenho do pipeline. Na **gerais** separador, indique um nome para a atividade.

    ![Adicionar atividades ao pipeline do Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "adicionar atividades ao pipeline do Data Factory")

1. Certifique-se de que tem a atividade de ramo de registo selecionada, selecione o **Cluster do HDI** separador e para o **serviço ligado do HDInsight** pendente, selecione o serviço ligado que criou anteriormente para o HDInsight.

    ![Forneça os detalhes de cluster do HDInsight para o pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "HDInsight fornecem detalhes do cluster para o pipeline")

1. Selecione o **Script** separador e conclua os seguintes passos:

    a. Para **serviço ligado de Script**, selecione **HDIStorageLinkedService**. Este valor é o serviço ligado de armazenamento que criou anteriormente.

    b. Para **caminho do ficheiro**, selecione **procurar no armazenamento** e navegue para a localização onde o script do Hive de exemplo está disponível. Se tiver executado o script do PowerShell anteriormente, nesta localização deve ser `adfgetstarted/hivescripts/hivescript.hql`.

    ![Fornecer detalhes de script de ramo de registo para o pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "Hive fornecer detalhes de script para o pipeline")

    c. Sob **avançadas** > **parâmetros**, selecione **preencher automaticamente a partir do script**. Esta opção procura todos os parâmetros do script de ramo de registo que requerem valores em tempo de execução. O script que utilizar (**hivescript. hql**) tem um **saída** parâmetro. Forneça o valor no formato `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` para apontar para uma pasta existente no seu armazenamento do Azure. O caminho é sensível a maiúsculas e minúsculas. Este é o caminho onde a saída do script será armazenada.

    ![Fornecer os parâmetros para o script do Hive](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "fornecer os parâmetros para o script do Hive")

1. Selecione **Validate** para validar o pipeline. Selecione o botão **>>** (seta para a direita) para fechar a janela de validação.

    ![Validar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "validar o pipeline da fábrica de dados do Azure")

1. Por fim, selecione **publicar tudo** para publicar os artefactos no Azure Data Factory.

    ![Publicar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "publicar o pipeline da fábrica de dados do Azure")

## <a name="trigger-a-pipeline"></a>Acionar um pipeline

1. Na barra de ferramentas na superfície de design, selecione **acionador** > **acionar agora**.

    ![Acionar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "acionar o pipeline da fábrica de dados do Azure")

1. Selecione **concluir** na barra do lado de pop-up.

## <a name="monitor-a-pipeline"></a>Monitorizar um pipeline

1. Mude para o separador **Monitorizar**, no lado esquerdo. Verá uma execução de pipeline na lista **Execuções de Pipeline**. Tenha em atenção o estado da execução sob o **estado** coluna.

    ![Monitorizar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "monitorizar o pipeline da fábrica de dados do Azure")

1. Selecione **Atualizar** para atualizar o estado.

1. Também pode selecionar o **ver execuções de atividades** ícone para ver a execução de atividade associadas do pipeline. A captura de ecrã abaixo, verá apenas uma atividade executar uma vez que existe apenas uma atividade no pipeline que criou. Para regressar à vista anterior, selecione **Pipelines** na parte superior da página.

    ![Monitorizar a atividade do pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "monitorizar a atividade do pipeline de fábrica de dados do Azure")


## <a name="verify-the-output"></a>Verificar a saída

1. Para verificar a saída, no portal do Azure navegue para a conta de armazenamento que utilizou para este tutorial. Deverá ver as pastas ou os contentores seguintes:

    - Verá uma **adfgerstarted/outputfolder** que contém a saída do script do Hive que foi executado como parte do pipeline.

    - Verá uma **adfhdidatafactory -\<ligado-service-name >-\<timestamp >** contentor. Este contentor é a localização de armazenamento predefinida do cluster HDInsight que foi criada como parte da execução de pipeline.

    - Verá uma **adfjobs** registos de contentor que tem a tarefa do Azure Data Factory.  

        ![Verificar a saída do pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "verificar a saída do pipeline de fábrica de dados do Azure")


## <a name="clean-up-the-tutorial"></a>Limpar o tutorial

Com no-deman criação de clusters do HDInsight, não é necessário eliminar explicitamente o cluster do HDInsight. O cluster é eliminado com base na configuração que forneceu ao criar o pipeline. No entanto, mesmo depois do cluster é eliminado, as contas de armazenamento associadas ao cluster continuam a existir. Este comportamento é predefinido para que possa manter seus dados intactos. No entanto, se não desejar persistir os dados, pode eliminar a conta de armazenamento que criou.

Em alternativa, pode eliminar o grupo de recursos inteiro que criou para este tutorial. Esta ação elimina a conta de armazenamento e a fábrica de dados do Azure que criou.

### <a name="delete-the-resource-group"></a>Eliminar o grupo de recursos

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Selecione **grupos de recursos** no painel da esquerda.
1. Selecione o nome do grupo de recursos que criou no script do PowerShell. Se tiver muitos grupos de recursos listados, utilize o filtro. Abre o grupo de recursos.
1. Sobre o **recursos** mosaico, deverá ter a conta de armazenamento predefinida e a fábrica de dados listado, a menos que partilha o grupo de recursos com outros projetos.
1. Selecione **Eliminar grupo de recursos**. Se o fizer, elimina a conta de armazenamento e os dados armazenados na conta de armazenamento.

    ![Eliminar grupo de recursos](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "eliminar grupo de recursos")

1. Introduza o nome do grupo de recursos para confirmar a eliminação e, em seguida, selecione **eliminar**.


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar o Azure Data Factory para criar o cluster de HDInsight a pedido e executar tarefas do Hive. Avance para o artciel seguinte para saber como criar clusters do HDInsight com configuração personalizada.

> [!div class="nextstepaction"]
>[Criar clusters do HDInsight do Azure com configuração personalizada](hdinsight-hadoop-provision-linux-clusters.md)


