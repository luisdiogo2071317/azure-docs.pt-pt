---
title: Personalizar Clusters do HDInsight com ações de script - Azure
description: Saiba como personalizar clusters do HDInsight com ação de Script.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/05/2016
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 6c4652e65f0f320063d989f97a5428510913005b
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105929"
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Personalizar clusters do HDInsight baseado em Windows com ação de Script
**Ação de script** pode ser usado para invocar [scripts personalizados](hdinsight-hadoop-script-actions.md) durante o processo de criação de cluster para a instalação de software adicional num cluster.

As informações neste artigo são específicas para clusters do HDInsight baseado em Windows. Para clusters baseados em Linux, consulte [HDInsight baseado em Linux personalizar clusters com ação de Script](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

Clusters do HDInsight podem ser personalizados numa variedade de outras formas também, como contas de armazenamento do Azure adicionais, incluindo alterar o Hadoop arquivos de configuração (core-site, site do hive, etc.) ou adicionar partilhado bibliotecas (por exemplo, Hive, Oozie) em locais comuns no cluster. Estas personalizações podem ser feitas através do Azure PowerShell, o SDK de .NET do Azure HDInsight ou o portal do Azure. Para obter mais informações, consulte [criar clusters Hadoop no HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Ação de script do processo de criação do cluster
Ação de script só é utilizada enquanto um cluster está no processo a ser criada. O diagrama seguinte ilustra quando a ação de Script é executada durante o processo de criação:

![Personalização de cluster do HDInsight e fases durante a criação do cluster][img-hdi-cluster-states]

Quando o script é executado, o cluster entra o **ClusterCustomization** estágio. Nesta fase, o script for executado sob a conta de administrador de sistema, em paralelo em todos os nós do cluster especificados e fornece privilégios totais de administrador em nós.

> [!NOTE]
> Uma vez que tem privilégios de administrador em nós do cluster durante a **ClusterCustomization** fase, pode utilizar o script para realizar operações como parar e iniciar serviços, incluindo serviços relacionados com o Hadoop. Por isso, como parte do script, tem de se certificar de que os serviços do Ambari e outros serviços relacionados com o Hadoop estão em execução antes do script termina a respetiva execução. Estes serviços são necessários para determinar com êxito o estado de funcionamento e o estado do cluster enquanto ele está a ser criado. Se alterar qualquer configuração no cluster que afeta estes serviços, tem de utilizar as funções de programa auxiliar que são fornecidas. Para obter mais informações sobre funções auxiliares, consulte [scripts de desenvolver ações de Script para HDInsight][hdinsight-write-script].
>
>

A saída e os registos de erros de script são armazenados na conta de armazenamento predefinida especificada para o cluster. Os registos são armazenados numa tabela com o nome **u < \cluster-name-fragment >< \time-stamp > setuplog**. Tratam-se de agregar registos do script ser executado em todos os nós (nó principal e nós de trabalho) no cluster.

Cada cluster pode aceitar várias ações de script que são invocadas na ordem em que são especificados. Um script pode ser executado no nó principal, os nós de trabalho ou ambos.

HDInsight fornece vários scripts para instalar os seguintes componentes em clusters do HDInsight:

| Nome | Script |
| --- | --- |
| **Instalar o Spark** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`. Ver [instalar e utilizar clusters do Spark no HDInsight][hdinsight-install-spark]. |
| **Instalar o R** | `https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1`. Ver [instalar e utilizar R nos clusters do HDInsight](r-server/r-server-hdinsight-manage.md#install-additional-r-packages-on-the-cluster). |
| **Instalar o Solr** | `https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1`. Ver [instalar e utilizar Solr nos HDInsight clusters](hdinsight-hadoop-solr-install.md). |
| **Instalar o Giraph** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`. Ver [instalar e utilizar Giraph no HDInsight clusters](hdinsight-hadoop-giraph-install.md). |
| **Pré-carregar bibliotecas do Hive** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`. Consulte [bibliotecas adicionar Hive em clusters do HDInsight](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Scripts de chamada com o portal do Azure
**No portal do Azure**

1. Começar a criar um cluster, conforme descrito em [criar clusters Hadoop no HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. Em configuração opcional, para o **ações de Script** painel, clique em **adicione a ação de script** para fornecer detalhes sobre a ação de script, conforme mostrado abaixo:

    ![Utilize a ação de Script para personalizar um cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "utiliza a ação de Script para personalizar um cluster")

    <table border='1'>
        <tr><th>Propriedade</th><th>Valor</th></tr>
        <tr><td>Nome</td>
            <td>Especifique um nome para a ação de script.</td></tr>
        <tr><td>URI do script</td>
            <td>Especifique o URI para o script que é invocado para personalizar o cluster. s</td></tr>
        <tr><td>HEAD/de trabalho</td>
            <td>Especifique os nós (**Head** ou **Worker**) no qual o script de personalização é executado.</b>.
        <tr><td>Parâmetros</td>
            <td>Especifique os parâmetros, se necessário, o script.</td></tr>
    </table>

    Prima ENTER para adicionar mais do que uma ação de script para instalar vários componentes no cluster.
3. Clique em **selecione** para guardar a configuração de ação de script e continuar com a criação do cluster.

## <a name="call-scripts-using-azure-powershell"></a>Scripts de chamada com o Azure PowerShell
Este script do PowerShell seguinte demonstra como instalar o Spark num cluster de HDInsight com base do Windows.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Connect-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Para instalar outro software, terá de substituir o ficheiro de script no script:

Quando lhe for pedido, introduza as credenciais para o cluster. Pode demorar vários minutos antes do cluster ser criado.

## <a name="call-scripts-using-net-sdk"></a>Scripts de chamada com o .NET SDK
O exemplo a seguir demonstra como instalar o Spark num cluster de HDInsight com base do Windows. Para instalar outro software, terá de substituir o ficheiro de script no código.

**Para criar um cluster do HDInsight com o Spark**

1. Crie uma aplicação de consola c# no Visual Studio.
2. A partir da consola do Gestor de pacotes Nuget, execute o seguinte comando.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. Utilize as seguintes instruções "using" no ficheiro Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Coloque o código na classe com o seguinte:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. Prima **F5** para executar a aplicação.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Suporte para o software de código aberto utilizada nos clusters do HDInsight
O serviço do Microsoft Azure HDInsight é uma plataforma flexível que lhe permite criar aplicações de macrodados na cloud através de um ecossistema de tecnologias de open source formado em torno do Hadoop. O Microsoft Azure fornece um nível geral de suporte para tecnologias de código-fonte aberto, como discutido no **o âmbito de suporte** secção a <a href="http://azure.microsoft.com/support/faq/" target="_blank">site FAQ de suporte do Azure</a>. O serviço HDInsight fornece um nível adicional de suporte para alguns dos componentes, conforme descrito abaixo.

Existem dois tipos de componentes de código-fonte aberto que estão disponíveis no serviço HDInsight:

* **Componentes internos** -esses componentes estão pré-instaladas em clusters do HDInsight e fornecer a funcionalidade principal do cluster. Por exemplo, ResourceManager do YARN, a linguagem de consulta do Hive (HiveQL) e a biblioteca Mahout pertencem a essa categoria. Uma lista completa dos componentes de cluster está disponível no [quais são as novidades nas versões de cluster do Hadoop fornecidas pelo HDInsight?](hdinsight-component-versioning.md) </a>.
* **Componentes personalizados** -, como um utilizador do cluster, pode instalar ou utilizar na sua carga de trabalho qualquer componente disponível na Comunidade ou criado por si.

Componentes internos são totalmente suportados e Support da Microsoft irá ajudar a isolar e resolver problemas relacionados com esses componentes.

> [!WARNING]
> Componentes fornecidos com o cluster do HDInsight são totalmente suportadas e Support da Microsoft irá ajudar a isolar e resolver problemas relacionados com esses componentes.
>
> Componentes personalizados recebem suporte comercialmente razoável para ajudá-lo a resolver o problema. Isso pode resultar em resolver o problema ou pedir-lhe para interagir com os canais disponíveis para as tecnologias de código-fonte aberto, onde os conhecimentos aprofundados para essa tecnologia é encontrado. Por exemplo, há muitos sites de Comunidade que podem ser utilizados, como: [fórum do MSDN para o HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Também projetos Apache tem sites de projeto no [ http://apache.org ](http://apache.org), por exemplo: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

O serviço HDInsight fornece várias formas de utilizar componentes personalizados. Independentemente de como um componente utilizado ou instalado no cluster, aplica-se o mesmo nível de suporte. Segue-se uma lista das formas mais comuns que os componentes personalizados podem ser utilizados em clusters do HDInsight:

1. Submissão de tarefas - Hadoop ou outros tipos de tarefas que executarem ou usam componentes personalizados pode ser submetida para o cluster.
2. Personalização de cluster - durante a criação do cluster, pode especificar definições adicionais e componentes personalizados que serão instalados em nós do cluster.
3. Exemplos - para populares componentes personalizados, Microsoft e outras pessoas podem fornecer exemplos de como esses componentes podem ser usados nos clusters do HDInsight. Estes exemplos são fornecidos sem suporte.

## <a name="develop-script-action-scripts"></a>Desenvolver scripts de ação de Script
Ver [scripts de desenvolver ações de Script para HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Consulte também
* [Criar clusters do Hadoop no HDInsight] [ hdinsight-provision-cluster] fornece instruções sobre como criar um cluster do HDInsight com outras opções personalizadas.
* [Desenvolver scripts de ação de Script para o HDInsight][hdinsight-write-script]
* [Instalar e utilizar o Spark em clusters do HDInsight][hdinsight-install-spark]
* [Instalar e utilizar Solr nos clusters do HDInsight](hdinsight-hadoop-solr-install.md).
* [Instalar e utilizar Giraph nos clusters do HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fases durante a criação do cluster"
