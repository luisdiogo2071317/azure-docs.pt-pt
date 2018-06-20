---
title: Implementar a sincronização de ficheiros do Azure (pré-visualização) | Microsoft Docs
description: Saiba como implementar a sincronização de ficheiros do Azure, do início ao fim.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: wgries
ms.openlocfilehash: f1230cbc4d654bfb59bb328ed7d75c6fa76ff10c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268249"
---
# <a name="deploy-azure-file-sync-preview"></a>Implementar a sincronização de ficheiros do Azure (pré-visualização)
Utilize sincronização de ficheiros do Azure (pré-visualização) para centralizar o processamento de partilhas de ficheiros da sua organização nos ficheiros do Azure, mantendo o flexibilidade, o desempenho e a compatibilidade de um servidor de ficheiros no local. Sincronização de ficheiros do Azure transforma do Windows Server para uma cache rápida da Azure da partilha de ficheiros. Pode utilizar qualquer protocolo de que está disponível no Windows Server para aceder aos seus dados localmente, incluindo SMB, NFS e FTPS. Pode ter caches tantos conforme necessário por todo o mundo.

Recomendamos vivamente que leia [planear uma implementação de ficheiros do Azure](storage-files-planning.md) e [planear uma implementação de sincronização de ficheiros do Azure](storage-sync-files-planning.md) antes de concluir os passos descritos neste artigo.

