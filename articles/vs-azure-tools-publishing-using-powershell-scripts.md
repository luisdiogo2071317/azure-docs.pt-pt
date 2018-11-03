---
title: Utilizar Scripts do Windows PowerShell para publicar no desenvolvimento e ambientes de teste | Documentos da Microsoft
description: Saiba como utilizar scripts do Windows PowerShell a partir do Visual Studio para publicar no desenvolvimento e os ambientes de teste.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: 99d723eee6bd5b60289af5490e4b1cd6a855cabb
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969511"
---
# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Utilizar scripts do Windows PowerShell para publicar em ambientes de desenvolvimento e teste

Quando cria uma aplicação web no Visual Studio, pode gerar um script do Windows PowerShell que pode utilizar mais tarde para automatizar a publicação do seu Web site para o Azure como uma aplicação Web no App Service do Azure ou uma máquina virtual. Pode editar e expandir o script do Windows PowerShell no editor do Visual Studio à medida das suas necessidades ou integrar o script com existentes de compilação, teste e scripts de publicação.

Utilizar estes scripts, pode aprovisionar versões personalizadas (também conhecido como ambientes de desenvolvimento e teste) do seu site para utilização temporária. Por exemplo, pode configurar uma versão específica do seu Web site numa máquina virtual do Azure ou no bloco de teste num Web site para executar um pacote de testes, reproduzir um bug, testar uma correção de erro, a versão de avaliação de uma alteração proposta ou configurar um ambiente personalizado para uma demonstração ou apresentação. Depois de criar um script que publica o seu projeto, pode recriar ambientes idênticos executando novamente o script conforme necessário, ou executar o script com a sua própria compilação da sua aplicação web para criar um ambiente personalizado para fins de teste.

## <a name="prerequisites"></a>Pré-requisitos

