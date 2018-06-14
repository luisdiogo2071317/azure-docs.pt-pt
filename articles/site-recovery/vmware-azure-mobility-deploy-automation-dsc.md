---
title: Implementar o serviço de mobilidade de recuperação de Site com o Automation DSC do Azure | Microsoft Docs
description: Descreve como utilizar o DSC de automatização do Azure para implementar automaticamente o serviço de mobilidade de recuperação de Site do Azure e o agente do Azure para a VM de VMware e replicação do servidor físico para o Azure
services: site-recovery
author: krnese
manager: lorenr
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: krnese
ms.openlocfilehash: 0bf1b551ba578cd152201c131bd60470bdc9d86a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
ms.locfileid: "29812853"
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc"></a>Implementar o serviço de Mobilidade com o Automation DSC do Azure

Implementar o serviço de mobilidade em VMs de VMware e servidores físicos que pretende replicar para o Azure, utilizando [do Azure Site Recovery](site-recovery-overview.md).

Este artigo descreve como implementar o serviço em computadores Windows, utilizando o Automation DSC do Azure pretendido Estado Configuration (), para se certificar de que:


## <a name="prerequisites"></a>Pré-requisitos

* Um repositório para armazenar o programa de configuração necessário
* Um repositório para armazenar o frase de acesso necessária para registar o servidor de gestão. Uma frase de acesso exclusivo é gerado para um servidor de configuração específicos. 
* Windows Management Framework (WMF) 5.0 deve ser instalado nas máquinas que pretende ativar a proteção. Este é um requisito de DSC de automatização.
Se pretender utilizar máquinas de DSC para Windows que tenham o WMF 4.0 instalado, consulte [utilizar DSC em ambientes desligados](## Use DSC in disconnected environments).
 

## <a name="extract-binaries"></a>Extraia os binários

O serviço de mobilidade pode ser instalado através da linha de comandos e aceita vários argumentos. Terá de obter os binários (depois de as extrair do seu programa de configuração) e armazená-las num local onde pode obtê-los utilizando uma configuração de DSC.

1. Para extrair os ficheiros que sejam necessárias para esta configuração, navegue até ao diretório seguinte no servidor de gestão: **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**
2. Nesta pasta, verifique se existe um ficheiro MSI designado: **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**.
3. Utilize o seguinte comando para extrair o instalador: **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Selecione todos os ficheiros e enviá-los para uma pasta comprimida (zipped).

Tem agora os binários que precisa para automatizar a configuração do serviço de mobilidade utilizando o DSC de automatização.

## <a name="store-the-passphrase"></a>Armazenar o frase de acesso

É necessário determinar onde pretende colocar esta pasta zipped. Pode utilizar uma conta de armazenamento do Azure, conforme mostrado posteriormente, para armazenar o frase de acesso que terá para o programa de configuração. O agente, em seguida, vai registar com o servidor de gestão como parte do processo.

1. Guarde a frase de acesso que recebeu quando implementou o servidor de configuratio num ficheiro de texto - passphrase.txt.
2. Coloque a pasta zipped e o frase de acesso num contentor dedicado na conta do storage do Azure.


Se preferir manter estes ficheiros numa partilha na sua rede, pode fazê. Terá de garantir que os recursos de DSC que irá utilizar mais tarde tem acesso e podem obter a configuração e o frase de acesso.

## <a name="create-the-dsc-configuration"></a>Criar a configuração de DSC

O programa de configuração depende WMF 5.0. Para a máquina aplicar com êxito a configuração através do DSC da automatização, WMF 5.0 tem de estar presente.

O ambiente utiliza a configuração de DSC de exemplo seguinte:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
A configuração será efetuado o seguinte:

* As variáveis informará a configuração onde obter os binários para o serviço de mobilidade e o agente de VM do Azure, onde obter o frase de acesso e onde pretende guardar a saída.
* A configuração irá importar o recurso de xPSDesiredStateConfiguration DSC para que possa utilizar `xRemoteFile` para transferir os ficheiros do repositório.
* A configuração irá criar um diretório onde pretende armazenar os binários.
* O recurso de arquivo será extraia os ficheiros da pasta zipped.
* O pacote de recursos de instalação irá instalar o serviço de mobilidade do UNIFIEDAGENT. Instalador. EXE com os argumentos específicos. (As variáveis que construir os argumentos tem de ser alterada para refletir o seu ambiente.)
* O pacote de recursos de AzureAgent irá instalar o agente de VM do Azure, recomenda-se em cada VM que é executada no Azure. O agente da VM do Azure também possibilita adicionar extensões para a VM após a ativação pós-falha.
* Os recursos ou recurso de serviço irão garantir que os serviços de mobilidade relacionados e os serviços do Azure estão sempre em execução.

Guardar a configuração como **ASRMobilityService**.

> [!NOTE]
> Não se esqueça de substituir o CSIP na configuração para refletir o servidor de gestão real, para que o agente será ligado corretamente e utilizará o frase de acesso correto.
>
>

## <a name="upload-to-automation-dsc"></a>Carregar no Automation DSC
Porque a configuração de DSC que efetuou irá importar um módulo de recursos de DSC necessário (xPSDesiredStateConfiguration), terá de importar este módulo de automatização antes de carregar a configuração de DSC.

1. Inicie sessão na sua conta de automatização, navegue até à **ativos** > **módulos**e clique em **procurar galeria**.
2. Para o módulo de pesquisa e importe-o à sua conta.
3. Quando concluir isto, aceda ao seu computador em que tenha os módulos do Azure Resource Manager instalados e continue para importar a configuração de DSC recentemente criada.

## <a name="import-cmdlets"></a>Cmdlets de importação

1. No PowerShell, inicie sessão sua subscrição do Azure.
2. Modificar os cmdlets para refletir o seu ambiente e capturar as informações da conta de automatização numa variável:

    ```powershell
    $AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
    ```

3. Carregar a configuração para o DSC de automatização utilizando o cmdlet seguinte:

    ```powershell
    $ImportArgs = @{
        SourcePath = 'C:\ASR\ASRMobilityService.ps1'
        Published = $true
        Description = 'DSC Config for Mobility Service'
    }
    $AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
    ```

## <a name="compile-the-configuration-in-automation-dsc"></a>Compilar a configuração no Automation DSC

1. Compile a configuração no DSC da automatização, para que possa começar registar nós à mesma. Alcançar que executando o seguinte cmdlet:

    ```    powershell
    $AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
    ```

Isto pode demorar alguns minutos, porque basicamente estiver a implementar a configuração para o serviço de solicitação do DSC alojado.

2. Depois de compilar a configuração, pode obter as informações de tarefa com o PowerShell (Get-AzureRmAutomationDscCompilationJob) ou utilizando o [portal do Azure](https://portal.azure.com/).

    ![Obter a tarefa](./media/vmware-azure-mobility-deploy-automation-dsc/retrieve-job.png)

Tem agora com êxito publicado e carregar a configuração de DSC no DSC da automatização.

## <a name="onboard-machines-to-automation-dsc"></a>Máquinas carregar no Automation DSC


1. Certifique-se de que as máquinas do Windows são atualizadas com a versão mais recente do WMF. Pode transferir e instalar a versão correta para a sua plataforma a partir de [Centro de transferências](https://www.microsoft.com/download/details.aspx?id=50395).
2. Crie um metaconfig para DSC que será aplicada aos seus nós. Para ter êxito com esta opção, terá de obter o URL de ponto final e a chave primária para a sua conta de automatização selecionada no Azure. Pode encontrar estes valores em **chaves** no **todas as definições** painel da conta de automatização.

    ![Valores da chave](./media/vmware-azure-mobility-deploy-automation-dsc/key-values.png)

Neste exemplo, é necessário um servidor físico do Windows Server 2012 R2 que pretende proteger os utilizando a recuperação de Site.

## <a name="check-for-any-pending-file-rename-operations"></a>Procure quaisquer pendentes ficheiro mudar o nome de operações

Antes de começar a associar o servidor com o ponto final de DSC de automatização, recomendamos que verifique se existem quaisquer operações de mudança de nome de ficheiro no registo pendentes. O programa de configuração poderá proíbem a partir de concluir devido a um reinício pendente.

1. Execute o cmdlet seguinte para verificar que não há nenhum reinício pendente no servidor:

    ```powershell
    Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
    ```
2. Se esta ação mostra vazia, está OK para continuar. Caso contrário, deve resolver isto pelo servidor a ser reiniciado durante uma janela de manutenção.
3. Para aplicar a configuração no servidor, inicie o PowerShell Integrated Scripting Environment (ISE) e execute o seguinte script. Isto é, essencialmente, uma configuração local do DSC que instruirá o motor de Gestor de configuração locais para registar com o serviço de DSC de automatização e obter a configuração específica (ASRMobilityService.localhost).

    ```powershell
    [DSCLocalConfigurationManager()]
    configuration metaconfig {
        param (
            $URL,
            $Key
        )
        node localhost {
            Settings {
                RefreshFrequencyMins = '30'
                RebootNodeIfNeeded = $true
                RefreshMode = 'PULL'
                ActionAfterReboot = 'ContinueConfiguration'
                ConfigurationMode = 'ApplyAndMonitor'
                AllowModuleOverwrite = $true
            }

            ResourceRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }

            ConfigurationRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
                ConfigurationNames = 'ASRMobilityService.localhost'
            }

            ReportServerWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }
        }
    }
    metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'
    
    Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
    ```

Esta configuração irá fazer com que o motor de Gestor de configuração locais para se registar no Automation DSC. Este também determina como o motor deve funcionar, o deve fazer se não houver um que se desviam configuration (ApplyAndAutoCorrect) e como deve continuar com a configuração se é necessário um reinício.

1. Depois de executar este script, o nó deve começar a registar o DSC de automatização.

    ![Registo de nó em curso](./media/vmware-azure-mobility-deploy-automation-dsc/register-node.png)

2. Se regressar ao portal do Azure, pode ver que o nó recém-registado tem agora apareceu no portal.

    ![Nó registado no portal](./media/vmware-azure-mobility-deploy-automation-dsc/registered-node.png)

3. No servidor, pode executar o seguinte cmdlet do PowerShell para verificar se o nó foi registado corretamente:

    ```powershell
    Get-DscLocalConfigurationManager
    ```

4. Depois de serem solicitada e aplicada ao servidor de configuração, pode verificar isto, executando o seguinte cmdlet:

    ```powershell
    Get-DscConfigurationStatus
    ```

O resultado mostra que o servidor com êxito tem solicitados a respetiva configuração:

![Saída](./media/vmware-azure-mobility-deploy-automation-dsc/successful-config.png)

Além disso, o programa de configuração do serviço de mobilidade tem o seu próprio registo que pode ser encontrado em *SystemDrive*\ProgramData\ASRSetupLogs.

Já está. Agora com êxito implementou e registado o serviço de mobilidade na máquina que pretende proteger os utilizando a recuperação de Site. DSC irá garantir que os serviços necessários estão sempre em execução.

![Implementação efetuada com êxito](./media/vmware-azure-mobility-deploy-automation-dsc/successful-install.png)

Depois do servidor de gestão Deteta a implementação com êxito, pode configurar a proteção e ativar a replicação na máquina utilizando a recuperação de Site.

## <a name="use-dsc-in-disconnected-environments"></a>Utilizar o DSC em ambientes desligados

Se as máquinas não estão ligadas à Internet, pode confiar no DSC para implementar e configurar o serviço de mobilidade nas cargas de trabalho que pretende proteger.

Pode instanciar o seu próprio servidor de solicitação do DSC no seu ambiente para fornecer essencialmente as mesmas capacidades que obtém a partir do Automation DSC. Ou seja, os clientes irão solicitar a configuração (após estar registado) para o ponto final de DSC. No entanto, outra opção é emitir manualmente a configuração de DSC para as máquinas, localmente ou remotamente.

Tenha em atenção que neste exemplo, existe um parâmetro adicionado para o nome do computador. Os ficheiros remotos estão agora localizados numa partilha remota que deve estar acessível pelas máquinas que pretende proteger. O fim do script enacts a configuração e, em seguida, começa a aplicar a configuração de DSC ao computador de destino.

### <a name="prerequisites"></a>Pré-requisitos
Certifique-se de que o módulo do PowerShell xPSDesiredStateConfiguration está instalado. Para máquinas de Windows em que o WMF 5.0 é instalado, pode instalar o módulo de xPSDesiredStateConfiguration executando o seguinte cmdlet nos computadores de destino:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Também pode transferir e guardar o módulo no caso de necessitar distribui-lo para as máquinas do Windows que tem o WMF 4.0. Execute este cmdlet num computador onde se encontra presente PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Também para o WMF 4.0, certifique-se de que o [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) está instalado nos computadores.

A seguinte configuração pode ser enviada a máquinas do Windows que tenham WMF 5.0 e o WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Se pretender instanciar o seu próprio servidor de solicitação do DSC na sua rede empresarial para imitar as capacidades que pode obter a partir do Automation DSC, consulte [configurar um servidor de solicitação do DSC web](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Opcional: Implementar uma configuração de DSC utilizando um modelo Azure Resource Manager
Este artigo tem centra-se na forma como pode criar sua própria configuração de DSC para implementar o serviço de mobilidade e o agente da VM do Azure – e certifique-se automaticamente que estejam a ser executados nas máquinas que pretende proteger. Além disso, temos um modelo Azure Resource Manager que irá implementar esta configuração de DSC para uma conta de automatização do Azure nova ou existente. O modelo irá utilizar os parâmetros de entrada para criar recursos de automatização que irão conter as variáveis para o seu ambiente.

Depois de implementar o modelo, pode simplesmente Consultar passo 4 neste guia para carregar as máquinas.

O modelo será efetuado o seguinte:

1. Utilizar uma conta de automatização existente ou crie um novo
2. Obtenha os parâmetros de entrada para:
   * ASRRemoteFile – a localização em que tem armazenados o programa de configuração do serviço de mobilidade
   * ASRPassphrase – a localização onde tem armazenou o ficheiro de passphrase.txt
   * ASRCSEndpoint – o endereço IP do seu servidor de gestão
3. Importe o módulo do PowerShell xPSDesiredStateConfiguration
4. Criar e compilar a configuração DSC

Todos os passos anteriores irão acontecer na ordem correta, para que possa começar a integração do seu máquinas para proteção.

O modelo, com instruções para a implementação, está localizado no [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Implemente o modelo utilizando o PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Passos Seguintes
Depois de implementar os agentes do serviço de mobilidade, pode [ativar a replicação](vmware-azure-tutorial.md) para as máquinas virtuais.
