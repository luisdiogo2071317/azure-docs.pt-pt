---
title: 'Tutorial: Criar clusters do Hadoop a pedido no Azure HDInsight utilizando o Data Factory | Microsoft Docs'
description: Saiba como criar clusters do Hadoop a pedido no HDInsight com o Azure Data Factory.
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
ms.openlocfilehash: 9d54d3481176b36a0d13a9b8af2fad03349b81be
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229258"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Tutorial: Criar clusters do Hadoop a pedido no HDInsight com o Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Neste artigo, irá aprender a criar um cluster do Hadoop, a pedido, no Azure HDInsight com o Azure Data Factory. Utilize, em seguida, pipelines de dados no Azure Data Factory para executar tarefas do Hive e eliminar o cluster. No final deste tutorial, irá aprender a operacionalizar uma em que a criação do cluster, ser executada a tarefa e eliminação de cluster são executadas com base numa agenda de execução da tarefa de macrodados.

Este tutorial abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Criar uma conta do Storage do Azure
> * Compreender a atividade do Azure Data Factory
> * Criar uma fábrica de dados através do portal do Azure
> * Criar serviços ligados
> * Criar um pipeline
> * Acionar um pipeline
> * Monitorizar um pipeline
> * Verificar a saída

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Azure PowerShell. Para obter instruções, consulte [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Um principal de serviço do Azure Active Directory. Assim que tiver criado o principal de serviço, não se esqueça de obter o **ID da aplicação** e **chave de autenticação** utilizando as instruções no artigo ligado. Necessitar destes valores mais tarde no tutorial. Além disso, certifique-se o principal de serviço é um membro do *contribuinte* função de subscrição ou o grupo de recursos no qual o cluster for criado. Para obter instruções obter os valores necessários e atribua as funções corretas, consulte [criar um Azure Active Directory principal de serviço](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

Nesta secção, crie uma conta de armazenamento que será utilizada como armazenamento de predefinido para o cluster do HDInsight que cria a pedido. Esta conta de armazenamento também contém o script de HiveQL de exemplo (**hivescript.hql**) que utiliza para simular uma tarefa do Hive de exemplo que é executado no cluster.

Esta secção utiliza um script do PowerShell do Azure para criar a conta de armazenamento e copiar os ficheiros necessários dentro da conta de armazenamento. O script de exemplo do PowerShell do Azure nesta secção efetua as seguintes tarefas:

1. Para os registos no Azure.
2. Cria um grupo de recursos do Azure.
3. Cria uma Conta de armazenamento do Azure.
4. Cria um contentor de BLOBs na conta de armazenamento
5. Copia o script de HiveQL de exemplo (**hivescript.hql**) o contentor de Blob. O script está disponível em [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). O script de exemplo já se encontra disponível no outro contentor de Blob público. O script do PowerShell abaixo cria uma cópia destes ficheiros para a conta de armazenamento do Azure cria.


**Para criar uma conta de armazenamento e copiar os ficheiros com o Azure PowerShell:**
> [!IMPORTANT]
> Especifique nomes para o grupo de recursos do Azure e a conta de armazenamento do Azure que será criada pelo script.
> Anote **nome do grupo de recursos**, **nome da conta de armazenamento**, e **chave da conta de armazenamento** debitado pelo script. Tem a secção seguinte.

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
2. Selecione **grupos de recursos** no painel esquerdo.
3. Faça duplo clique o nome do grupo de recursos que criou no seu script do PowerShell. Utilize o filtro se tem demasiados grupos de recursos listados.
4. No **recursos** mosaico, verá um recurso listado, a menos que partilhar o grupo de recursos com outros projetos. Esse recurso é a conta de armazenamento com o nome que especificou anteriormente. Selecione o nome de conta de armazenamento.
5. Selecione o **Blobs** mosaicos.
6. Selecione o **adfgetstarted** contentor. Pode ver uma pasta denominada **hivescripts**.
7. Abra a pasta e certifique-se de que contém o ficheiro de script de exemplo, **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Compreender a atividade do Azure Data Factory

[O Azure Data Factory](../data-factory/introduction.md) orquestra e automatiza o movimento e a transformação de dados. O Azure Data Factory pode criar um Hadoop do HDInsight cluster just-in-time para processar um setor de dados de entrada e eliminar o cluster quando o processamento é concluído. 

No Azure Data Factory, uma fábrica de dados pode ter um ou mais pipelines de dados. Um pipeline de dados tem uma ou mais atividades. Existem dois tipos de atividades:

* [Atividades de movimentos de dados](../data-factory/copy-activity-overview.md) -utilizar atividades de movimentos de dados para mover dados de um arquivo de dados de origem para um arquivo de dados de destino.
* [Atividades de transformação de dados](../data-factory/transform-data.md). Utilize atividades de transformação de dados para a transformação/processar dados. Atividade do ramo de registo do HDInsight é uma das atividades de transformação suportadas pela fábrica de dados. Utilize a atividade de transformação do ramo de registo neste tutorial.

Neste artigo, configurar a atividade do ramo de registo para criar um cluster de Hadoop do HDInsight a pedido. Quando a atividade é executada para processar dados, eis o que acontece:

1. Um cluster de Hadoop do HDInsight é criado automaticamente para just-in-time processar o setor. 

2. Os dados de entrada são processados, executando um script de HiveQL no cluster. Neste tutorial, o script de HiveQL associado com a atividade do ramo de registo efetua as seguintes ações:

    * Utilize a tabela existente (*hivesampletable*) para criar outra tabela **HiveSampleOut**.
    * Povoa o **HiveSampleOut** tabela com apenas colunas específicas do original *hivesampletable*.

3. O cluster de Hadoop do HDInsight é eliminado após o processamento é concluído e o cluster estiver inativo durante o período de tempo (definição de timeToLive) configurado. Se o setor de dados seguinte está disponível para o processamento no tempo de inatividade deste timeToLive, mesmo cluster é utilizado para processar o setor.  

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).

2. No portal do Azure, selecione **crie um recurso** > **dados + análise** > **Data Factory**.

    ![O Azure Data Factory no portal do](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "do Azure Data Factory no portal")

2. Introduza ou selecione os valores, conforme mostrado na captura de ecrã seguinte:

    ![Criar fábrica de dados do Azure através do portal do Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "criar do Azure Data Factory com o portal do Azure")

    Introduza ou selecione os seguintes valores:
    
    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome** |  Introduza um nome para a fábrica de dados. Este nome tem de ser globalmente exclusivo.|
    |**Subscrição**     |  Selecione a sua subscrição do Azure. |
    |**Grupo de recursos**     | Selecione **utilizar existente** e, em seguida, selecione o grupo de recursos que criou utilizando o script do PowerShell. |
    |**Versão**     | Selecione **V2 (pré-visualização)** |
    |**Localização**     | A localização é automaticamente definida para a localização que especificou ao criar o grupo de recursos anteriores. Para este tutorial, a localização é definida para **EUA Leste 2**. |
    

3. Selecione **afixar ao dashboard**e, em seguida, selecione **criar**. Irá ver um novo mosaico intitulado **A submeter a implementação** no dashboard do portal. Criar uma fábrica de dados pode demorar entre 2 a 4 minutos.

    ![Progresso da implementação de modelo](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "progresso da implementação de modelo") 
 
4. Assim que for criada a fábrica de dados, o portal mostra a descrição geral do data Factory.

    ![Descrição geral do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "descrição geral do Azure Data Factory")

5. Selecione **autor & Monitor** para iniciar o Azure Data Factory criação e monitorização do portal.

## <a name="create-linked-services"></a>Criar serviços ligados

Nesta secção, criar dois serviços ligados numa fábrica de dados.

- Um **serviço ligado do Armazenamento do Azure** que liga uma conta de armazenamento do Azure à fábrica de dados. Este armazenamento é utilizado pelo cluster do HDInsight a pedido. Também contém o script de ramo de registo que é executado no cluster.
- Um **serviço ligado do HDInsight a pedido**. O Azure Data Factory cria um cluster do HDInsight e executa o script de ramo de registo automaticamente. Em seguida, elimina o cluster do HDInsight depois de o cluster estar inativo durante um período de tempo pré-configurado.

###  <a name="create-an-azure-storage-linked-service"></a>Criar um serviço ligado do Armazenamento do Azure

1. No painel esquerdo do **comecemos** página, selecione o **editar** ícone.

    ![Criar um serviço ligado Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "criar um serviço ligado Azure Data Factory")

2. Selecione **ligações** no canto inferior esquerdo da janela e, em seguida, selecione **+ novo**.

    ![Criar ligações no Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "criar ligações no Azure Data Factory")

3. No **novo serviço ligado** caixa de diálogo, selecione **Blob Storage do Azure** e, em seguida, selecione **continuar**.

    ![Serviço do Data Factory ligado do Storage do Azure de criar](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "serviço Data Factory ligado do Storage do Azure de criar")

4. Forneça um nome para o serviço ligado de armazenamento, selecione a conta de armazenamento do Azure que criou como parte do script do PowerShell e, em seguida, selecione **concluir**.

    ![Serviço ligado de fornecer o nome para o Storage do Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "serviço ligado de fornecer o nome para o Storage do Azure")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Crie um serviço ligado do HDInsight a pedido

1. Selecione o botão **+ Novo** novamente para criar outro serviço ligado.

2. Na janela **Novo Serviço Ligado**, selecione **Computação** > **Azure HDInsight** e, em seguida, **Continuar**.

    ![Criar HDInsight ligado serviço para o Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "ligado de HDInsight criar serviço para o Azure Data Factory")

3. No **novo serviço ligado** janela, forneça os valores necessários.

    ![Forneça valores para o HDInsight serviço ligado](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "forneça valores para o HDInsight serviço ligado")

    Introduza os seguintes valores e deixe o resto como predefinição.

    | Propriedade | Descrição |
    | --- | --- |
    | Nome | Introduza um nome para o serviço ligado do HDInsight |
    | Tipo | Selecione **HDInsight a pedido** |
    | Serviço Ligado do Storage do Azure | Selecione o serviço ligado de armazenamento que criou anteriormente. |
    | Tipo de cluster | Selecione **hadoop** |
    | TTL | Forneça a duração para o qual pretende que o cluster do HDInsight fique disponível antes de ser eliminado automaticamente.|
    | ID do principal de serviço | Forneça o ID da aplicação do principal de serviço do Azure Active Directory que criou como parte dos pré-requisitos |
    | Chave principal do serviço | Forneça a chave de autenticação para o principal de serviço do Azure Active Directory |
    | Prefixo de nome de cluster | Forneça um valor que será adicionado como prefixo a todos os tipos de cluster que são criados pela fábrica de dados |
    | Grupo de recursos | Selecione o grupo de recursos que criou como parte do script do PowerShell que utilizou anteriormente| 
    | Nome de utilizador SSH do cluster | Introduza um nome de utilizador do SSH |
    | Palavra-passe SSH do cluster | Forneça uma palavra-passe para o utilizador SSH |

    Selecione **Concluir**.

## <a name="create-a-pipeline"></a>Criar um pipeline

1. Selecione o botão **+** (mais) e, em seguida, selecione **Pipeline**.

    ![Criar um pipeline no Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "criar um pipeline no Azure Data Factory")

2. No **atividades** caixa de ferramentas, expanda **HDInsight**e arraste o **Hive** atividade para a superfície do designer de pipeline. No **geral** separador, forneça um nome para a atividade.

    ![Adicionar atividades ao pipeline do Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "adicionar atividades ao pipeline do Data Factory")

3. Certifique-se de que a atividade do ramo de registo selecionada, selecione o **HDI Cluster** separador e para o **serviço ligado de HDInsight** pendente, selecione o serviço ligado que criou anteriormente para o HDInsight.

    ![Forneça os detalhes do cluster de HDInsight para o pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "HDInsight fornecer os detalhes do cluster para o pipeline")

4. Selecione o **Script** separador e conclua os seguintes passos:

    a. Para **serviço ligado do Script**, selecione **HDIStorageLinkedService**. Este valor é o serviço ligado de armazenamento que criou anteriormente.

    b. Para **caminho do ficheiro**, selecione **procurar armazenamento** e navegue para a localização onde o script de ramo de registo de exemplo está disponível. Se tiver executado o script do PowerShell anteriormente, esta localização deve ser `adfgetstarted/hivescripts/hivescript.hql`.

    ![Forneça detalhes de script de ramo de registo para o pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "ramo de registo fornecem detalhes de script para o pipeline")

    c. Em **avançadas** > **parâmetros**, selecione **Auto-preenchimento do script**. Esta opção procura quaisquer parâmetros no script de ramo de registo que necessitam de valores no tempo de execução. O script que utilizar (**hivescript.hql**) tem um **saída** parâmetro. Forneça o valor no formato `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` para apontar para uma pasta existente no seu armazenamento do Azure. O caminho é sensível a maiúsculas e minúsculas. Este é o caminho onde será armazenada a saída do script.

    ![Fornecer parâmetros para o script de ramo de registo](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "fornecer parâmetros para o script de ramo de registo")

5. Selecione **validar** para validar o pipeline. Selecione o botão **>>** (seta para a direita) para fechar a janela de validação.

    ![Validar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "validar o pipeline do Azure Data Factory")

5. Por fim, selecione **publicar todas as** para publicar os artefactos ao Azure Data Factory.

    ![Publicar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "publicar o pipeline do Azure Data Factory")

## <a name="trigger-a-pipeline"></a>Acionar um pipeline

1. Na barra de ferramentas na superfície do designer, selecione **acionador** > **acionador agora**.

    ![Acionar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "acionar o pipeline do Azure Data Factory")

2. Selecione **concluir** na barra lateral pop-up.

## <a name="monitor-a-pipeline"></a>Monitorizar um pipeline

1. Mude para o separador **Monitorizar**, no lado esquerdo. Verá uma execução de pipeline na lista **Execuções de Pipeline**. Tenha em atenção o estado de execução sob o **estado** coluna.

    ![Monitorizar o pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "monitorizar o pipeline do Azure Data Factory")

2. Selecione **Atualizar** para atualizar o estado.

3. Também pode selecionar o **execuções de atividade de vista** ícone para ver a execução da atividade associada com o pipeline. Na captura de ecrã abaixo, verá apenas uma atividade executar uma vez que existe apenas uma atividade no pipeline que criou. Para mudar para a vista anterior, selecione **Pipelines** direção ao topo da página.

    ![Monitorizar a atividade de pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "monitorizar a atividade de pipeline do Azure Data Factory")


## <a name="verify-the-output"></a>Verificar a saída

1. Para verificar o resultado, no portal do Azure navegue para a conta de armazenamento que utilizou para este tutorial. Deverá ver as seguintes pastas ou contentores:

    - Verá uma **adfgerstarted/outputfolder** que contém o resultado do script de ramo de registo que foi executado como parte do pipeline.

    - Verá uma **adfhdidatafactory -\<ligado-service-name >-\<timestamp >** contentor. Este contentor é a localização de armazenamento predefinida do cluster do HDInsight que foi criada como parte da execução de pipeline.

    - Verá uma **adfjobs** registos do contentor que contém a tarefa do Azure Data Factory.  

        ![Certifique-se a saída do pipeline do Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Certifique-se a saída do pipeline do Azure Data Factory")


## <a name="clean-up-the-tutorial"></a>Limpar o tutorial

Com no-deman criação de clusters do HDInsight, não terá de eliminar explicitamente o cluster do HDInsight. Eliminar o cluster com base na configuração de que forneceu ao criar o pipeline. No entanto, mesmo depois de eliminar o cluster, as contas de armazenamento associadas o cluster continuam existir. Este comportamento é por predefinição, para que pode manter os dados intactos. No entanto, se não pretender manter os dados, pode eliminar a conta de armazenamento que criou.

Em alternativa, pode eliminar o grupo de recursos completo que criou para este tutorial. Isto elimina a conta de armazenamento e o Azure Data Factory que criou.

### <a name="delete-the-resource-group"></a>Eliminar o grupo de recursos

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **grupos de recursos** no painel esquerdo.
3. Selecione o nome do grupo de recursos que criou no seu script do PowerShell. Utilize o filtro se tem demasiados grupos de recursos listados. É aberto o grupo de recursos.
4. No **recursos** mosaico, deverá ter a conta do storage predefinida e a fábrica de dados listados, a menos que partilhar o grupo de recursos com outros projetos.
5. Selecione **Eliminar grupo de recursos**. Se o fizer, elimina a conta de armazenamento e os dados armazenados na conta de armazenamento.

    ![Eliminar grupo de recursos](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "eliminar grupo de recursos")

6. Introduza o nome do grupo de recursos para confirmar a eliminação e, em seguida, selecione **eliminar**.


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar o Azure Data Factory para criar cluster do HDInsight a pedido e executar tarefas do Hive. Avançar para o artciel seguinte para saber como criar clusters do HDInsight com configuração personalizada.

> [!div class="nextstepaction"]
>[Criar clusters do HDInsight do Azure com a configuração personalizada](hdinsight-hadoop-provision-linux-clusters.md)