* Azure SDK 2.3 ou posterior. Ver [Downloads do Visual Studio](http://go.microsoft.com/fwlink/?LinkID=624384). (Não é necessário o Azure SDK para gerar scripts para projetos web. Esta funcionalidade é para os projetos da web, funções da web não nos serviços cloud.)
* O Azure PowerShell 0.7.4 ou posterior. Consulte [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
* [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) ou posterior.

## <a name="additional-tools"></a>Ferramentas adicionais

Ferramentas adicionais e recursos para trabalhar com o PowerShell no Visual Studio para desenvolvimento do Azure estão disponíveis. Ver [ferramentas do PowerShell para o Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Gerar os scripts de publicação

Pode gerar os scripts de publicação para uma máquina virtual que aloja o seu Web site ao criar um novo projeto seguindo [estas instruções](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Também pode [gerar publicar scripts para aplicações web no App Service do Azure](app-service/scripts/app-service-powershell-deploy-github.md).

## <a name="scripts-that-visual-studio-generates"></a>Scripts de que o Visual Studio gera

Visual Studio gera uma pasta de nível de solução chamada **PublishScripts** que contém dois arquivos do Windows PowerShell, um script de publicação para a sua máquina virtual ou Web site e um módulo que contém as funções que pode utilizar o scripts. Visual Studio também gera um ficheiro no formato JSON que especifica os detalhes do projeto que está a implementar.

### <a name="windows-powershell-publish-script"></a>Script de publicação do Windows PowerShell

O script de publicar contém passos de publicação específica para implementar um Web site ou a máquina virtual. O Visual Studio fornece a sintaxe de cores para o desenvolvimento do Windows PowerShell. Ajuda para as funções está disponível e pode editar livremente as funções no script para se adequar às suas necessidades em constante mudança.

### <a name="windows-powershell-module"></a>Módulo do Windows PowerShell

O módulo do Windows PowerShell que o Visual Studio gera contém funções que utiliza o script de publicar. Estas funções do Azure PowerShell não se destinam a ser modificada. Consulte [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

### <a name="json-configuration-file"></a>Ficheiro de configuração JSON

O ficheiro JSON é criado na **configurações** pasta e contém dados de configuração que especifica exatamente quais os recursos para implementar no Azure. O nome do ficheiro que o Visual Studio gera é o projeto-nome-WAWS-dev.json se tiver criado um Web site ou projeto nome-VM-dev.json se tiver criado uma máquina virtual. Eis um exemplo de um ficheiro de configuração JSON que é gerado quando cria um Web site. A maioria dos valores são auto-explicativos. O nome do site é gerado pelo Azure, para que ele pode não corresponde ao nome do projeto.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```

Quando cria uma máquina virtual, o ficheiro de configuração do JSON é semelhante ao seguinte. Um serviço em nuvem é criado como um contêiner para a máquina virtual. A máquina virtual contém os pontos de extremidade comuns para o acesso web via HTTP e HTTPS, bem como pontos finais para o Web Deploy, que lhe permita publicar para o Web site a partir do seu computador local, o ambiente de trabalho remoto e o Windows PowerShell.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Pode editar a configuração do JSON para alterar o que acontece quando executa os scripts de publicação. O `cloudService` e `virtualMachine` secções são necessárias, mas pode eliminar o `databases` secção se não precisa dela. As propriedades que estão vazias no arquivo de configuração padrão que o Visual Studio gera são opcionais; essas propriedades que têm valores no ficheiro de configuração padrão são necessárias.

Se tiver um Web site que tem vários ambientes de implantação (conhecidos como ranhuras) em vez de um site de produção única no Azure, pode incluir o nome da ranhura no nome do Web site no ficheiro de configuração JSON. Por exemplo, se tiver um Web site com o nome **mysite** e uma entrada para ele, chamado **testar** , em seguida, o URI é `mysite-test.cloudapp.net`, mas o nome correto a utilizar no ficheiro de configuração é mysite(test). Só pode fazer este se o Web site e ranhuras já existem na sua subscrição. Caso não existam, criar o Web site ao executar o script sem especificar a ranhura, em seguida, criar a ranhura no [portal do Azure](https://portal.azure.com/), e posteriormente, execute o script com o nome do Web site modificado. Para obter mais informações sobre os blocos de implementação para aplicações web, consulte [configurar ambientes de teste para aplicações web no App Service do Azure](app-service/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Como executar os scripts de publicação

Se nunca executar um script do Windows PowerShell antes de, primeiro tem de definir a política de execução para permitir a execução de scripts. A política é uma funcionalidade de segurança para impedir que os utilizadores a executar scripts do Windows PowerShell, se forem vulneráveis a malware ou vírus que envolvem a execução de scripts.

### <a name="run-the-script"></a>Executar o script

1. Crie o pacote do Web Deploy para seu projeto. Um pacote de Web Deploy é um arquivo compactado (ficheiro. zip) que contêm os ficheiros que pretende copiar para o seu site ou a máquina virtual. Pode criar pacotes Web Deploy no Visual Studio para qualquer aplicativo da web.

![Criar Web implementar pacote](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Para obter mais informações, consulte [como: criar um pacote de implementação Web no Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Também pode automatizar a criação do pacote Web Deploy, conforme descrito em [personalização e extensão scripts[(#customizing-and-extending-publish-scripts) a publicar]

1. Na **Explorador de soluções**, abra o menu de contexto para o script e, em seguida, escolha **abre-se com o ISE do PowerShell**.
1. Se executar scripts do Windows PowerShell neste computador pela primeira vez, abra uma janela de linha de comandos com privilégios de administrador e escreva o seguinte comando:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

1. Inicie sessão no Azure utilizando o seguinte comando.

    ```powershell
    Add-AzureAccount
    ```

Quando lhe for pedido, forneça o nome de utilizador e palavra-passe.

Tenha em atenção que, quando é automatizar o script, este método de fornecer credenciais do Azure não funciona. Em vez disso, deve usar o `.publishsettings` ficheiro para fornecer credenciais. Uma vez apenas, que utiliza o comando **Get-AzurePublishSettingsFile** para transferir o ficheiro do Azure e, posteriormente, utilizar **importação AzurePublishSettingsFile** para importar o ficheiro. Para obter informações detalhadas, veja [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

1. (Opcional) Se quiser criar recursos do Azure como a máquina virtual, a base de dados e o Web site sem a publicação da sua aplicação web, utilizam o **Publish-WebApplication.ps1** comando com o **-configuração**argumento definido para o ficheiro de configuração JSON. Esta linha de comandos utiliza o ficheiro de configuração do JSON para determinar quais os recursos para criar. Uma vez que utiliza as predefinições para outros argumentos da linha de comandos, ele cria os recursos, mas não publicar a sua aplicação web. – Verboso opção dá-lhe obter mais informações sobre o que está acontecendo.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

1. Utilize o **Publish-WebApplication.ps1** comando, como mostra a um dos exemplos seguintes para invocar o script e publicar a sua aplicação web. Se precisar de substituir as predefinições para qualquer um dos outros argumentos, por exemplo, o nome da subscrição, publicar o nome do pacote, as credenciais da máquina virtual ou as credenciais do servidor de base de dados, pode especificar esses parâmetros. Utilize o **– verboso** opção para ver mais informações sobre o progresso do processo de publicação.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

Se estiver a criar uma máquina virtual, o comando é semelhante ao seguinte. Este exemplo também mostra como especificar as credenciais para várias bases de dados. Para as máquinas virtuais que criam esses scripts, o certificado SSL não é de uma autoridade de raiz fidedigna. Por conseguinte, tem de utilizar o **– AllowUntrusted** opção.

```powershell
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

O script pode criar bases de dados, mas ele não criar servidores de base de dados. Se quiser criar um servidor de base de dados, pode utilizar o **New-AzureSqlDatabaseServer** função no módulo do Azure.

## <a name="customizing-and-extending-the-publish-scripts"></a>Personalizar e expandir os scripts de publicação

Pode personalizar o script de publicação e o ficheiro de configuração JSON. As funções no módulo Windows PowerShell **AzureWebAppPublishModule.psm1** não se destinam a ser modificado. Se quiser apenas especificar uma base de dados diferente ou alterar algumas das propriedades da máquina virtual, edite o ficheiro de configuração JSON. Se quiser estender a funcionalidade do script para automatizar a criação e teste o seu projeto, pode implementar os stubs de função na **Publish-WebApplication.ps1**.

Para automatizar a criação de seu projeto, adicione código que chama o MSBuild para `New-WebDeployPackage` conforme mostrado neste exemplo de código. O caminho para o comando do MSBuild é diferente consoante a versão do Visual Studio que instalou. Para obter o caminho correto, pode usar a função **Get-MSBuildCmd**, conforme mostrado neste exemplo.

### <a name="to-automate-building-your-project"></a>Para automatizar a criação de seu projeto

1. Adicionar o `$ProjectFile` parâmetro na secção global param.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

1. Copiar a função `Get-MSBuildCmd` em seu arquivo de script.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

1. Substitua `New-WebDeployPackage` com o código seguinte e substitua os marcadores de posição na construção da linha `$msbuildCmd`. Esse código é para o Visual Studio 2017. Se estiver a utilizar o Visual Studio 2015, altere a **VisualStudioVersion** propriedade `14.0` (`12.0` para Visual Studio 2013).

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function to build and package your web application
    ```

Para criar seu aplicativo web, utilize MsBuild.exe. Para obter ajuda, consulte a referência de linha de comando do MSBuild em: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

```powershell
Write-VerboseWithTime 'Build-WebDeployPackage: Start'

$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=15.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
```

### <a name="start-execution-of-the-build-command"></a>Iniciar a execução do comando de compilação

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Chamar o `New-WebDeployPackage` função antes desta linha: `$Config = Read-ConfigFile $Configuration` para aplicações web ou `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` para máquinas virtuais.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

1. Invocar o script personalizado da linha de comando com o passar do `$Project` argumento, tal como o exemplo seguinte:

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

Para automatizar os testes da sua aplicação, adicione código ao `Test-WebApplication`. Certifique-se de que as linhas de anule os comentários **Publish-WebApplication.ps1** onde essas funções forem chamadas. Se não fornecer uma implementação, pode criar manualmente o seu projeto com o Visual Studio e, em seguida, execute o script de publish para publicar no Azure.

## <a name="publishing-function-summary"></a>Resumida de função de publicação

Para obter ajuda para funções que pode utilizar a linha de comandos do Windows PowerShell, utilize o comando `Get-Help function-name`. A ajuda inclui a ajuda de parâmetro e exemplos. O mesmo texto de ajuda também está a ser os arquivos de origem do script **AzureWebAppPublishModule.psm1** e **Publish-WebApplication.ps1**. O script e ajuda está localizada na sua linguagem do Visual Studio.

**AzureWebAppPublishModule**

| Nome da função | Descrição |
| --- | --- |
| Add-AzureSQLDatabase |Cria uma nova base de dados SQL do Azure. |
| Add-AzureSQLDatabases |Cria bases de dados SQL do Azure entre os valores no ficheiro de configuração JSON que o Visual Studio gera. |
| Add-AzureVM |Cria uma máquina virtual do Azure e devolve o URL da VM implementada. A função configura os pré-requisitos e, em seguida, chama o **New-AzureVM** função (módulo do Azure) para criar uma nova máquina virtual. |
| Add-AzureVMEndpoints |Adiciona pontos finais de entrada novo a uma máquina virtual e retorna a máquina virtual com o novo ponto final. |
| Add-AzureVMStorage |Cria uma nova conta de armazenamento do Azure na subscrição atual. O nome da conta começa com "devtest", seguido de uma cadeia exclusiva de alfanumérica. A função devolve o nome da nova conta de armazenamento. Especifique uma localização ou um grupo de afinidade para a nova conta de armazenamento. |
| Add-AzureWebsite |Cria um Web site com o nome especificado e a localização. Essa função chama o **New-AzureWebsite** função no módulo do Azure. Se a subscrição já não inclui um Web site com o nome especificado, essa função cria o Web site e retorna um objeto de Web site. Caso contrário, ele retorna `$null`. |
| Subscrição de cópia de segurança |Guarda a subscrição do Azure atual no `$Script:originalSubscription` variável no âmbito de script. Esta função guarda a subscrição do Azure atual (tal como obtidas pelo `Get-AzureSubscription -Current`) e sua conta de armazenamento e a subscrição que é alterada por este script (armazenado na variável `$UserSpecifiedSubscription`) e a respetiva conta de armazenamento, no âmbito de script. Ao salvar os valores, pode utilizar uma função, como `Restore-Subscription`, para restaurar a conta de armazenamento e de subscrição atual original para o status atual, se tiver alterado o estado atual. |
| Find-AzureVM |Obtém a máquina virtual do Azure especificada. |
| Format-DevTestMessageWithTime |Acrescenta a data e hora a uma mensagem. Esta função foi concebida para mensagens escritas para os fluxos de erro e Verbose. |
| Get-AzureSQLDatabaseConnectionString |Monta uma cadeia de ligação para ligar a uma base de dados SQL do Azure. |
| Get-AzureVMStorage |Devolve o nome da primeira conta de armazenamento com o padrão de nome "Dev/Test *" (maiúsculas de minúsculas) na localização especificada ou grupo de afinidade. Se o "Dev/Test*" conta de armazenamento não corresponde a localização ou grupo de afinidade, a função ignora-lo. Especifique uma localização ou um grupo de afinidades. |
| Get-MSDeployCmd |Devolve um comando para executar a ferramenta de MsDeploy.exe. |
| New-AzureVMEnvironment |Encontrar ou cria uma máquina virtual na subscrição que correspondam aos valores no ficheiro de configuração JSON. |
| Publish-WebPackage |Utiliza MsDeploy.exe e uma web publicar o pacote. Ficheiro zip para implementar recursos para um Web site. Esta função não gera quaisquer dados. Se a chamada para MSDeploy.exe falhar, a função emite uma exceção. Para obter uma saída mais detalhada, utilize o **-Verbose** opção. |
| Publish-WebPackageToVM |Verifica se os valores de parâmetros e, em seguida, chama o **Publish-WebPackage** função. |
| Read-ConfigFile |Valida o ficheiro de configuração JSON e devolve uma tabela de hash de valores selecionados. |
| Restore-Subscription |Repõe a subscrição atual para a subscrição original. |
| Test-AzureModule |Devolve `$true` se a versão do módulo do Azure instalado é 0.7.4 ou posterior. Devolve `$false` se o módulo não está instalado ou é uma versão anterior. Esta função não tem parâmetros. |
| Test-AzureModuleVersion |Devolve `$true` se a versão do módulo do Azure é 0.7.4 ou posterior. Devolve `$false` se o módulo não está instalado ou é uma versão anterior. Esta função não tem parâmetros. |
| Test-HttpsUrl |Converte o URL de entrada para um objeto System. Devolve `$True` se o URL é absoluto e seu esquema de https. Devolve `$false` se o URL é relativo, seu esquema não é HTTPS ou a cadeia de entrada não pode ser convertida para um URL. |
| Membro de teste |Devolve `$true` se uma propriedade ou método for um membro do objeto. Caso contrário, devolve `$false`. |
| Escrita ErrorWithTime |Escreve uma mensagem de erro prefixada com a hora atual. Essa função chama o **formato DevTestMessageWithTime** função preceder o tempo antes de gravar a mensagem no fluxo de erro. |
| Write-HostWithTime |Escreve uma mensagem para o programa de anfitrião (**Write-Host**) como prefixo com a hora atual. O efeito de escrever para o programa anfitrião varia. A maioria dos programas que alojam Windows PowerShell gravar estas mensagens de saída padrão. |
| Write-VerboseWithTime |Escreve uma mensagem verbosa prefixada com a hora atual. Uma vez que ele chama **Write-Verbose**, a mensagem apresentada apenas quando o script é executado com o **verboso** parâmetro ou quando o **VerbosePreference** preferência estiver definida como  **Continuar**. |

**Publish-WebApplication**

| Nome da função | Descrição |
| --- | --- |
| New-AzureWebApplicationEnvironment |Cria os recursos do Azure, como um Web site ou a máquina virtual. |
| New-WebDeployPackage |Esta função não está implementada. Pode adicionar os comandos nesta função para criar o seu projeto. |
| Publish-AzureWebApplication |Publica uma aplicação web no Azure. |
| Publish-WebApplication |Cria e implementa aplicações Web, máquinas virtuais, bases de dados SQL e contas de armazenamento para um projeto de web do Visual Studio. |
| Test-WebApplication |Esta função não está implementada. Pode adicionar os comandos nesta função para testar a aplicação. |

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre o script do PowerShell lendo [Scripting com o Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) e ver outros scripts do PowerShell do Azure no [Script Center](https://azure.microsoft.com/documentation/scripts/).
