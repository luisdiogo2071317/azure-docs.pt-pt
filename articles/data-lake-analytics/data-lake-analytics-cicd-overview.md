---
title: Como configurar o pipeline CI/CD para o Azure Data Lake Analytics | Documentos da Microsoft
description: Saiba como configurar a integração contínua e implementação contínua para Azure Data Lake Analytics.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: c069bc2a6147a021ea9bdf37e2926d5c8f33281c
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145014"
---
# <a name="how-to-set-up-cicd-pipeline-for-azure-data-lake-analytics"></a>Como configurar o pipeline CI/CD para o Azure Data Lake Analytics

Neste documento, irá aprender a configurar o pipeline de CI/CD para tarefas de U-SQL e bases de dados U-SQL.

## <a name="cicd-for-u-sql-job"></a>CI/CD para tarefa U-SQL

O Azure Data Lake Tools para Visual Studio fornece o tipo de projeto U-SQL que o ajuda a organizar os scripts U-SQL. Usando o projeto de U-SQL para gerir o seu código de U-SQL torna os cenários de CI/CD mais facilmente.

## <a name="build-u-sql-project"></a>Criar projeto U-SQL

O projeto de U-SQL pode ser criado com o MSBuild, passando parâmetros correspondentes. Siga os passos abaixo para configurar o processo de compilação para projetos de U-SQL.

### <a name="project-migration"></a>Migração do projeto

Antes de configurar tarefas de compilação para o projeto U-SQL, certifique-se utilizar a versão mais recente do projeto U-SQL. Abra o ficheiro de projeto do U-SQL no editor e verificar se tem abaixo itens de importação:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Caso contrário, tem duas opções para migrar o projeto:

- Opção 1: Alteração o antigo se importar com a descrita anteriormente.
- Opção 2: Abra o projeto antigo no Azure Data Lake Tools para Visual Studio depois da versão 2.3.3000.0. O modelo de projeto antigo será atualizado automaticamente para a versão mais recente. O novo projeto criado após a versão 2.3.3000.0 utiliza diretamente o novo modelo.

### <a name="get-nuget-package"></a>Obter o pacote NuGet

MSBuild não fornece suporte interno para o tipo de projeto U-SQL. Para adicionar essa capacidade, tem de adicionar uma referência para a sua solução para o [pacote Microsoft.Azure.DataLake.USQL.SDK Nuget](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) que adiciona o serviço de idiomas necessários.

Para adicionar a referência de pacote do NuGet, com o botão direito a solução no Explorador de soluções e escolha **gerir pacotes NuGet**. Ou pode adicionar um arquivo chamado `packages.config` na pasta de soluções e adicione abaixo conteúdo para o mesmo.

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>Gerir as referências de base de dados U-SQL

Se os scripts U-SQL no projeto U-SQL tem instruções de consulta para objetos de banco de dados U-SQL, por exemplo, consultar uma tabela de U-SQL ou referenciar um assembly, precisa referenciar o projeto de banco de dados U-SQL correspondente que inclui a definição desses objetos antes de a criar este projeto U-SQL.

