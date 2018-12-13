---
title: Início rápido para gerir partilhas de ficheiros do Azure com o Azure PowerShell
description: Utilize este início rápido para saber como gerir partilhas de ficheiros do Azure com o Azure PowerShell.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: a82e7d795d9e40ebef8cf0937dd2b91f5bacd42e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138879"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Início rápido: Criar e gerir uma partilha de ficheiros do Azure com o Azure PowerShell 
Este guia orienta-o pelas noções básicas da utilização de [partilhas de ficheiros do Azure](storage-files-introduction.md) com o PowerShell. As partilhas de ficheiros do Azure são como outras partilhas de ficheiros, mas armazenadas na cloud e apoiadas pela plataforma do Azure. As partilhas de ficheiros do Azure suportam o protocolo SMB padrão do setor e permite a partilha de ficheiros entre várias máquinas, aplicações e instâncias. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se quiser instalar e utilizar o PowerShell localmente, este guia requer a versão 5.1.1 ou posterior do módulo do Azure PowerShell. Para saber qual é a versão do módulo do Azure PowerShell que está a executar, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar o PowerShell localmente, também terá de executar `Login-AzureRmAccount` para iniciar sessão na sua conta do Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Se ainda não tiver um grupo de recursos do Azure, pode criar um novo com o cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na região E.U.A Leste:

```azurepowershell-interactive
New-AzureRmResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
Uma conta de armazenamento é um conjunto partilhado de armazenamento que pode utilizar para implementar partilhas de ficheiros do Azure ou outros recursos de armazenamento, como blobs ou filas. Uma conta de armazenamento pode conter um número ilimitado de partilhas e uma partilha pode armazenar um número ilimitado de ficheiros, até aos limites de capacidade da conta de armazenamento.

Este exemplo cria uma conta de armazenamento com o cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). A conta de armazenamento é criada com o nome *mystorageaccount<random number>* e é armazenada uma referência a essa conta de armazenamento na variável **$storageAcct**. Os nomes de contas de armazenamento têm de ser exclusivos, por isso, utilize `Get-Random` para anexar um número ao nome e torná-lo exclusivo. 

```azurepowershell-interactive 
$storageAcct = New-AzureRmStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure
Agora, pode criar a sua primeira partilha de ficheiros do Azure. Pode criar uma partilha de ficheiros com o cmdlet [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare). Este exemplo cria uma partilha com o nome `myshare`.

```azurepowershell-interactive
New-AzureStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

Os nomes das partilhas só podem ter letras minúsculas, números e hífenes, mas não podem começar com um hífen. Para obter detalhes completos sobre a nomenclatura de partilhas de ficheiros e ficheiros, veja [Naming and Referencing Shares, Directories, Files, and Metadata (Nomenclatura e Referência de Partilhas, Diretórios, Ficheiros e Metadados)](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="use-your-azure-file-share"></a>Utilizar a partilha de ficheiros do Azure
O serviço Ficheiros do Azure fornece dois métodos para utilizar ficheiros e pastas na sua partilha de ficheiros do Azure : o [protocolo SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) padrão do setor e o [protocolo REST de Ficheiros](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Para montar uma partilha de ficheiros com SMB, veja o documento seguinte com base no seu SO:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Utilizar uma partilha de ficheiros do Azure com o protocolo REST de Ficheiros 
É possível trabalhar diretamente com o protocolo REST de Ficheiros (ou seja, criar manualmente chamadas HTTP REST), mas a forma mais comum de utilizar o protocolo REST de Ficheiros é através do módulo AzureRM PowerShell, da [CLI do Azure](storage-how-to-use-files-cli.md) ou de um SDK do Armazenamento do Azure, sendo que todos fornecem um bom wrapper para o protocolo REST de Ficheiros na linguagem de scripts/programação da sua preferência.  

Na maioria dos casos, irá utilizar a partilha de ficheiros do Azure através do protocolo SMB, pois permite-lhe utilizar as aplicações e ferramentas existentes, mas existem vários motivos pelos quais é vantajoso utilizar a API REST de Ficheiros em vez do SMB, tais como:

- Se estiver a navegar na partilha de ficheiros a partir do PowerShell Cloud Shell (que não pode montar partilhas de ficheiros através de SMB).
- Se precisar de executar um script ou aplicação a partir de um cliente que não pode montar partilhas SMB, como clientes no local que não têm a porta 445 desbloqueada.
- Se estiver a tirar partido de recursos sem servidor, como o serviço [Funções do Azure](../../azure-functions/functions-overview.md). 

Os exemplos seguintes mostram como utilizar o módulo AzureRM PowerShell para manipular a partilha de ficheiros do Azure com o protocolo REST de Ficheiros. 

#### <a name="create-directory"></a>Criar um diretório
Para criar um novo diretório com o nome *myDirectory* na raiz da partilha de ficheiros do Azure, utilize o cmdlet [New-AzureStorageDirectory](/powershell/module/azure.storage/new-azurestoragedirectory).

```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Carregar um ficheiro
Para demonstrar como carregar um ficheiro com o cmdlet [Set-AzureStorageFileContent](/powershell/module/azure.storage/set-azurestoragefilecontent), primeiro é necessário criar um ficheiro na unidade do PowerShell Cloud Shell a carregar. 

