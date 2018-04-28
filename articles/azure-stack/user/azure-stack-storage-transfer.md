---
title: Ferramentas para armazenamento de pilha do Azure
description: Saiba mais sobre os dados de armazenamento do Azure pilha ferramentas de transferência
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/25/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 2876565f3d6a3411eb170d4da640166fa3e607eb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tools-for-azure-stack-storage"></a>Ferramentas para armazenamento de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Microsoft Azure fornece um conjunto de serviços de armazenamento para discos, blobs, tabelas, filas e funções de gestão de conta. Pode utilizar um conjunto de ferramentas de armazenamento do Azure se pretender gerir ou mover dados de ou para armazenamento de pilha do Azure. Este artigo fornece uma descrição geral rápida das ferramentas disponíveis.

A ferramenta que funciona melhor para si depende das necessidades:
* [AZCopy](#azcopy)

    Utilitário de armazenamento específico, da linha de comandos que pode transferir para copiar dados de um objeto a outro objeto na sua conta de armazenamento, ou entre contas de armazenamento.

* [Azure PowerShell](#azure-powershell)

    Uma shell da linha de comandos, baseado em tarefas e linguagem de scripts foi concebido especialmente para administração de sistema.

* [CLI do Azure](#azure-cli)

    Uma ferramenta open source, entre plataformas que fornece um conjunto de comandos para trabalhar com as plataformas do Azure e pilha do Azure.

* [Explorador de armazenamento da Microsoft](#microsoft-azure-storage-explorer)

    Uma aplicação autónoma fáceis de utilizar com uma interface de utilizador.

Devido às diferenças de serviços de armazenamento entre o Azure e Azure pilha, poderão existir alguns requisitos específicos para cada ferramenta descritos nas secções seguintes. Para ver uma comparação entre o armazenamento de pilha do Azure e o armazenamento do Azure, consulte [Azure pilha de armazenamento: diferenças e as considerações](azure-stack-acs-differences.md).


## <a name="azcopy"></a>AzCopy

O AzCopy é um utilitário da linha de comandos concebido para copiar dados e de armazenamento de BLOBs e de tabela do Microsoft Azure através de comandos simples com um desempenho ideal. Pode copiar dados de um objeto para outro dentro da sua conta de armazenamento, ou entre contas de armazenamento.

### <a name="download-and-install-azcopy"></a>Transfira e instale o AzCopy

Existem duas versões do utilitário AzCopy: AzCopy no Windows e AzCopy no Linux.

 - **AzCopy no Windows**  
    - Transferir a versão suportada do AzCopy para pilha do Azure. Pode instalar e utilizar o AzCopy na pilha do Azure da mesma forma que o Azure. Para obter mais informações, consulte [AzCopy no Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).
        - Para a atualização 1802 ou versões mais recentes, [transferir AzCopy 7.1.0](https://aka.ms/azcopyforazurestack20170417).
        - Para versões anteriores, [transferir AzCopy 5.0.0](https://aka.ms/azcopyforazurestack20170417).

 - **AzCopy no Linux**  

    - AzCopy no Linux suporta a atualização do Azure pilha 1802 ou versões mais recentes. Pode instalar e utilizar o AzCopy na pilha do Azure da mesma forma que o Azure. Para obter mais informações, consulte [AzCopy no Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="azcopy-command-examples-for-data-transfer"></a>Exemplos de comandos do AzCopy para transferência de dados

Os exemplos seguintes siga cenários típicos para copiar dados para e de blobs de pilha do Azure. Para obter mais informações, consulte [AzCopy no Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) e [AzCopy no Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="download-all-blobs-to-a-local-disk"></a>Transferir todos os blobs num disco local

**Windows**

````AzCopy  
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
````

**Linux**

````AzCopy  
azcopy \
    --source https://myaccount.blob.local.azurestack.external/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
````

### <a name="upload-single-file-to-virtual-directory"></a>Carregar ficheiros única para o diretório virtual

**Windows**

```AzCopy  
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

**Linux**

````AzCopy  
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.local.azurestack.external/mycontainer/vd/abc.txt \
    --dest-key <key>
````

### <a name="move-data-between-azure-and-azure-stack-storage"></a>Mover dados entre o Azure e Azure pilha de armazenamento

Transferência de dados assíncrona entre o armazenamento do Azure e Azure pilha não é suportada. Tem de especificar a transferência com o **/SyncCopy** ou **-sync-cópia** opção.

**Windows**

````AzCopy  
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
````

**Linux**

````AzCopy  
azcopy \
    --source https://myaccount1.blob.local.azurestack.external/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --sync-copy
````

### <a name="azcopy-known-issues"></a>Problemas conhecidos do Azcopy

 - Qualquer operação AzCopy um arquivo de ficheiros não está disponível porque o armazenamento de ficheiros ainda não está disponível na pilha do Azure.
 - Transferência de dados assíncrona entre o armazenamento do Azure e Azure pilha não é suportada. Pode especificar a transferência com o **/SyncCopy** opção para copiar os dados.
 - A versão do Linux do Azcopy só suporta 1802 atualização ou versões posteriores. E não suporta o serviço tabela.

## <a name="azure-powershell"></a>Azure PowerShell

O Azure PowerShell é um módulo que fornece cmdlets para gerir serviços no Azure e pilha do Azure. É uma shell da linha de comandos, baseado em tarefas e linguagem de scripts foi concebido especialmente para administração de sistema.

### <a name="install-and-configure-powershell-for-azure-stack"></a>Instalar e configurar o PowerShell para a pilha do Azure

Pilha do Azure compatíveis módulos do PowerShell do Azure são necessários para funcionar com a pilha do Azure. Para obter mais informações, consulte [instale o PowerShell para Azure pilha](azure-stack-powershell-install.md) e [configurar o Azure pilha ambiente do utilizador PowerShell](azure-stack-powershell-configure-user.md) para obter mais informações.

### <a name="powershell-sample-script-for-azure-stack"></a>Script de exemplo do PowerShell para a pilha do Azure 

Este exemplo partem do princípio de que tem com êxito [instalado o PowerShell para o Azure pilha](azure-stack-powershell-install.md). Este script irão ajudá-lo a concluir a configuração e peça ao seu inquilino do Azure pilha credenciais para adicionar a sua conta para o ambiente de PowerShell local. Em seguida, o script irá predefinir a subscrição do Azure, criar uma nova conta de armazenamento no Azure, criar um novo contentor nesta nova conta de armazenamento e carregar um ficheiro de imagem existente (blob) para esse contentor. Depois do script apresenta uma lista de todos os blobs no contentor, irá criar um novo diretório de destino no seu computador local e transfira o ficheiro de imagem.

1. Instalar [módulos do Azure pilha compatível com o Azure PowerShell](azure-stack-powershell-install.md).  
2. Transferir o [ferramentas necessárias para trabalhar com a pilha de Azure](azure-stack-powershell-download.md).  
3. Abra **ISE do Windows PowerShell** e **executar como administrador**, clique em **ficheiro** > **novo** para criar um novo ficheiro de script.
4. Copie o script abaixo e cole o novo ficheiro de script.
5. Atualize as variáveis de script com base nas suas definições de configuração. 
  > ! [Nota]  
  > Este script tem de ser executado no diretório de raiz para **AzureStack_Tools**. 

```PowerShell 
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environemnt name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudiance = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase.
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\downlaod" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudience

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Add-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder

# end
```

### <a name="powershell-known-issues"></a>Problemas conhecidos do PowerShell 
A versão atual do compatível Azure PowerShell módulo para a pilha do Azure é 1.2.12. É diferente da versão mais recente do Azure PowerShell. Esta diferença afeta a operação de serviços de armazenamento:

* O formato do valor de retorno de `Get-AzureRmStorageAccountKey` versão 1.2.12 tem duas propriedades: `Key1` e `Key2`, enquanto a versão atual do Azure devolve uma matriz que contém todas as chaves de conta.
   ```
   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.4, and later versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Value[0]

   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.3.2, and previous versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Key1

   ```
   Para obter mais informações, consulte [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey?view=azurermps-4.1.0).

## <a name="azure-cli"></a>CLI do Azure
A CLI do Azure é a experiência de linha de comandos do Azure para gerir recursos do Azure. Pode instalá-lo no macOS, Linux e Windows e executá-la a partir da linha de comandos. 

CLI do Azure está otimizada para gerir e administrar recursos do Azure a partir da linha de comandos e para a criação de scripts de automatização que funcionam com o Azure Resource Manager. Fornece muitas das mesmas funções encontradas no portal do Azure pilha, incluindo acesso a dados avançado.

Pilha do Azure requer a CLI do Azure versão 2.0. Para obter mais informações sobre como instalar e configurar a CLI do Azure com a pilha do Azure, consulte [instalar e configurar a CLI do Azure pilha](azure-stack-version-profiles-azurecli2.md). Para obter mais informações sobre como utilizar o 2.0 CLI do Azure para efetuar várias tarefas de trabalhar com recursos na sua conta de armazenamento de pilha do Azure, consulte [utilizando o CLI2.0 do Azure com o Storage do Azure](../../storage/storage-azure-cli.md)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Script de exemplo do CLI do Azure para a pilha do Azure 
Depois de concluir a instalação do CLI e configuração, pode tentar os seguintes passos para trabalhar com um script de exemplo pequeno shell para interagir com recursos de armazenamento de pilha do Azure. O script cria pela primeira vez um novo contentor na sua conta de armazenamento, em seguida, carrega um ficheiro existente (como um blob) para esse contentor, apresenta uma lista de todos os blobs no contentor e por fim, transfere o ficheiro para um destino no seu computador local que especificar. Antes de executar este script, certifique-se de que o se ligar com êxito e o início de sessão para a pilha do Azure de destino. 
1. Abra o editor de texto favorito, em seguida, copie e cole o script anterior no editor.
2. Atualize as variáveis o script para refletir as definições de configuração. 
3. Depois de atualizou as variáveis necessárias, guarde o script e saia do editor. Os passos partem do princípio de que com o nome do script **my_storage_sample.sh**.
4. Marque o script como executável, se necessário: `chmod +x my_storage_sample.sh`
5. Execute o script. Por exemplo, no Bash: `./my_storage_sample.sh`

```bash
#!/bin/bash
# A simple Azure Stack Storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
```

## <a name="microsoft-azure-storage-explorer"></a>Explorador de Armazenamento do Microsoft Azure

Explorador de armazenamento do Microsoft Azure é uma aplicação autónoma da Microsoft. Permite-lhe trabalhar facilmente com o Storage do Azure e dados de armazenamento de pilha do Azure no Windows, macOS e Linux. Se pretender uma forma fácil de gerir os seus dados de armazenamento de pilha do Azure, em seguida, considere utilizar o Explorador de armazenamento do Microsoft Azure.

 - Para obter mais informações sobre como configurar o Explorador de armazenamento do Azure para trabalhar com a pilha do Azure, consulte [Explorador de armazenamento de ligar a uma subscrição do Azure pilha](azure-stack-storage-connect-se.md).
 - Para saber mais sobre o Explorador de armazenamento do Microsoft Azure, consulte o artigo [introdução ao Explorador de armazenamento](../../vs-azure-tools-storage-manage-with-storage-explorer.md)

## <a name="next-steps"></a>Passos Seguintes
* [Ligar o Explorador de armazenamento a uma subscrição de pilha do Azure](azure-stack-storage-connect-se.md)
* [Introdução ao Explorador de armazenamento](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Armazenamento do Azure consistente: diferenças e considerações](azure-stack-acs-differences.md)
* [Introdução ao Armazenamento do Microsoft Azure](../../storage/common/storage-introduction.md)