## <a name="prerequisites"></a>Pré-requisitos
* Uma conta de armazenamento do Azure e um ficheiro do Azure partilham na mesma região que pretende implementar a sincronização de ficheiros do Azure. Para obter mais informações, consulte:
    - [Disponibilidade de região](storage-sync-files-planning.md#region-availability) para sincronização de ficheiros do Azure.
    - [Criar uma conta de armazenamento](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para obter uma descrição passo a passo sobre como criar uma conta de armazenamento.
    - [Criar uma partilha de ficheiros](storage-how-to-create-file-share.md) para obter uma descrição passo a passo sobre como criar uma partilha de ficheiros.
* Pelo menos uma instância suportada do Windows Server ou cluster do Windows Server para sincronizar com sincronização de ficheiros do Azure. Para obter mais informações sobre as versões suportadas do Windows Server, consulte [interoperabilidade com o Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).
* Certifique-se de que PowerShell 5.1 está instalado no seu servidor do Windows. Se estiver a utilizar o Windows Server 2012 R2, certifique-se de que está a executar, pelo menos, 5.1 do PowerShell. \*. Pode ignorar esta verificação no Windows Server 2016 com segurança como 5.1 do PowerShell é a predefinição versão out-of-box. No Windows Server 2012 R2, pode verificar que está a executar 5.1 do PowerShell. \* ao observar o valor da **PSVersion** propriedade o **$PSVersionTable** objeto:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Se o valor de PSVersion for inferior a 5.1. \*, como será o caso com a maioria das instalações de raiz do Windows Server 2012 R2, pode facilmente atualizar ao descarregar e instalar [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). O pacote adequado para transferir e instalar para o Windows Server 2012 R2 é **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Sincronização de ficheiros do Azure ainda não suporta PowerShell 6 + no Windows Server 2012 R2 ou Windows Server 2016.
* O módulo do PowerShell de AzureRM nos servidores que pretende utilizar com sincronização de ficheiros do Azure. Para obter mais informações sobre como instalar os módulos do AzureRM PowerShell, consulte [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Sempre Recomendamos que utilize a versão mais recente dos módulos do PowerShell do Azure. 

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Preparar o Windows Server para utilizar com sincronização de ficheiros do Azure
Para cada servidor que pretende utilizar com sincronização de ficheiros do Azure, incluindo a cada nó de servidor num Cluster de ativação pós-falha, desativar **da configuração de segurança avançada do Internet Explorer**. Isto é necessário apenas para o registo de servidor inicial. Pode voltar a ativá-lo Depois do servidor foi registado.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Abra o Gestor de servidor.
2. Clique em **servidor Local**:  
    !["Servidor local" no lado esquerdo da IU do Gestor de servidor](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. No **propriedades** subpane, selecione a ligação **configuração de segurança avançada do IE**.  
    ![O painel de "Configuração avançada do IE segurança" na IU do Gestor de servidor](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. No **da configuração de segurança avançada do Internet Explorer** caixa de diálogo, selecione **desativar** para **administradores** e **utilizadores**:  
    ![A janela de pop da configuração de segurança avançada do Internet Explorer com "Desligado" selecionado](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Para desativar a configuração de segurança avançada do Internet Explorer, execute o seguinte a partir de uma sessão elevada do PowerShell:

```PowerShell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="install-the-azure-file-sync-agent"></a>Instalar o agente de sincronização de ficheiros do Azure
O agente de sincronização de ficheiros do Azure é um pacote transferível, que permite que o Windows Server ser sincronizados com uma partilha de ficheiros do Azure. 

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Pode transferir o agente do [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Quando a transferência estiver concluída, faça duplo clique o pacote MSI para iniciar a instalação do agente de sincronização de ficheiros do Azure.

> [!Important]  
> Se pretender utilizar a sincronização de ficheiros do Azure com um Cluster de ativação pós-falha, o agente de sincronização de ficheiros do Azure tem de estar instalado em cada nó no cluster.

Recomendamos que efetue o seguinte:
- Deixe o caminho de instalação predefinido (c:\Programas\Microsoft Files\Azure\StorageSyncAgent), para simplificar a manutenção de resolução de problemas e de servidor.
- Ative o Microsoft Update manter a sincronização de ficheiros do Azure atualizada. Todas as atualizações, para o agente de sincronização de ficheiros do Azure, incluindo atualizações de funcionalidades e correções, ocorrerem do Microsoft Update. Recomendamos que instale a atualização mais recente para sincronização de ficheiros do Azure. Para obter mais informações, consulte [política de atualização de sincronização de ficheiros do Azure](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Quando a instalação do agente de sincronização de ficheiros do Azure estiver concluída, a IU do registo de servidor abra automaticamente. Tem de ter um serviço de sincronização de armazenamento antes de registrating; consulte a secção seguinte sobre como criar um serviço de sincronização de armazenamento.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Execute o seguinte código de PowerShell para transferir a versão adequada do agente de sincronização de ficheiros do Azure para o SO e instalá-lo no seu sistema.

```PowerShell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="deploy-the-storage-sync-service"></a>Implementar o serviço de sincronização de armazenamento 
A implementação de sincronização de ficheiros do Azure começa por colocar um **o serviço de sincronização de armazenamento** recursos para um grupo de recursos da sua subscrição selecionada. Recomendamos aprovisionamento como poucos destes conforme necessário. Irá criar uma relação de fidedignidade entre os servidores e este recurso e um servidor só pode ser registado para um serviço de sincronização de armazenamento. Como resultado, recomenda-se para implementar serviços de sincronização, tantos armazenamento conforme for necessário separar os grupos de servidores. Tenha em atenção que não consegue sincronizar os servidores a partir de serviços de sincronização de armazenamento diferentes entre si.

> [!Note]
> O serviço de sincronização de armazenamento herdadas as permissões de acesso a subscrição e o grupo de recursos que foi implementada no. Recomendamos que cuidadosamente Verifique quem tem acesso ao mesmo. Entidades com acesso de escrita podem iniciar a sincronização novos conjuntos de ficheiros a partir de servidores registados para este armazenamento serviço de sincronização e fazer com que dados fluam para armazenamento do Azure que seja acessível aos mesmos.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para implementar um serviço de sincronização do Storage, vá para o [portal do Azure](https://portal.azure.com/), clique em *novo* e, em seguida, procure a sincronização de ficheiros do Azure. Nos resultados da pesquisa, selecione **sincronização de ficheiros do Azure (pré-visualização)** e, em seguida, selecione **criar** para abrir o **implementar sincronização de armazenamento** separador.

No painel que se abre, introduza as seguintes informações:

- **Nome**: um nome exclusivo (por subscrição) para o serviço de sincronização de armazenamento.
- **Subscrição**: A subscrição na qual pretende criar o serviço de sincronização de armazenamento. Dependendo da estratégia de configuração da sua organização, poderá ter acesso a uma ou mais subscrições. Uma subscrição do Azure é o contentor mais básico da faturação para cada serviço em nuvem (por exemplo, ficheiros do Azure).
- **Grupo de recursos**: um grupo de recursos é um grupo lógico de recursos do Azure, tal como uma conta de armazenamento ou um serviço de sincronização de armazenamento. Pode criar um novo grupo de recursos ou utilizar um grupo de recursos existente para a sincronização de ficheiros do Azure. (Recomendamos que utilize grupos de recursos como contentores para isolar os recursos logicamente para a sua organização, tais como agrupamento de recursos de RH ou recursos para um projeto específico.)
- **Localização**: A região na qual pretende implementar a sincronização de ficheiros do Azure. Apenas as regiões suportadas estão disponíveis nesta lista.

Quando tiver terminado, selecione **criar** para implementar o serviço de sincronização de armazenamento.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Antes de a interagir com os cmdlets de gestão de sincronização de ficheiros do Azure, terá de importar um DLL e criar um contexto de gestão de sincronização de ficheiros do Azure. Isto é necessário porque os cmdlets de gestão de sincronização de ficheiros do Azure ainda não fazem parte do módulo do AzureRM PowerShell.

> [!Note]  
> O pacote de StorageSync.Management.PowerShell.Cmdlets.dll, que contém os cmdlets de gestão de sincronização de ficheiros do Azure, (intencionalmente) contém um cmdlet com um verbo não aprovada (`Login`). O nome `Login-AzureRmStorageSync` foi escolhido para fazer corresponder o `Login-AzureRmAccount` alias de cmdlet no módulo AzureRM PowerShell. Esta mensagem de erro (e o cmdlet) serão removidos o agente de sincronização de ficheiros do Azure é adicionado ao módulo do AzureRM PowerShell.

```PowerShell
$acctInfo = Login-AzureRmAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzureRmResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Assim que tiver criado o contexto de sincronização de ficheiros do Azure com o `Login-AzureRmStorageSync` cmdlet, pode criar o serviço de sincronização de armazenamento. Não se esqueça de substituir `<my-storage-sync-service>` com o nome do seu serviço de sincronização de armazenamento pretendido.

```PowerShell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Registar o servidor do Windows com o serviço de sincronização de armazenamento
Registar o servidor do Windows com um serviço de sincronização de armazenamento estabelece uma relação de confiança entre o servidor (ou o cluster) e o serviço de sincronização de armazenamento. Um servidor só pode ser registado para um serviço de sincronização de armazenamento e podem sincronizar com outros servidores e do Azure associadas com o mesmo serviço de sincronização de armazenamento de partilhas de ficheiros.

> [!Note]
> Registar o servidor utiliza as credenciais do Azure para criar uma relação de confiança entre o serviço de sincronização de armazenamento e o Windows Server, no entanto, subsequentemente, o servidor cria e utiliza-própria identidade que é válida, desde que o servidor permanece registado e o token de assinatura de acesso partilhado (SAS de armazenamento) atual é válido. Não é possível emitir um novo token SAS para o servidor depois do servidor não está registado, remover, por conseguinte, a capacidade do servidor para aceder as partilhas de ficheiros do Azure, parar qualquer sincronização.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
A IU de registo do servidor deve abrir automaticamente após a instalação do agente de sincronização de ficheiros do Azure. Se não, pode abri-lo manualmente da localização do ficheiro: c:\Programas\Microsoft Files\Azure\StorageSyncAgent\ServerRegistration.exe. Quando abre a IU de registo do servidor, selecione **início de sessão** para começar.

Depois de iniciar sessão, serão apresentadas as seguintes informações:

![Uma captura de ecrã da IU do registo de servidor](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Subscrição do Azure**: A subscrição que contém o serviço de sincronização de armazenamento (consulte [implementar o serviço de sincronização de armazenamento](#deploy-the-storage-sync-service)). 
- **Grupo de recursos**: O grupo de recursos que contém o serviço de sincronização de armazenamento.
- **O serviço de sincronização de armazenamento**: O nome do serviço de sincronização de armazenamento com o qual pretende registar.

Depois de selecionar as informações adequadas, selecione **registar** para concluir o registo do servidor. Como parte do processo de registo, é-lhe pedida uma adicional início de sessão.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Criar um grupo de sincronização e um ponto final da nuvem
Um grupo de sincronização define a topologia de sincronização para um conjunto de ficheiros. Pontos finais dentro de um grupo de sincronização são mantidos sincronizados entre si. Um grupo de sincronização tem de conter ponto final de pelo menos uma nuvem, que representa uma partilha de ficheiros do Azure e um ou pontos finais do servidor. Um ponto final do servidor representa um caminho de servidor registado. Um servidor pode ter pontos finais do servidor em vários grupos de sincronização. Pode criar grupos de sincronização tantos conforme necessário para descrever adequadamente a topologia de sincronização pretendido.

Um ponto final da nuvem é um ponteiro para uma partilha de ficheiros do Azure. Todos os pontos finais de servidor irão sincronizar com um ponto final da nuvem, tornando o ponto final da nuvem do hub. A conta de armazenamento para a partilha de ficheiros do Azure deve estar localizada na mesma região que o serviço de sincronização de armazenamento. A totalidade da partilha de ficheiros do Azure será sincronizada, com uma exceção: pastas especiais, comparável para a pasta "Informações de Volume do sistema" oculta num NTFS volume, serão aprovisionadas. Este diretório é chamado ". SystemShareInformation". Contém os metadados de sincronização importante que não serão sincronizados para outros pontos finais. Não utilize ou eliminá-lo!

> [!Important]  
> Pode efetuar alterações a qualquer ponto final da nuvem ou no grupo de sincronização de ponto final do servidor e os ficheiros foram sincronizadas para os pontos finais no grupo de sincronização. Se fizer uma alteração para o ponto final da nuvem (partilha de ficheiros do Azure) diretamente, alterações primeiro tem de ser detetados por uma tarefa de deteção de alteração de sincronização de ficheiros do Azure. Uma tarefa de deteção de alteração é iniciada, para um ponto final da nuvem, apenas uma vez a cada 24 horas. Para obter mais informações, consulte [ficheiros do Azure perguntas mais frequentes](storage-files-faq.md#afs-change-detection).

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para criar um grupo de sincronização, no [portal do Azure](https://portal.azure.com/), aceda ao seu serviço de sincronização de armazenamento e, em seguida, selecione **+ o grupo de sincronização**:

![Criar um novo grupo de sincronização no portal do Azure](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

No painel de que se abre, introduza as seguintes informações para criar um grupo de sincronização com um ponto final da nuvem:

- **Nome do grupo de sincronização**: O nome do grupo de sincronização para ser criado. Este nome tem de ser exclusivo no âmbito do serviço de sincronização de armazenamento, mas pode ser qualquer nome que seja lógica para si.
- **Subscrição**: A subscrição em que implementou o serviço de sincronização de armazenamento no [implementar o serviço de sincronização de armazenamento](#deploy-the-storage-sync-service).
- **Conta de armazenamento**: Se selecionar **Selecionar conta de armazenamento**, outro painel aparece na qual pode selecionar a conta de armazenamento que tenha a partilha de ficheiros do Azure que pretende sincronizar com.
- **Partilha de ficheiros do Azure**: O nome da partilha de ficheiros do Azure com o qual pretende sincronizar.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Para criar o grupo de sincronização, execute o PowerShell seguinte. Não se esqueça de substituir `<my-sync-group>` com o nome do grupo de sincronização de pretendido.

```PowerShell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Quando o grupo de sincronização tiver sido criado com êxito, pode criar o ponto final da nuvem. Não se esqueça de substituir `<my-storage-account>` e `<my-file-share>` com os valores esperados.

```PowerShell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzureRmStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzureStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Criar um ponto final do servidor
Um ponto final do servidor representa uma localização específica num servidor registado, tais como uma pasta no volume do servidor. Um ponto final do servidor tem de ser um caminho de um servidor registado (em vez de uma partilha montada) e, para utilizar camadas de nuvem, o caminho tem de ser num volume não pertencente ao sistema. Armazenamento ligado à rede (NAS) não é suportado.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para adicionar um ponto final do servidor, vá para o grupo de sincronização recém-criado e, em seguida, selecione **adicionar ponto final do servidor**.

![Adicionar um novo ponto final do servidor no painel do grupo de sincronização](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

No **adicionar ponto final do servidor** painel, introduza as seguintes informações para criar um ponto final do servidor:

- **Servidor registado**: O nome do servidor ou cluster onde pretende criar o ponto final do servidor.
- **Caminho**: caminho do Windows Server para ser sincronizada com êxito como parte do grupo de sincronização.
- **Criação de camadas de nuvem**: um comutador para ativar ou desativar na nuvem em camadas. Com a nuvem em camadas, raramente utilizado ou aceder a ficheiros pode ser colocado em camadas para ficheiros do Azure.
- **Espaço livre no volume**: A quantidade de espaço a reservar em volume no qual o ponto final do servidor está localizado. Por exemplo, se o espaço livre no volume estiver definido como 50%, num volume que tenha um ponto final de servidor único, aproximadamente meio a quantidade de dados está em camadas para ficheiros do Azure. Independentemente de se na nuvem em camadas é ativada, a partilha de ficheiros do Azure tem sempre uma cópia completa dos dados no grupo de sincronização.

Para adicionar o ponto final do servidor, selecione **criar**. Os ficheiros são agora mantidos sincronizados em toda a partilha de ficheiros do Azure e o Windows Server. 

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Execute os seguintes comandos do PowerShell para criar o ponto final do servidor e não se esqueça de substituir `<your-server-endpoint-path>` e `<your-volume-free-space>` com os valores pretendidos.

```PowerShell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Integração com a sincronização de ficheiros do Azure
Os passos recomendados para carregar em sincronização de ficheiros do Azure para o primeiro com período de indisponibilidade de zero preservando a fidelidade de ficheiro completo e a lista de controlo de acesso (ACL) são os seguintes:
 
1. Implemente um serviço de sincronização de armazenamento.
2. Crie um grupo de sincronização.
3. Instale o agente de sincronização de ficheiros do Azure no servidor com o conjunto completo de dados.
4. Registar esse servidor e crie um ponto final do servidor na partilha. 
5. Permita sincronização de fazer o carregamento total à partilha de ficheiros do Azure (ponto final da nuvem).  
6. Depois do carregamento inicial estiver concluído, instale o agente de sincronização de ficheiros do Azure em cada um dos restantes servidores.
7. Crie novas partilhas de ficheiros em cada um dos restantes servidores.
8. Crie pontos finais do servidor em novas partilhas de ficheiros com a política de camadas na nuvem, se assim o desejar. (Este passo necessita de armazenamento adicional fique disponível para a configuração inicial.)
9. Permitir que o agente de sincronização de ficheiros do Azure para efetuar um restauro rápido do espaço de nomes completo sem a transferência de dados real. Após a sincronização completa do espaço de nomes, o motor de sincronização irá preencher o espaço de disco local com base na política de camadas na nuvem para o ponto final do servidor. 
10. Certifique-se a sincronização estiver concluída e testar a sua topologia conforme pretendido. 
11. Redireciona os utilizadores e aplicações para esta partilha de novo.
12. Pode, opcionalmente, elimine quaisquer partilhas duplicadas nos servidores.
 
Se não tiver armazenamento adicional para a integração inicial e pretende anexar pelas partilhas existentes, pode seeding previamente os dados das partilhas de ficheiros do Azure. Esta abordagem é sugerida, se, e apenas se pode aceitar o período de inatividade e garante absolutamente sem alterações de dados em partilhas de servidor durante o processo de integração inicial. 
 
1. Certifique-se de que os dados em qualquer servidor não é possível alterar durante o processo de integração.
2. Pré-seed Azure as partilhas de ficheiros com os dados de servidor utilizando qualquer ferramenta de transferência de dados através de SMB, por exemplo, Robocopy, cópia SMB direta. Uma vez que o AzCopy não carregar os dados através de SMB pelo que não pode ser utilizado para o seeding previamente.
3. Crie a topologia de sincronização de ficheiros do Azure com os pontos finais de servidor pretendido apontar para as partilhas existentes.
4. Permita sincronização de concluir o processo de reconciliação em todos os pontos finais. 
5. Depois de concluída a reconciliação, pode abrir partilhas para as alterações.
 
. Lembre-se de que, atualmente, o seeding previamente abordagem tem algumas limitações- 
- Não é preservada fidelidade completa em ficheiros. Por exemplo, ficheiros percam ACLs e carimbos.
- Alterações de dados no servidor antes de topologia de sincronização está totalmente operacional e em execução podem causar conflitos em pontos finais do servidor.  
- Depois de criar o ponto final da nuvem, sincronização de ficheiros do Azure é executado um processo para detetar os ficheiros na nuvem antes de iniciar a sincronização inicial. O tempo necessário para concluir este processo varia consoante vários fatores, como a velocidade da rede, a largura de banda disponível e o número de ficheiros e pastas. Para a estimativa aproximada na versão de pré-visualização, o processo de deteção é executada aproximadamente a cada 10 ficheiros/seg.  Por conseguinte, mesmo que o seeding previamente o executa rápido, o tempo geral para obter um sistema totalmente em execução pode ser significativamente maior, quando os dados pré-propagados na nuvem.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Migrar uma implementação de replicação de DFS (DFS-R) a sincronização de ficheiros do Azure
Para migrar uma implementação de DFS-R para sincronização de ficheiros do Azure:

1. Crie um grupo de sincronização para representar a topologia de DFS-R, que estará a substituir.
2. Inicie no servidor que tenha o conjunto completo de dados na sua topologia de DFS-R para migrar. Instale a sincronização de ficheiros do Azure nesse servidor.
3. Registar esse servidor e criar um ponto final do servidor para o servidor primeiro a ser migrada. Não ative a nuvem em camadas.
4. Permitir que todos os a sincronização de dados para a partilha de ficheiros do Azure (ponto final da nuvem).
5. Instalar e registar o agente de sincronização de ficheiros do Azure em cada um dos servidores DFS-R restantes.
6. Desativar DFS-R. 
7. Crie um ponto final do servidor em cada um dos servidores DFS-R. Não ative a nuvem em camadas.
8. Certifique-se a sincronização estiver concluída e testar a sua topologia conforme pretendido.
9. Extinguir DFS-R.
10. Na nuvem em camadas pode agora ser ativada em qualquer ponto final do servidor conforme pretendido.

Para obter mais informações, consulte [interoperabilidade de sincronização de ficheiros do Azure com o sistema de ficheiros distribuído (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Passos Seguintes
- [Adicionar ou remover um Azure sincronização do ponto final do ficheiro](storage-sync-files-server-endpoint.md)
- [Registar ou anular o registo de um servidor com sincronização de ficheiros do Azure](storage-sync-files-server-registration.md)