Este exemplo coloca a data e hora atuais num novo ficheiro no disco e, em seguida, carrega o ficheiro para a partilha de ficheiros.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzureStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

Se estiver a executar o PowerShell localmente, deve substituir `C:\Users\ContainerAdministrator\CloudDrive\` por um caminho existente no seu computador.

Depois de carregar o ficheiro, pode utilizar o cmdlet [Get-AzureStorageFile](/powershell/module/Azure.Storage/Get-AzureStorageFile) para assegurar que o ficheiro foi carregado para a partilha de ficheiros do Azure. 

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Transferir um ficheiro
Pode utilizar o cmdlet [Get-AzureStorageFileContent](/powershell/module/azure.storage/get-azurestoragefilecontent) para transferir uma cópia do ficheiro que acabou de carregar para a unidade do Cloud Shell.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item 
    `-Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzureStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Depois de transferir o ficheiro, pode utilizar `Get-ChildItem` para ver se o ficheiro foi transferido para a unidade do PowerShell Cloud Shell.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

#### <a name="copy-files"></a>Copiar ficheiros
Uma tarefa comum é copiar ficheiros de uma partilha de ficheiros para outra ou de/para um contentor de armazenamento de Blobs do Azure. Para demonstrar esta funcionalidade, pode criar uma nova partilha e copiar o ficheiro que acabou de carregar para esta nova partilha com o cmdlet [Start-AzureStorageFileCopy](/powershell/module/azure.storage/start-azurestoragefilecopy). 

```azurepowershell-interactive
New-AzureStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzureStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Agora, se listar os ficheiros na nova partilha, deverá ver o ficheiro copiado.

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Embora o cmdlet `Start-AzureStorageFileCopy` seja conveniente para movimentações de ficheiros ad-hoc entre partilhas de ficheiros do Azure e contentores de armazenamento de Blobs do Azure, recomendamos o AzCopy para mover ficheiros maiores (em termos de número ou tamanho dos ficheiros que serão movidos). Saiba mais sobre o [AzCopy para Windows](../common/storage-use-azcopy.md) e [AzCopy para Linux](../common/storage-use-azcopy-linux.md). O AzCopy tem de ser instalado localmente; não está disponível no Cloud Shell. 

## <a name="create-and-manage-share-snapshots"></a>Criar e gerir instantâneos de partilha
Uma tarefa útil adicional que pode fazer com uma partilha de ficheiros do Azure é criar instantâneos de partilha. Um instantâneo preserva um ponto no tempo para uma partilha de ficheiros do Azure. Os instantâneos de partilha são semelhantes às tecnologias de sistema operativo com as quais pode já estar familiarizado, tais como:
- [Serviço de Cópia Sombra de Volumes (VSS)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal) para sistemas de ficheiros Windows, como NTFS e ReFS
- Instantâneos do [Gestor de Volumes Lógicos (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) para sistemas Linux
- Instantâneos do [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) para macOS. 
 Pode criar um instantâneo de partilha para uma partilha com o método `Snapshot` no objeto do PowerShell para uma partilha de ficheiros, obtido com o cmdlet [Get-AzureStorageShare](/powershell/module/azure.storage/get-azurestorageshare). 

```azurepowershell-interactive
$share = Get-AzureStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Procurar instantâneos de partilha
Pode procurar o conteúdo do instantâneo de partilha ao transmitir a referência do instantâneo (`$snapshot`) ao parâmetro `-Share` do cmdlet `Get-AzureStorageFile`.

```azurepowershell-interactive
Get-AzureStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Listar instantâneos de partilha
Pode ver a lista de instantâneos que criou para a sua partilha com o seguinte comando.

```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Restaurar a partir de um instantâneo de partilha
Pode restaurar um ficheiro com o comando `Start-AzureStorageFileCopy` utilizado anteriormente. Para efeitos deste início rápido, vamos eliminar primeiro o ficheiro `SampleUpload.txt` carregado anteriormente para podermos restaurá-lo a partir do instantâneo.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzureStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"
 # Restore SampleUpload.txt from the share snapshot
Start-AzureStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Eliminar um instantâneo de partilha
Pode eliminar um instantâneo de partilha com o cmdlet [Remove-AzureStorageShare](/powershell/module/azure.storage/remove-azurestorageshare)`$snapshot`, com a variável que contém a referência `-Share` ao parâmetro.

```azurepowershell-interactive
Remove-AzureStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Limpar recursos
Quando terminar, pode utilizar o cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos relacionados. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Em alternativa, pode remover recursos um a um:

- Para remover as partilhas de ficheiros do Azure criadas para este início rápido.

    ```azurepowershell-interactive
    Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
        Remove-AzureStorageShare -Context $storageAcct.Context -Name $_.Name
    }
    ```

- Para remover a própria conta de armazenamento (isto removerá implicitamente as partilhas de ficheiros do Azure, bem como quaisquer outros recursos de armazenamento que possa ter criado, como um contentor de armazenamento de Blobs do Azure).

    ```azurepowershell-interactive
    Remove-AzureRmStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
    ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [O que são os Ficheiros do Azure?](storage-files-introduction.md)