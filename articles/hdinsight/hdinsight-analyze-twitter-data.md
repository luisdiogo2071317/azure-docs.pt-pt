---
title: Analisar dados do Twitter com o Hadoop no HDInsight - Azure
description: Saiba como utilizar o Hive para analisar dados do Twitter do Hadoop no HDInsight para determinar a frequência de utilização de uma palavra específica.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 294353cfcfba617ab19e703f11f35402bcf7ea82
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387586"
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analisar dados do Twitter com o Hive no HDInsight
Web sites sociais são uma das principais forças que para a adoção de grandes volumes de dados. APIs públicas, fornecidas por sites como o Twitter são uma fonte útil dos dados para analisar e compreender as tendências populares.
Neste tutorial, irá obter tweets com um API de transmissão em fluxo de Twitter e, em seguida, utilizar o Apache Hive no HDInsight do Azure para obter uma lista de utilizadores do Twitter que enviou os maioria dos tweets que continha uma determinada palavra.

> [!IMPORTANT]
> Os passos neste documento exigem um cluster do HDInsight baseado em Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows). Para obter passos específicos para um cluster baseado em Linux, veja [Twitter analisar dados com o Hive no HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de ter o seguinte:

* **Uma estação de trabalho** com o Azure PowerShell instalada e configurada.

    Para executar scripts do Windows PowerShell, tem de executar o Azure PowerShell como administrador e defina a política de execução para *RemoteSigned*. Ver [executar o Windows PowerShell scripts][powershell-script].

    Antes de executar scripts do Windows PowerShell, certifique-se de que está ligado à sua subscrição do Azure utilizando o cmdlet seguinte:

    ```powershell
    Connect-AzureRmAccount
    ```

    Se tiver várias subscrições do Azure, utilize o cmdlet seguinte para definir a subscrição atual:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > O suporte do Azure PowerShell para gerir recursos do HDInsight com o Gestor de Serviços do Azure está **preterido**, e foi removido a 1 de janeiro de 2017. Os passos neste documento utilizam os novos cmdlets do HDInsight que funcionam com o Azure Resource Manager.
    >
    > Siga os passos em [Instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para instalar a versão mais recente do Azure PowerShell. Se tiver scripts que tenham de ser modificados para utilizar os novos cmdlets que funcionam com o Azure Resource Manager, veja [Migrar para as ferramentas de desenvolvimento baseadas no Azure Resource Manager para clusters do HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) para obter mais informações.

* **Um cluster do Azure HDInsight**. Para obter instruções sobre o aprovisionamento do cluster, consulte [introdução à utilização do HDInsight] [ hdinsight-get-started] ou [Provision HDInsight clusters][hdinsight-provision]. Terá o nome do cluster mais tarde no tutorial.

A tabela seguinte lista os ficheiros utilizados neste tutorial:

| Ficheiros | Descrição |
| --- | --- |
| /Tutorials/twitter/data/tweets.txt |A origem de dados para a tarefa do Hive. |
| /Tutorials/twitter/output |A pasta de saída da tarefa do Hive. O nome de ficheiro de saída do padrão do Hive tarefa é **000000_0**. |
| Tutorials/twitter/twitter.hql |O ficheiro de script de HiveQL. |
| /Tutorials/twitter/JobStatus |O estado da tarefa de Hadoop. |

## <a name="get-twitter-feed"></a>Get no feed do Twitter
Neste tutorial, irá utilizar o [APIs de transmissão em fluxo do Twitter][twitter-streaming-api]. O Twitter específico, irá utilizar de API de transmissão em fluxo é [Estados/filtro][twitter-statuses-filter].

> [!NOTE]
> Um ficheiro que contenha 10.000 tweets e o ficheiro de script do Hive (abrangido na próxima seção) foram carregados num contentor do Blob público. Pode ignorar esta secção se pretender utilizar os ficheiros carregados.

Dados de tweets são armazenados no formato JavaScript Object Notation (JSON) que contém uma estrutura aninhada complexa. Em vez de escrever muitas linhas de código ao utilizar uma linguagem de programação convencional, pode transformar esta estrutura aninhada numa tabela do Hive, para que ele pode ser consultado por uma linguagem SQL (Structured Query)-como linguagem denominada HiveQL.

Twitter utiliza o OAuth para fornecer acesso autorizado para sua API. OAuth é um protocolo de autenticação que permite aos utilizadores aprovar as aplicações para agir em nome sem partilhar a palavra-passe. Obter mais informações podem ser encontradas em [oauth.net](http://oauth.net/) ou no excelente [guia para iniciantes para o OAuth](http://hueniverse.com/oauth/) de Hueniverse.

É o primeiro passo para utilizar o OAuth criar um novo aplicativo no site do desenvolvedor do Twitter.

**Para criar uma aplicação no Twitter**

1. Inicie sessão em [https://apps.twitter.com/](https://apps.twitter.com/). Clique nas **Inscreva-se agora** ligar se não tiver uma conta do Twitter.
2. Clique em **criar nova aplicação**.
3. Introduza **Name**, **Descrição**, **site**. Pode compor um URL para o **Web site** campo. A tabela seguinte mostra alguns valores de exemplo para utilizar:

   | Campo | Valor |
   | --- | --- |
   |  Nome |MyHDInsightApp |
   |  Descrição |MyHDInsightApp |
   |  Site |http://www.myhdinsightapp.com |
4. Verifique **Sim, eu Concordo**e, em seguida, clique em **criar a sua aplicação do Twitter**.
5. Clique nas **permissões** separador. A permissão de predefinição está **só de leitura**. Isso é suficiente para este tutorial.
6. Clique nas **chaves e Tokens de acesso** separador.
7. Clique em **criar meu token de acesso**.
8. Clique em **OAuth de teste** no canto superior direito da página.
9. Anote **chave de consumidor**, **segredo de consumidor**, **token de acesso**, e **segredo de token de acesso**. Terá dos valores mais tarde no tutorial.

Neste tutorial, utilize o Windows PowerShell para fazer o chamada do serviço web. Outra ferramenta popular para fazer chamadas de serviço da web é [ *Curl*][curl]. Curl pode ser transferido a partir [aqui][curl-download].

> [!NOTE]
> Ao utilizar o comando curl no Windows, utilize aspas duplas em vez de aspas simples para os valores de opção.

**Para obter tweets**

1. Abra o Windows PowerShell Integrated Scripting Environment (ISE). (Na tela Iniciar do Windows 8, digite **PowerShell_ISE** e, em seguida, clique em **ISE do Windows PowerShell**. Consulte [iniciar o Windows PowerShell no Windows 8 e Windows](https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell?view=powershell-6)
2. Copie o seguinte script para o painel de scripts:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Connect-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Defina os primeiros cinco a oito variáveis no script:

    Variável|Descrição
    ---|---
    $clusterName|Este é o nome do cluster HDInsight em que pretende executar a aplicação.
    $oauth_consumer_key|Este é o aplicativo do Twitter **chave de consumidor** escrevi, anteriormente quando criou a aplicação do Twitter.
    $oauth_consumer_secret|Este é o aplicativo do Twitter **segredo de consumidor** escrevi, anteriormente.
    $oauth_token|Este é o aplicativo do Twitter **token de acesso** escrevi, anteriormente.
    $oauth_token_secret|Este é o aplicativo do Twitter **segredo de token de acesso** escrevi, anteriormente.
    $destBlobName|Este é o nome de blob de saída. O valor predefinido é **tutorials/twitter/data/tweets.txt**. Se alterar o valor predefinido, terá de atualizar os scripts do Windows PowerShell em conformidade.
    $trackString|O serviço web irá devolver tweets relacionados com estas palavras-chave. O valor predefinido é **do Azure, Cloud, HDInsight**. Se alterar o valor predefinido, irá atualizar os scripts do Windows PowerShell em conformidade.
    $lineMax|O valor determina quantos tweets irá ler o script. Demora cerca de três minutos para ler 100 tweets. Pode definir um número maior, mas irá demorar mais tempo para transferir.

1. Prima **F5** para executar o script. Caso se depare com problemas, como solução, selecione todas as linhas e, em seguida, prima **F8**.
2. Deverá ver "Complete!" no final de saída. As mensagens de erro serão apresentadas em vermelho.

Como um procedimento de validação, pode verificar o ficheiro de saída **/tutorials/twitter/data/tweets.txt**, no seu armazenamento de Blobs do Azure utilizando um Explorador de armazenamento do Azure ou o Azure PowerShell. Para um script do Windows PowerShell de exemplo para listar ficheiros, consulte [armazenamento de BLOBs de utilização com o HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Criar script de HiveQL
Com o Azure PowerShell, pode executar várias declarações HiveQL um de cada vez ou a instrução de HiveQL para um ficheiro de script do pacote. Neste tutorial, irá criar um script de HiveQL. O ficheiro de script tem de ser carregado para o armazenamento de Blobs do Azure. Na secção seguinte, irá executar o ficheiro de script com o Azure PowerShell.

> [!NOTE]
> O ficheiro de script do Hive e um ficheiro que contenha 10.000 tweets foram carregados num contentor do Blob público. Pode ignorar esta secção se pretender utilizar os ficheiros carregados.

O script de HiveQL irá executar o seguinte:

1. **Remover a tabela de tweets_raw** no caso da tabela já existe.
2. **Criar a tabela de Hive tweets_raw**. Este ramo de registo temporário estruturada tabela contém os dados para obter mais extrair, transformar e carregar o processamento (ETL). Para obter informações sobre as partições, consulte [Hive tutorial][apache-hive-tutorial].
3. **Carregar dados** da pasta de origem, /tutorials/twitter/data. O conjunto de dados de grandes tweets no formato JSON aninhado agora ter sido transformado numa estrutura de tabela do Hive temporária.
4. **Remover a tabela de tweets** no caso da tabela já existe.
5. **Criar a tabela de tweets**. Pode consultar o conjunto de dados de tweets através do Hive, terá de executar outro processo ETL. Este processo ETL define um esquema de tabela mais detalhado para os dados que tem armazenados na tabela "twitter_raw".
6. **Inserir tabela de substituição**. Este script de Hive complexo para iniciar um conjunto de tarefas de MapReduce longo pelo cluster do Hadoop. Dependendo do seu conjunto de dados e o tamanho do seu cluster, esta ação pode demorar cerca de 10 minutos.
7. **INSERT substituir directory**. Executar uma consulta e o conjunto de dados para um ficheiro de saída. Essa consulta retornará uma lista de utilizadores do Twitter que enviou a maioria dos tweets que continha a palavra "Azure".

**Para criar um script do Hive e carregá-lo para o Azure**

1. Abra o ISE do Windows PowerShell.
2. Copie o seguinte script para o painel de scripts:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Defina as duas primeiras variáveis no script:

   | Variável | Descrição |
   | --- | --- |
   |  $clusterName |Introduza o nome de cluster do HDInsight em que pretende executar a aplicação. |
   |  $subscriptionID |Introduza o ID de subscrição do Azure. |
   |  $sourceDataPath |A localização de armazenamento de Blobs do Azure onde as consultas do Hive irão ler os dados do. Não precisa de alterar essa variável. |
   |  $outputPath |A localização de armazenamento de Blobs do Azure onde as consultas do Hive produzirá os resultados. Não precisa de alterar essa variável. |
   |  $hqlScriptFile |A localização e o nome de ficheiro do ficheiro de script de HiveQL. Não precisa de alterar essa variável. |
4. Prima **F5** para executar o script. Caso se depare com problemas, como solução, selecione todas as linhas e, em seguida, prima **F8**.
5. Deverá ver "Complete!" no final de saída. As mensagens de erro serão apresentadas em vermelho.

Como um procedimento de validação, pode verificar o ficheiro de saída **/tutorials/twitter/twitter.hql**, no seu armazenamento de Blobs do Azure utilizando um Explorador de armazenamento do Azure ou o Azure PowerShell. Para um script do Windows PowerShell de exemplo para listar ficheiros, consulte [armazenamento de BLOBs de utilização com o HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Processar os dados do Twitter através do Hive
Concluiu todo o trabalho de preparação. Agora, pode invocar o script do Hive e verificar os resultados.

### <a name="submit-a-hive-job"></a>Submeter uma tarefa do Hive
Utilize o seguinte script do Windows PowerShell para executar o script do Hive. Terá de definir a variável primeiro.

> [!NOTE]
> Para utilizar os tweets e o script de HiveQL carregou nas últimas duas secções, defina $hqlScriptFile como "/ tutorials/twitter/twitter.hql". Para utilizar os que foram carregados para um blob público para, defina $hqlScriptFile como "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace �\s*$� -replace �.*\s�
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>Verificar os resultados
Utilize o seguinte script do Windows PowerShell para verificar o resultado da tarefa do Hive. Terá de definir as primeiras duas variáveis.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> A tabela de Hive utiliza \001 como o delimitador de campos. O delimitador não é visível na saída.

Depois dos resultados da análise foram colocados no armazenamento de Blobs do Azure, pode exportar os dados para um servidor de base de dados /SQL SQL do Azure, exporte os dados para o Excel, com o Power Query ou ligar a aplicação aos dados ao utilizar o controlador ODBC do Hive. Para obter mais informações, consulte [utilização Sqoop com o HDInsight][hdinsight-use-sqoop], [analisar dados de atraso de voo com o HDInsight][hdinsight-analyze-flight-delay-data], [ Ligar o Excel ao HDInsight com o Power Query][hdinsight-power-query], e [ligar o Excel ao HDInsight com o controlador Microsoft Hive ODBC][hdinsight-hive-odbc].

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial vimos como transformar um conjunto de dados não estruturado do JSON para uma tabela do Hive estruturada para consultar, explorar e analisar dados do Twitter com o HDInsight no Azure. Para saber mais, consulte:

* [Introdução ao HDInsight][hdinsight-get-started]
* [Analisar dados de atraso de voo com o HDInsight][hdinsight-analyze-flight-delay-data]
* [Ligar o Excel ao HDInsight com o Power Query][hdinsight-power-query]
* [Ligar o Excel ao HDInsight com o controlador Microsoft Hive ODBC][hdinsight-hive-odbc]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop] (Utilizar o Sqoop com o HDInsight)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://developer.twitter.com/en/docs/api-reference-index
[twitter-statuses-filter]: https://developer.twitter.com/en/docs/tweets/filter-realtime/api-reference/post-statuses-filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
