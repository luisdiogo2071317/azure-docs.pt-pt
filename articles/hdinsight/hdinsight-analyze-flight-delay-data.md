---
title: Analisar dados de atraso de voo com o Apache Hadoop no HDInsight - Azure
description: Saiba como utilizar um script do Windows PowerShell para criar um cluster do HDInsight, execute uma tarefa do Hive, executar uma tarefa de Sqoop e eliminar o cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: e95440f72580b928cd41b6d03f30459cfb70a510
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965397"
---
# <a name="analyze-flight-delay-data-by-using-apache-hive-in-hdinsight"></a>Analisar dados de atraso de voo com o Apache Hive no HDInsight
[Apache Hive](https://hive.apache.org/) fornece um meio da execução [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) tarefas por meio de uma linguagem de script semelhante a SQL denominada *[HiveQL] [ hadoop-hiveql]*, que podem ser aplicado em resumir, consultar e analisar grandes volumes de dados.

> [!IMPORTANT]  
> Os passos neste documento exigem um cluster do HDInsight baseado em Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows). Para obter os passos que funcionam com um cluster baseado em Linux, veja [analisar dados de atraso de voo com o Apache Hive no HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Uma das principais vantagens do Azure HDInsight é a separação de dados de armazenamento e computação. HDInsight utiliza o armazenamento de Blobs do Azure para armazenamento de dados. Uma típica tarefa envolve três partes:

1. **Store dados no armazenamento de Blobs do Azure.**  Por exemplo, sobreviver aos dados, dados de sensores, registos web e, neste caso, os dados de atraso de voo são guardados no armazenamento de Blobs do Azure.
2. **Execute tarefas.** Quando chegou a hora para processar os dados, executar um script do Windows PowerShell (ou uma aplicação de cliente) para criar um cluster do HDInsight, executar tarefas e eliminar o cluster. As tarefas de guardam os dados de saída para o armazenamento de Blobs do Azure. Os dados de saída são mantidos, mesmo depois do cluster é eliminado. Dessa forma, paga apenas o que tiver consumido.
3. **Obter a saída do armazenamento de Blobs do Azure**, ou neste tutorial, exportar os dados para uma base de dados SQL do Azure.

O diagrama seguinte ilustra o cenário e a estrutura deste Tutorial:

![HDI.FlightDelays.flow][img-hdi-flightdelays-flow]

Tenha em atenção que os números no diagrama correspondem aos títulos de secção. **M** significa para o processo principal. **A** significa o conteúdo no anexo.

A principal parte do tutorial mostra-lhe como utilizar um script do Windows PowerShell para realizar as seguintes tarefas:

* Crie um cluster do HDInsight.
* Execute uma tarefa do Hive no cluster para calcular a média atrasos em aeroportos. Os dados de atraso de voo são armazenados numa conta de armazenamento de Blobs do Azure.
* Execute uma tarefa de Sqoop para exportar o resultado da tarefa do Hive para uma base de dados SQL do Azure.
* Elimine o cluster do HDInsight.

No appendixes, pode encontrar as instruções para carregar dados de atraso de voo, criar/carregar uma cadeia de caracteres de consulta do Hive e preparar a base de dados SQL do Azure para a tarefa de Sqoop.

> [!NOTE]  
> Os passos neste documento são específicos para clusters do HDInsight baseado em Windows. Para obter os passos que funcionam com um cluster baseado em Linux, veja [analisar dados de atraso de voo com o Apache Hive no HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

### <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de ter os seguintes itens:

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Uma estação de trabalho com o Azure PowerShell**.

    > [!IMPORTANT]  
    > O suporte do Azure PowerShell para gerir recursos do HDInsight com o Gestor de Serviços do Azure está **preterido**, e foi removido a 1 de janeiro de 2017. Os passos neste documento utilizam os novos cmdlets do HDInsight que funcionam com o Azure Resource Manager.
    >
    > Siga os passos em [Instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para instalar a versão mais recente do Azure PowerShell. Se tiver scripts que tenham de ser modificados para utilizar os novos cmdlets que funcionam com o Azure Resource Manager, veja [Migrar para as ferramentas de desenvolvimento baseadas no Azure Resource Manager para clusters do HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) para obter mais informações.

**Ficheiros utilizados neste tutorial**

Este tutorial utiliza o desempenho de chegadas de dados de voo companhia aérea da [Research e inovadora de tecnologia de administração, estatísticas de Bureau de transporte ou RITA][rita-website].
Uma cópia dos dados tiver sido carregada para um contentor de armazenamento de Blobs do Azure com a permissão de acesso do Blob público.
Uma parte do seu script de PowerShell copia os dados do contentor de BLOBs público para o contentor de BLOBs predefinido do cluster. O script de HiveQL é também copiado para o mesmo contentor de Blobs.
Se quiser aprender como get/carregar os dados para a sua própria conta de armazenamento e como criar/carregar o ficheiro de script de HiveQL, consulte [apêndice A](#appendix-a) e [apêndice B](#appendix-b).

A tabela seguinte lista os ficheiros utilizados neste tutorial:

|Ficheiros|Descrição|  
|----|----|   
|wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql|O ficheiro de script de HiveQL utilizado pela tarefa do Hive. Este script tiver sido carregado para uma conta de armazenamento de Blobs do Azure com o acesso público. <a href="#appendix-b">Apêndice B</a> tem instruções sobre como preparar e carregar este ficheiro para a sua própria conta de armazenamento de Blobs do Azure.|
|wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data|Dados de entrada para a tarefa do Hive. Os dados tem sido carregados para uma conta de armazenamento de Blobs do Azure com o acesso público. <a href="#appendix-a">Apêndice A</a> tem instruções sobre obtendo os dados e carregar os dados para a sua própria conta de armazenamento de Blobs do Azure.|
|\tutorials\flightdelays\output|O caminho de saída para a tarefa do Hive. O contentor predefinido é utilizado para armazenar os dados de saída.|
|\tutorials\flightdelays\jobstatus|A pasta estado do trabalho do Hive que se no contentor predefinido.|


## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Criar cluster e executar tarefas do Hive/Sqoop
MapReduce do Hadoop é o processamento em lotes. A maneira mais econômica para executar uma tarefa do Hive é criar um cluster para a tarefa e eliminar a tarefa depois da tarefa estiver concluída. O script seguinte abrange todo o processo.
Para obter mais informações sobre como criar um cluster do HDInsight e executar tarefas do Hive, consulte [Apache Hadoop criar clusters no HDInsight] [ hdinsight-provision] e [utilizar o Apache Hive com HDInsight] [hdinsight-use-hive].

**Para executar as consultas do Hive ao Azure PowerShell**

1. Utilizar as instruções em criar uma base de dados SQL do Azure e a tabela do resultado da tarefa de Sqoop [apêndice C](#appendix-c).
2. Abra o ISE do Windows PowerShell e execute o seguinte script:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure the following variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different

    ###########################################
    # (Optional) configure the following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter the Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use the cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Connect-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare the HiveQL script and source data
    ###########################################

    # Create the temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data to default container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit the Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit the Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete the cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. Ligar à base de dados SQL e veja os atrasos de voos média por cidade na tabela AvgDelays:

    ![HDI.FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Apêndice A – dados de atraso de voos de carregamento para o armazenamento de Blobs do Azure
Carregar o ficheiro de dados e o [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) arquivos de script (consulte [apêndice B](#appendix-b)) exige algum planejamento. A idéia é armazenar os ficheiros de dados e o ficheiro de HiveQL antes de criar um cluster do HDInsight e executar a tarefa do Hive. Tem duas opções:

* **Utilize a mesma conta de armazenamento do Azure que será utilizada pelo cluster do HDInsight, como o sistema de ficheiros predefinido.** Uma vez que o cluster de HDInsight terá a chave de acesso da conta de armazenamento, não precisa de fazer alterações adicionais.
* **Utilize uma conta de armazenamento do Azure diferente do sistema de ficheiros predefinido do cluster de HDInsight.** Se for este o caso, tem de modificar a parte da criação do Windows PowerShell script encontrado no cluster do HDInsight de criar e executar tarefas do Apache Hive/Sqoop para ligar a conta de armazenamento como uma conta de armazenamento adicional. Para obter instruções, consulte [Apache Hadoop criar clusters no HDInsight][hdinsight-provision]. O cluster do HDInsight, em seguida, sabe que a chave de acesso para a conta de armazenamento.

> [!NOTE]  
> O caminho de armazenamento de BLOBs para o ficheiro de dados estiver codificado no ficheiro de script de HiveQL. Tem de atualizá-lo em conformidade.

**Para transferir os dados de voo**

1. Navegue para [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website] (Administração de Investigação e Inovação Tecnológica, Instituto de Estatísticas de Transportes).
2. Na página, selecione os seguintes valores:

    |Nome|Valor|
    |---|---|
    |Filtrar Ano|2013|
    |Filtrar Período|Janeiro|
    |Campos|*Ano*, *FlightDate*, *UniqueCarrier*, *operadora*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (limpar todos os outros campos)|

3. Clique em **Transferir**.
4. Deszipe o ficheiro para o **C:\Tutorials\FlightDelay\2013Data** pasta. Cada arquivo é um ficheiro CSV e cerca de 60GB de tamanho.
5. Renomeie o arquivo com o nome do mês que contém dados para. Por exemplo, o ficheiro que contém os dados de Janeiro teria o nome *January.csv*.
6. Repita os passos 2 e 5 para transferir um ficheiro para cada um dos 12 meses em 2013. Precisará de um mínimo de um ficheiro para executar o tutorial.

**Para carregar os dados de atraso de voo para o armazenamento de Blobs do Azure**

1. Prepare os parâmetros:

    |Nome da variável|Notas|
    |---|---|
    |$storageAccountName|A conta de armazenamento do Azure para carregar os dados para onde pretende.|
    |$blobContainerName|O contentor de BLOBs para carregar os dados para onde pretende.|
    
2. Abra o ISE do PowerShell do Azure.
3. Cole o seguinte script no painel de script:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
    $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
    #EndRegion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy the file from local workstation to Azure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName to $blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
    }

    # List the uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Prima **F5** para executar o script.

Se optar por utilizar um método diferente para carregar os ficheiros, certifique-se de que o caminho do ficheiro é tutoriais/flightdelay/data. A sintaxe para aceder aos ficheiros é:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

O caminho flightdelay/tutoriais/dados é a pasta virtual que criou ao carregar os ficheiros. Certifique-se de que existem 12 ficheiros, um para cada mês.

> [!NOTE]  
> Tem de atualizar a consulta do Hive para ler a partir da nova localização.
>
> Também tem de configurar a permissão de acesso do contentor para ser público ou vincular a conta de armazenamento para o cluster do HDInsight. Caso contrário, a cadeia de consulta do Hive não será capaz de acessar os arquivos de dados.

- - -

## <a id="appendix-b"></a>Apêndice B - criar e carregar um script de HiveQL
Com o Azure PowerShell, pode executar várias [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) instruções um cada vez, ou pacote a instrução de HiveQL para um ficheiro de script. Esta secção mostra-lhe como criar um script de HiveQL e carregar o script para o armazenamento de Blobs do Azure com o Azure PowerShell. Hive requer que os scripts de HiveQL para ser armazenados no armazenamento de Blobs do Azure.

O script de HiveQL irá executar o seguinte:

1. **Remover a tabela de delays_raw**, caso a tabela já existe.
2. **Criar a tabela do Hive externa delays_raw** apontando para a localização de armazenamento de Blob com os ficheiros de atraso de voo. Esta consulta Especifica que campos são delimitados por "," e que as linhas são terminar com "\n". Isso constitui um problema quando os valores de campo conter vírgulas porque o ramo de registo não consegue diferenciar entre uma vírgula, que é um delimitador de campos e uma que faz parte de um valor de campo (que é o caso nos valores de campo para a origem do\_Cidade\_nome e DEST\_ Cidade\_nome). Para resolver isso, a consulta cria colunas TEMP para armazenar dados que incorretamente são divididos em colunas.
3. **Remover a tabela de atrasos**, caso a tabela já existe.
4. **Criar a tabela de atrasos**. É útil limpar os dados antes de processamento adicional. Esta consulta cria uma nova tabela *atrasos*, da tabela delays_raw. Tenha em atenção que as colunas TEMP (conforme mencionado anteriormente) não são copiadas e de que o **subcadeia** função é usada para remover o entre aspas duplas dos dados.
5. **Computação o atraso de meteorológicas médias e grupos os resultados por nome de cidade.** Também exporta os resultados para o armazenamento de Blobs. Tenha em atenção que a consulta irá remover apóstrofos a partir dos dados e excluir linhas em que o valor para **weather_delay** é nulo. Isso é necessário porque o Sqoop, utilizado mais tarde neste tutorial, não lida com esses valores com elegância por predefinição.

Para obter uma lista completa dos comandos HiveQL, consulte [Apache de linguagem de definição de dados do Hive][hadoop-hiveql]. Cada [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) comando tem de terminar com um ponto e vírgula.

**Para criar um ficheiro de script de HiveQL**

1. Prepare os parâmetros:

    |Nome da variável|Notas|
    |---|---|
    |$storageAccountName|A conta de armazenamento do Azure para carregar o script de HiveQL para onde pretende.|
    |$blobContainerName|O contentor de BLOBs para carregar o script de HiveQL para onde pretende.|
    
2. Abra o ISE do PowerShell do Azure.  

3. Copie e cole o seguinte script no painel de script:  

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # The HiveQL script file is exported as this file before it's uploaded to Blob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # The HiveQL script file will be uploaded to Blob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by the HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate the file and file path

    # Check if a file with the same file name already exists on the workstation
    Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwrite it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create the folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write the Hive script into a local file
    Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload the Hive script to the default Blob container
    Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload the file from local workstation to Blob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

    Aqui estão as variáveis no script:

   * **$hqlLocalFileName** -o script guarda o ficheiro de script de HiveQL localmente antes de o carregar para o armazenamento de Blobs. Este é o nome de ficheiro. O valor predefinido é <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -este é o nome de blob do ficheiro do HiveQL script usado no armazenamento de Blobs do Azure. O valor predefinido é tutorials/flightdelay/flightdelays.hql. Uma vez que o arquivo será gravado diretamente para o armazenamento de Blobs do Azure, não existe uma "/" no início do nome do blob. Se quiser acessar o arquivo de armazenamento de BLOBs, terá de adicionar uma "/" no início do nome do ficheiro.
   * **$srcDataFolder** and **$dstDataFolder** - = "tutorials/flightdelay/data" = "tutorials/flightdelay/output"

- - -
## <a id="appendix-c"></a>Apêndice C - preparar uma base de dados SQL do Azure para o resultado da tarefa Sqoop
**Para preparar a base de dados do SQL (mesclará com o Sqoop script)**

1. Prepare os parâmetros:

    |Nome da variável|Notas|
    |---|---|
    |$sqlDatabaseServerName|O nome do servidor de base de dados SQL do Azure. Introduza nada para criar um novo servidor.|
    |$sqlDatabaseUsername|O nome de início de sessão para o servidor de base de dados SQL do Azure. Se $sqlDatabaseServerName for um servidor existente, o início de sessão e palavra-passe de início de sessão são utilizados para autenticar com o servidor. Caso contrário, eles são usados para criar um novo servidor.|
    |$sqlDatabasePassword|A palavra-passe do início de sessão para o servidor de base de dados SQL do Azure.|
    |$sqlDatabaseLocation|Este valor é utilizado apenas quando estiver a criar um novo servidor de base de dados do Azure.|
    |$sqlDatabaseName|A base de dados SQL utilizada para criar a tabela de AvgDelays para a tarefa de Sqoop. Deixar em branco, irá criar uma base de dados chamado HDISqoop. O nome da tabela do resultado da tarefa de Sqoop é AvgDelays.|

    
2. Abra o ISE do PowerShell do Azure.

3. Copie e cole o seguinte script no painel de script:  

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the region to create the Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "https://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Connect-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resource group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command to create the AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]  
   > O script usa um serviço de transferência (REST) de estado representational https://bot.whatismyipaddress.com, para obter seu endereço IP externo. O endereço IP é utilizado para criar uma regra de firewall para o servidor de base de dados SQL.

    Aqui estão algumas variáveis no script:

   * **$ipAddressRestService** -o valor predefinido é https://bot.whatismyipaddress.com. É um serviço REST para obter seu endereço IP externo do endereço IP público. Pode utilizar outros serviços, se desejar. O endereço IP externo obtido por meio do serviço será utilizado para criar uma regra de firewall do servidor de base de dados SQL do Azure, para que possa acessar o banco de dados da sua estação de trabalho (ao utilizar um script do Windows PowerShell).
   * **$fireWallRuleName** -este é o nome da regra de firewall para o servidor de base de dados SQL do Azure. O nome predefinido é <u>FlightDelay</u>. Pode alterá-lo se desejar.
   * **$sqlDatabaseMaxSizeGB** -este valor é utilizado apenas quando estiver a criar um novo servidor de base de dados SQL do Azure. O valor predefinido é de 10GB. 10GB é suficiente para este tutorial.
   * **$sqlDatabaseName** -este valor é utilizado apenas quando estiver a criar uma nova base de dados SQL do Azure. O valor predefinido é HDISqoop. Se renomeá-lo, tem de atualizar o script do PowerShell de Windows Sqoop em conformidade.
4. Prima **F5** para executar o script.
5. Valide o resultado do script. Certifique-se de que o script foi executado com êxito.

## <a id="nextsteps"></a> Passos seguintes
Agora que compreende como carregar um ficheiro para o armazenamento de Blobs do Azure, como popular uma tabela do Apache Hive, utilizando os dados do armazenamento de Blobs do Azure, como executar consultas do Hive e como utilizar o Sqoop para exportar dados a partir [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) para um SQL do Azure base de dados. Para obter mais informações, consulte os artigos seguintes:

* [Introdução ao HDInsight][hdinsight-get-started]
* [Utilizar o Apache Hive com o HDInsight][hdinsight-use-hive]
* [Utilizar o Apache Oozie com o HDInsight][hdinsight-use-oozie]
* [Utilizar o Apache Sqoop com o HDInsight][hdinsight-use-sqoop]
* [Utilizar o Apache Pig com o HDInsight][hdinsight-use-pig]
* [Desenvolver programas Java MapReduce para o HDInsight][hdinsight-develop-mapreduce]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[rita-website]: https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: https://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
