---
title: "PowerShell no início rápido do Azure na nuvem Shell (pré-visualização) | Microsoft Docs"
description: "Início rápido para o PowerShell na Shell de nuvem"
services: Azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: damaerte
ms.openlocfilehash: b454720dd5bd2df036a400c8bfc1c383de5af542
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell-preview"></a>Início rápido para o PowerShell na Shell de nuvem do Azure (pré-visualização)

Este documento fornece detalhes sobre como utilizar o PowerShell na nuvem Shell no [portal do Azure](https://aka.ms/PSCloudPreview).

> [!NOTE]
> A [Bash na Shell de nuvem do Azure](quickstart.md) início rápido também está disponível.

## <a name="start-cloud-shell"></a>Iniciar a Shell de nuvem

1. Clique em **nuvem Shell** botão da barra de navegação superior do portal do Azure

  ![](media/quickstart-powershell/shell-icon.png)

2. Selecione o ambiente de PowerShell a partir da lista pendente e será na unidade do Azure`(Azure:)`

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>Execute comandos do PowerShell

Execute comandos do PowerShell regulares na Shell do Cloud, como:

```Powershell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Navegue de recursos do Azure

 1. Lista as suas subscrições

    ``` Powershell
    PS Azure:\> dir
    ```

 2. `cd`a sua subscrição preferencial

    ``` Powershell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Ver todos os seus recursos do Azure sob a subscrição atual
 
    Tipo `dir` para listar várias vistas dos seus recursos Azure.
 
    ``` PowerShell
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
     ```

### <a name="allresources-view"></a>Vista de AllResources 
Tipo `dir` em `AllResources` diretório para ver os recursos do Azure.
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>Explorar os grupos de recursos

 Pode ir para o `ResourceGroups` diretório e dentro de um grupo de recursos específico pode encontrar as máquinas virtuais.

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> Poderá reparar que na segunda vez quando escrever `dir`, a shell de nuvem é capaz de apresentar os itens muito mais rápidos.
> Isto acontece porque os itens subordinados são colocadas em cache na memória para uma melhor experiência de utilizador.
No entanto, pode utilizar sempre `dir -Force` obter novos dados.

### <a name="navigate-storage-resources"></a>Navegue de recursos de armazenamento
    
Ao introduzir no `StorageAccounts` pasta pode navegar facilmente os recursos de armazenamento
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

Com a cadeia de ligação, pode utilizar o seguinte comando para montar a partilha de ficheiros do Azure.
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

Para obter mais informações, consulte [montar uma partilha de ficheiros do Azure e aceder à partilha do Windows][azmount].

Também pode navegar os diretórios sob a partilha de ficheiros do Azure da seguinte forma:

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>Interagir com máquinas virtuais

Pode encontrar todas as suas máquinas virtuais sob a subscrição atual através de `VirtualMachines` diretório.
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Invocar o script do PowerShell entre VMs remotas

 > [!WARNING]
 > Consulte [resolução de problemas de gestão remota de VMs do Azure](troubleshooting.md#powershell-resolutions).

  Partindo do princípio que tem uma VM, MyVM1, vamos utilizar `Invoke-AzureRmVMCommand` para invocar um scriptblock PowerShell no computador remoto.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  Também pode navegar para o diretório de virtualMachines pela primeira vez e executar `Invoke-AzureRmVMCommand` da seguinte forma.

  ``` Powershell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  Poderá ver um resultado semelhante ao seguinte:

  ``` Powershell
  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Interativamente iniciar sessão a uma VM remota

Pode utilizar `Enter-AzureRmVM` interativamente iniciar sessão uma VM em execução no Azure.

  ``` Powershell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Também pode navegar para o `virtualMachines` diretório primeiro e execute `Enter-AzureRmVM` da seguinte forma

  ``` Powershell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>Detetar WebApps

Ao introduzir no `WebApps` pasta pode navegar facilmente os recursos de aplicações web

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

```

## <a name="ssh"></a>SSH

[O Win32 OpenSSH](https://github.com/PowerShell/Win32-OpenSSH) está disponível no CloudShell do PowerShell.
Para autenticar para servidores ou VMs com SSH, gerar o par de chaves públicas-privadas no CloudShell e publicar a chave pública para `authorized_keys` no computador remoto, tais como `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> Pode criar as chaves de privada público SSH utilizando `ssh-keygen` e publicá-los para `$env:USERPROFILE\.ssh` no CloudShell.

### <a name="using-a-custom-profile-to-persist-git-and-ssh-settings"></a>Utilizar um perfil personalizado para manter as definições do GIT e SSH

Uma vez que as sessões não são mantidas após a fim de sessão, guarde o `$env:USERPROFILE\.ssh` pasta para `CloudDrive` ou crie um symlink quando CloudShell obtém iniciada.
Adicione o seguinte código no seu profile.ps1 para criar um symlink para CloudDrive abreviado.

``` Powershell
# Check if the ssh folder exists
if( -not (Test-Path $home\CloudDrive\.ssh){
    mkdir $home\CloudDrive\.ssh
}

# .ssh path relative to this script
$script:sshFolderPath = Join-Path $PSScriptRoot .ssh

# Create a symlink to .ssh in user's $home
if(Test-Path $script:sshFolderPath){
   if(-not (Test-Path (Join-Path $HOME .ssh ))){
        New-Item -ItemType SymbolicLink -Path $HOME -Name .ssh -Value $script:sshFolderPath
   }
}

```

### <a name="using-ssh"></a>Utilização do SSH

Siga as instruções [aqui](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) para criar uma nova configuração de VM com os Cmdlets de AzureRM.
Chamar anteriormente para `New-AzureRMVM` para reunião inicial-desativar a implementação, adicione a chave pública SSH para a configuração de VM.
A VM recentemente criada irá conter a chave pública no `~\.ssh\authorized_keys` localização, ativando assim livre de credencial ssh sessão para a VM.

``` Powershell

# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH Keys in CloudShell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzureRmVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# ssh to the VM
ssh azureuser@MyVM.Domain.Com

```


## <a name="list-available-commands"></a>Lista de comandos disponíveis

Em `Azure` unidade, escreva `Get-AzureRmCommand` obter específico do contexto de comandos do Azure.

Em alternativa, pode utilizar sempre `Get-Command *azurerm* -Module AzureRM.*` para descobrir os comandos do Azure disponíveis.

## <a name="install-custom-modules"></a>Instalar módulos personalizados

Pode executar `Install-Module` para instalar os módulos do [galeria do PowerShell][gallery].

## <a name="get-help"></a>Get-Help

Tipo `Get-Help` para obter informações sobre o PowerShell na Shell de nuvem do Azure.

``` Powershell
PS Azure:\> Get-Help
```

Para um comando específico, ainda pode fazer Get-Help seguido de um cmdlet.

``` Powershell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Utilize ficheiros do Azure para armazenar os dados

Pode criar um script, diga `helloworld.ps1`e guardá-lo para o `CloudDrive` para utilizá-lo entre sessões de shell.

``` Powershell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Hora seguinte quando utilizar o PowerShell na Shell de nuvem, o `helloworld.ps1` ficheiro existirá sob o `CloudDrive` pasta monta a partilha de ficheiros do Azure.

## <a name="use-custom-profile"></a>Utilizar perfil personalizado

Pode personalizar o seu ambiente de PowerShell, criando PowerShell perfis - `profile.ps1` ou `Microsoft.PowerShell_profile.ps1`. Guardá-lo sob o `CloudDrive` para que podem ser carregada em cada sessão do PowerShell quando iniciar a Shell de nuvem.

Para saber como criar um perfil, consulte [sobre perfis][profile].

## <a name="use-git"></a>Utilize o Git

Para clonar um repositório de git na Shell de nuvem, terá de criar um [token de acesso pessoal] [ githubtoken] e utilizá-lo como o nome de utilizador. Assim que tiver o token, clone o repositório da seguinte forma:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Uma vez que as sessões na Shell de nuvem não são mantidas ao terminar sessão ou a sessão expirar, o ficheiro de configuração do Git não existirá após o início de sessão seguinte. Para que a configuração de Git persistirem, tem de guardar o seu .gitconfig para sua `CloudDrive` e copiá-lo ou criar um symlink quando obtém iniciada a Shell de nuvem. Utilize o seguinte fragmento de código no seu profile.ps1, para criar um symlink para `CloudDrive`.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $Home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>Sair da shell

Tipo `exit` para terminar a sessão.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