[Saiba mais sobre o projeto de banco de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

>[!NOTE]
>Projeto de banco de dados U-SQL está atualmente em pré-visualização pública. Se tiver a instrução DROP no projeto, a compilação falha. A instrução DROP será permitida em breve.
>

### <a name="build-u-sql-project-with-msbuild-command-line"></a>Criar projeto U-SQL com linha de comando do MSBuild

Depois de migrar o projeto e obter o pacote NuGet, pode chamar a linha de comando do MSBuild padrão com os argumentos adicionais abaixo para criar o seu projeto de U-SQL:

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

A definição de argumentos e valores são:

* USQLSDKPath = < pacote Nuget do U-SQL > \build\runtime: este parâmetro refere-se para o caminho de instalação do pacote NuGet para o serviço de linguagem U-SQL.
* USQLTargetType = intercalação ou SyntaxCheck:
    * Intercalação: O modo de intercalação compila arquivos code-behind, como linhas internas da biblioteca de código definido pelo utilizador resultante, ficheiros. PY e. r e. CS (como um binário de dll, Python ou R de código) para o script de U-SQL.
    * SyntaxCheck: Modo SyntaxCheck primeiro intercala arquivos code-behind para o script de U-SQL e, em seguida, compila o script de U-SQL para validar o seu código.
* DataRoot =<DataRoot path>: DataRoot só é necessário para o modo de SyntaxCheck. Ao criar o script com o modo de SyntaxCheck, MSBuild verifica as referências no script para objetos de base de dados. Certifique-se configurar um ambiente local correspondente que contém os objetos referenciados da base de dados U-SQL na pasta DataRoot de máquina de compilação antes da criação. Também pode gerir estas dependências de base de dados por [referenciar um projeto de banco de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). Tenha em atenção que o MSBuild verifica somente a referência de objetos de base de dados, não os ficheiros.
* EnableDeployment = VERDADEIRO ou FALSO: EnableDeployment indica se é permitido para implementar bases de dados de U-SQL referenciados durante o processo de compilação. Se referenciar o projeto de banco de dados U-SQL e consumir os objetos de base de dados no seu script de U-SQL, defina este parâmetro como true.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Integração contínua com o Visual Studio Team Service

Além da linha de comandos, os clientes também podem utilizar tarefas de compilação do Visual Studio ou o MSBuild para criar projetos de U-SQL no Visual Studio Team Service. Para configurar a tarefa de compilação, certifique-se:

1.  Adicionar a tarefa de restauro do NuGet para obter a solução referenciado incluindo o pacote NuGet `Azure.DataLake.USQL.SDK`, para que o MSBuild, pode encontrar os destinos de linguagem U-SQL. Definir **avançadas > diretório de destino** como `$(Build.SourcesDirectory)/packages` se pretender utilizar o exemplo de argumentos de MSBuild diretamente no passo 2.

    ![Tarefa do MSBuild de CD de CI definir Lake de dados para o projeto U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

    ![Tarefa de Nuget de CD de CI definir Lake de dados para o projeto U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  Argumentos de MSBuild do conjunto e pode definir os argumentos na tarefa de compilação do Visual Studio ou o MSBuild, como abaixo ou pode definir as variáveis para esses argumentos na definição de compilação do VSTS.

    ```
    /p:USQLSDKPath=/p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

    ![Data Lake definir CI CD MSBuild variáveis para o projeto U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

### <a name="u-sql-project-build-output"></a>Saída de compilação do projeto U-SQL

Depois de executar a compilação, todos os scripts no projeto U-SQL criados e saídos para um arquivo zip chamado `USQLProjectName.usqlpack`. A estrutura de pastas no seu projeto será mantida na saída da compilação zipados.

>[!NOTE]
>
>Code-behind do ficheiro para cada script de U-SQL será unida como instrução de inline para a saída de compilação de scripts.
>

## <a name="test-u-sql-script"></a>Testar o script U-SQL

O Azure Data Lake fornece projetos de teste para o script de U-SQL e c# UDO/UDAG/UDF:
* [Saiba como adicionar casos de teste de script de U-SQL e o código c# expandido](data-lake-analytics-cicd-test.md#test-u-sql-scripts)
* [Saiba como executar os casos de teste no Visual Studio Team Service](data-lake-analytics-cicd-test.md#run-test-cases-in-visual-studio-team-service)

## <a name="u-sql-job-deployment"></a>Implementação da tarefa U-SQL

Depois de verificar o código por meio de compilação e teste o processo, pode submeter tarefas de U-SQL diretamente a partir do Visual Studio Team Service através de **tarefas de Azure PowerShell**. Também pode implementar o script para armazenamento de Blobs do Azure Data Lake Store/do Azure e [executar as tarefas agendadas através do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-visual-studio-team-service"></a>Submeter tarefas de U-SQL através do Visual Studio Team Service

A compilação de saída do projeto U-SQL é um ficheiro zip chamado **USQLProjectName.usqlpack** inclui todos os scripts U-SQL no projeto. Pode utilizar o [tarefas de Azure PowserShell no Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) com script de PowserShell de exemplo para submeter o U-SQL abaixo tarefas diretamente do Visual Studio Team Service criar ou pipeline de versões.

```powershell
<#
    This script can be used to submit U-SQL Jobs with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and submit all scripts one-by-one.

    Note: the code behind file for each U-SQL script will be merged into the built U-SQL script in build output.
          
    Example :
        USQLJobSubmission.ps1 -ADLAAccountName "myadlaaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\" -DegreeOfParallelism 2
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLAAccountName, # ADLA account name to submit U-SQL jobs
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

## Get U-SQL scripts in U-SQL project build output(.usqlpack file)
Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue

    return $USQLFiles
}

## Submit U-SQL scripts to ADLA account one-by-one
Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."

    # Submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzureRmDataLakeAnalyticsJob -Account $ADLAAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        
        LogJobInformation $jobToSubmit
        
        Write-Output "Waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzureRmDataLakeAnalyticsJob -Account $ADLAAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{
    Write-Output ([string]::Format("ADLA account: {0}", $ADLAAccountName))
    Write-Output ([string]::Format("Root folde for usqlpack: {0}", $ArtifactsRoot))
    Write-Output ([string]::Format("AU count: {0}", $DegreeOfParallelism))

    Write-Output "Starting USQL script deployment..."
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>Implementar tarefas de U-SQL através do Azure Data Factory

Além de submissão de tarefas de U-SQL diretamente a partir do Visual Studio Team Service, também pode carregar os scripts criados para o armazenamento de Blobs do Azure Data Lake Store/Azure e [executar as tarefas agendadas através do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Utilize o [tarefas do PowerShell do Azure no Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) com abaixo de script do PowerShell de exemplo para carregar o U-SQL scripts para a conta do Azure Data Lake Store.

```powershell
<#
    This script can be used to upload U-SQL files to ADLS with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and upload all scripts to ADLS one-by-one.
          
    Example :
        FileUpload.ps1 -ADLSName "myadlsaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\"
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLSName, # ADLS account name to upload U-SQL scripts
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DesitinationFolder = "USQLScriptSource" # Desitination folder in ADLS
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles

    $files = @(get-childitem $usqlScripts -recurse)
    foreach($file in $files)
    {
        Write-Host "Uploading file: $($file.Name)"
        Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/$(Join-Path $DesitinationFolder $file)" -Force
    }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue
}

UploadResources
```

## <a name="cicd-for-u-sql-database"></a>CI/CD para base de dados U-SQL

O Azure Data Lake Tools para Visual Studio fornece o modelo de projeto de banco de dados U-SQL ajuda os desenvolvedores a desenvolver, gerir e implementar as bases de dados U-SQL, rápida e facilmente. [Saiba mais sobre o projeto de banco de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>Criar projeto de banco de dados U-SQL

### <a name="get-nuget-package"></a>Obter o pacote NuGet

MSBuild não fornece suporte interno para o tipo de projeto de banco de dados U-SQL. Para adicionar essa capacidade, tem de adicionar uma referência para a sua solução para o [pacote Microsoft.Azure.DataLake.USQL.SDK Nuget](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) que adiciona o serviço de idiomas necessários.

Para adicionar a referência de pacote do NuGet, pode clicar com o botão direito a solução no Explorador de soluções e escolha **gerir pacotes NuGet** para a solução, em seguida, procure e instale o pacote NuGet. Ou pode adicionar um arquivo chamado "Packages" na pasta da solução e adicionar abaixo conteúdo para o mesmo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-database-project-with-msbuild-command-line"></a>Criar projeto de banco de dados U-SQL com linha de comando do MSBuild

Pode chamar a linha de comando do MSBuild e passe a referência de pacote de NuGet do SDK de U-SQL como argumento adicional, conforme mostrado abaixo para criar o seu projeto de banco de dados U-SQL padrão:

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Os argumentos `USQLSDKPath=<U-SQL Nuget package>\build\runtime` refere-se para o caminho de instalação do pacote NuGet para o serviço de linguagem U-SQL.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Integração contínua com o Visual Studio Team Service

Além da linha de comandos, os clientes também podem usar **compilação do Visual Studio** ou **tarefa do MSBuild** para criar projetos de banco de dados U-SQL no Visual Studio Team Service. Para configurar a tarefa de compilação, certifique-se:

1.  Adicionar a tarefa de restauro do NuGet para obter a solução referenciado incluindo o pacote NuGet `Azure.DataLake.USQL.SDK`, para que o MSBuild, pode encontrar os destinos de linguagem U-SQL. Definir **avançadas > diretório de destino** como `$(Build.SourcesDirectory)/packages` se pretender utilizar o exemplo de argumentos de MSBuild diretamente no passo 2.

    ![Tarefa do MSBuild de CD de CI definir Lake de dados para o projeto U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

    ![Tarefa de Nuget de CD de CI definir Lake de dados para o projeto U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  Argumentos de MSBuild do conjunto e pode definir os argumentos na tarefa de compilação do Visual Studio ou o MSBuild, como abaixo ou pode definir as variáveis para esses argumentos na definição de compilação do VSTS.

    ```
    /p:USQLSDKPath=/p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
    ```

    ![Data Lake definir variáveis de MSBuild do CD de CI para o projeto de banco de dados U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

### <a name="u-sql-database-project-build-output"></a>Saída de compilação do projeto de banco de dados do U-SQL

A compilação de saída para o projeto de banco de dados U-SQL é um pacote de implementação da base de dados U-SQL, com o nome com o sufixo `.usqldbpack`. O `.usqldbpack` pacote é um ficheiro zip inclui todas as instruções DDL num único script de U-SQL na pasta DDL e todas as DLLs e ficheiros adicionais para assemblies na pasta Temp.

## <a name="test-table-valued-function-and-stored-procedure"></a>Função de valor de tabela de teste e o procedimento armazenado

Neste momento, a adição de casos de teste de funções com valor de tabela e procedimentos armazenados diretamente não é suportada. Como solução, pode criar um projeto de U-SQL que tem scripts U-SQL chamar essas funções e escrever casos de teste para os mesmos. Siga os passos para configurar a casos de teste de funções com valor de tabela e procedimentos armazenados definidos no projeto de banco de dados U-SQL abaixo:

1.  Criar um projeto de U-SQL para fins de teste e escrever scripts U-SQL, chamando as funções com valor de tabela e os procedimentos armazenados.
2.  Adicione referência de base de dados para este projeto U-SQL. Para obter a função com valor de tabela e a definição de procedimento armazenado, precisa referenciar o projeto de banco de dados que contém a instrução DDL. [Saiba mais sobre a referência de base de dados](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Adicione casos de teste para os scripts U-SQL que chamam funções com valor de tabela e procedimentos armazenados. [Saiba como adicionar casos de teste de script de U-SQL](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-visual-studio-team-service"></a>Implementar a base de dados U-SQL através do Visual Studio Team Service

`PackageDeploymentTool.exe` Fornece as interfaces de linha de comandos que ajudam a implementar package(.usqldbpack) de implementação do U-SQL da base de dados e programação. O SDK está incluído no [pacote NuGet do SDK U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), localização na build/runtime/PackageDeploymentTool.exe. Ao utilizar `PackageDeploymentTool.exe`, pode implementar bases de dados U-SQL para o Azure Data Lake Analytics e conta local.

>[!NOTE]
>
>Suporte de tarefas para a implementação do U-SQL da base de dados de versão do suporte de linha e o Visual Studio Team Service de comando do PowerShell está o caminho.
>

Siga os passos para configurar a tarefa de implementação de base de dados no Visual Studio Team Service abaixo:

1. Adicionar uma tarefa de Script do PowerShell são criados ou pipeline de versões e executar script do PowerShell abaixo. Esta tarefa ajuda a obter dependências de SDK do Azure para `PackageDeploymentTool.exe` e `PackageDeploymentTool.exe`. Pode definir os parâmetros - AzureSDK e - DBDeploymentTool para carregar as dependências e a ferramenta de implementação para algumas pastas específicas. Transmitir - AzureSDK caminho para `PackageDeploymentTool.exe` como parâmetro - AzureSDKPath no passo 2. 

    ```powershell
    <#
        This script is used for getting dependencies and SDKs for U-SQL database deployment.
        PowerShell command line support for deploying U-SQL database package(.usqldbpack file) will come soon.
        
        Example :
            GetUSQLDBDeploymentSDK.ps1 -AzureSDK "AzureSDKFolderPath" -DBDeploymentTool "DBDeploymentToolFolderPath"
    #>

    param (
        [string]$AzureSDK = "AzureSDK", # Folder to cache Azure SDK dependencies
        [string]$DBDeploymentTool = "DBDeploymentTool", # Folder to cache U-SQL dabatase deployment tool
        [string]$workingfolder = "" # Folder to execute these command lines
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.2.3-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.3.3-preview -outf Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip
    iwr https://www.nuget.org/api/v2/package/Newtonsoft.Json/6.0.8 -outf Newtonsoft.Json.6.0.8.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.DataLake.USQL.SDK/ -outf USQLSDK.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.3.3-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force
    Expand-Archive Newtonsoft.Json.6.0.8.zip -DestinationPath Newtonsoft.Json.6.0.8 -Force
    Expand-Archive USQLSDK.zip -DestinationPath USQLSDK -Force

    echo "Copy required DLLs to output folder..."

    mkdir $AzureSDK -Force
    mkdir $DBDeploymentTool -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.Azure.Management.DataLake.Store.2.3.3-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $AzureSDK
    copy Newtonsoft.Json.6.0.8\lib\net45\*.dll $AzureSDK
    copy USQLSDK\build\runtime\*.* $DBDeploymentTool
    ```

2. Adicionar uma **tarefa da linha de comandos** além de criar ou versão pipeline e preencha a chamada de script `PackageDeploymentTool.exe`. `PackageDeploymentTool.exe` está localizado na pasta $DBDeploymentTool definidos. O script de exemplo é o seguinte: 

    * Implemente U-SQL da base de dados localmente

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Utilize o modo de autenticação interativa para implementar base de dados U-SQL para a conta do Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Utilize secrete autenticação para implementar a base de dados U-SQL para a conta do Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Utilize autenticação certFile para implementar a base de dados U-SQL para a conta do Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

**Descrição do parâmetro PackageDeploymentTool.exe:**

**Parâmetros comuns:**

|Parâmetro|Descrição|Valor Predefinido|Necessário|
|---------|-----------|-------------|--------|
|Pacote|Caminho do pacote de implementação do U-SQL da base de dados a serem implantados|Nulo|true|
|Base de Dados|Nome devem ser implantados de base de dados / ou criado|master|false|
|Ficheiro de registo|Caminho do ficheiro de registo predefinido ao standard out (consola)|Nulo|false|
|LogLevel|Nível de registo: verboso, Normal, aviso, erro|LogLevel.Normal|false|

**Parâmetro de implementação de local:**

|Parâmetro|Descrição|Valor Predefinido|Necessário|
|---------|-----------|-------------|--------|
|DataRoot|Caminho da pasta de raiz de dados local|Nulo|true|

**Parâmetro de implementação do Azure Data Lake Analytics:**

|Parâmetro|Descrição|Valor Predefinido|Necessário|
|---------|-----------|-------------|--------|
|Conta|Especifica a implementação em que nome de conta por conta do Azure Data Lake Analytics|Nulo|true|
|ResourceGroup|Nome do grupo de recursos do Azure para a conta do Azure Data Lake Analytics|Nulo|true|
|SubscriptionId|ID de subscrição do Azure para a conta do Azure Data Lake Analytics|Nulo|true|
|Inquilino|Nome do inquilino (nome de domínio do AAD do, pode encontrá-lo na página de gestão da subscrição no portal do Azure)|Nulo|true|
|AzureSDKPath|Caminho para procurar conjuntos de módulos dependentes no SDK do Azure|Nulo|true|
|Interativo|Utilizar o modo interativo para autenticação ou não|false|false|
|ID de cliente|ID de aplicação do AAD para nenhuma autenticação interativa, necessária para nenhuma autenticação interativa|Nulo|necessário para nenhuma autenticação interativa|
|Secrete|Secrete/palavra-passe para nenhuma autenticação interativa, ele só deve utilizar no ambiente fidedigno/seguro|Nulo|necessário para nenhuma autenticação interativa ou utilização SecreteFile|
|SecreteFile|Ficheiro guarda secrete/palavra-passe para nenhuma autenticação interativa, certifique-se para mantê-la apenas para leitura ao utilizador atual|Nulo|necessário para nenhuma autenticação interativa ou utilização Secrete|
|CertFile|Ficheiro guarda certificação X.509 para nenhuma autenticação interativa, a predefinição é utilizar o cliente secrete autenticação|Nulo|false|
|JobPrefix|Prefixo para a implementação de base de dados da tarefa de DDL do U-SQL|Deploy_ + DateTime. Now|false|

## <a name="next-steps"></a>Próximos Passos

- [Como testar seu código do Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
- [Execute o script de U-SQL no seu computador local](data-lake-analytics-data-lake-tools-local-run.md)
- [Utilizar o projeto de banco de dados U-SQL para desenvolver a base de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)
