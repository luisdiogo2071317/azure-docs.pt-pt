---
title: Configurar o mapa de serviço no Azure | Microsoft Docs
description: O Mapa de Serviços é uma solução no Azure que deteta componentes da aplicação em sistemas Windows e Linux e mapeia a comunicação entre os serviços. Este artigo fornece detalhes para implementar o mapa de serviço no seu ambiente e utilizá-la numa variedade de cenários.
services: monitoring
documentationcenter: ''
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: aa85f06355ad5afc8e67ff4bace3b0ed471dc703
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204197"
---
# <a name="configure-service-map-in-azure"></a>Configurar o mapa de serviço no Azure
O Mapa de Serviço deteta automaticamente componentes de aplicações em sistemas Windows e Linux e mapeia a comunicação entre serviços. Pode utilizá-lo para ver os servidores que acha que deles – como interligados sistemas que fornecem serviços críticos. Mapa de serviço mostra as ligações entre servidores, processos e portas em qualquer arquitetura TCP ligados sem qualquer configuração necessária, que não seja a instalação de um agente.

Este artigo descreve os detalhes de configuração de agentes de mapa de serviço e integração. Para obter informações sobre como utilizar o mapa de serviço, consulte [utilizar a solução de mapa de serviço no Azure]( monitoring-service-map.md).

## <a name="dependency-agent-downloads"></a>Transferências do Agente de Dependência
| Ficheiro | SO | Versão | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.5.0 | 8B8FE0F6B0A9F589C4B7B52945C2C25DF008058EB4D4866DC45EE2485062C9D7 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.5.1 | 09D56EF43703A350FF586B774900E1F48E72FE3671144B5C99BB1A494C201E9E |


## <a name="connected-sources"></a>Origens ligadas
Mapa de serviço obtém os dados do agente de dependência do Microsoft. O Agente de Dependência depende do Agente do OMS para as suas ligações ao Log Analytics. Isto significa que um servidor tem de ter o agente do OMS instalado e configurado pela primeira vez e, em seguida, pode ser instalado o agente de dependência. A tabela seguinte descreve as origens ligadas que suporte a solução de mapa de serviço.

| Origem ligada | Suportadas | Descrição |
|:--|:--|:--|
| Agentes do Windows | Sim | Mapa de serviço analisa e recolhe dados de computadores de agente do Windows. <br><br>Além do [Agente do OMS](../log-analytics/log-analytics-windows-agent.md), os agentes do Windows precisam do Agente de Dependência da Microsoft. Veja os [sistemas operativos suportados](#supported-operating-systems) para obter uma lista completa das versões do sistema operativo. |
| Agentes do Linux | Sim | Mapa de serviço analisa e recolhe dados de computadores de agente do Linux. <br><br>Além do [Agente do OMS](../log-analytics/log-analytics-linux-agents.md), os agentes Linux precisam do Agente de Dependência da Microsoft. Veja os [sistemas operativos suportados](#supported-operating-systems) para obter uma lista completa das versões do sistema operativo. |
| Grupo de gestão do System Center Operations Manager | Sim | Mapa de serviço analisa e recolhe dados do Windows e Linux agentes num ligado [grupo de gestão do System Center Operations Manager](../log-analytics/log-analytics-om-agents.md). <br><br>É preciso uma ligação direta do computador do agente do System Center Operations Manager ao Log Analytics. Dados seja reencaminhados do grupo de gestão para a área de trabalho de análise de registos.|
| Conta de armazenamento do Azure | Não | Mapa de serviço recolhe dados de computadores de agente, pelo que não existem dados a partir do mesmo para recolher do armazenamento do Azure. |

Mapa de serviço suporta apenas plataformas de 64 bits.

No Windows, o Microsoft Monitoring Agent (MMA) é utilizado pelo System Center Operations Manager e análise de registos para recolher e enviar dados de monitorização. (Este agente é denominado o agente do System Center Operations Manager, agente do OMS, agente de análise do registo, MMA ou agente direta, dependendo no contexto.) System Center Operations Manager e análise de registos fornecem versões de caixa de fora os diferentes do MMA. Estas versões podem reportar ao System Center Operations Manager, ao Log Analytics ou a ambos.  

No Linux, o agente do OMS para reúne de Linux e envia dados para análise de registos de monitorização. Pode utilizar o mapa de serviço em servidores com agentes direta do OMS ou em servidores que estejam anexados a análise de registos através de grupos de gestão do System Center Operations Manager.  

Neste artigo, iremos irá referir-se a todos os agentes – se Linux ou Windows, se ligada a um grupo de gestão do System Center Operations Manager ou diretamente ao Log Analytics – como o "agente OMS." Iremos utilizar o nome da implementação específico do agente apenas se for necessário para o contexto.

O agente de mapa de serviço não transmitir quaisquer dados propriamente ditos, e não requer quaisquer alterações aos firewalls ou portas. Os dados no mapa de serviço é sempre transmitidos pelo agente OMS à análise de registos, diretamente ou através do Gateway do OMS.

![Agentes de mapa de serviço](media/monitoring-service-map/agents.png)

Se for um cliente do System Center Operations Manager com um grupo de gestão ligado à análise de registos:

- Se os agentes do System Center Operations Manager podem aceder à Internet para ligar à análise de registos, é necessária nenhuma configuração adicional.  
- Se os agentes do System Center Operations Manager não consegue aceder à análise de registos através da Internet, terá de configurar o Gateway do OMS para trabalhar com o System Center Operations Manager.
  
Se estiver a utilizar o agente do OMS direta, terá de configurar o agente de OMS para ligar à análise de registos ou para o seu Gateway OMS. O Gateway do OMS pode ser transferido a partir de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Pacotes de gestão
Quando o mapa de serviço está ativado numa área de trabalho de análise de registos, é enviado um pacote de gestão de 300 KB para todos os servidores do Windows na área de trabalho. Se estiver a utilizar os agentes do System Center Operations Manager num [grupo de gestão ligado](../log-analytics/log-analytics-om-agents.md), é implementado o pacote de gestão de mapa de serviço do System Center Operations Manager. Se os agentes estiverem ligados diretamente, o Log Analytics fornecerá o pacote de gestão.

O pacote de gestão é designado Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Este é escrito %Programfiles%\Microsoft monitorização Agent\Agent\Health serviço State\Management Packs\. A origem de dados que utiliza o pacote de gestão é % programa files%\Microsoft monitorização Agent\Agent\Health serviço State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Instalação
### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Instale o agente de dependência no Microsoft Windows
São precisos privilégios de administrador para instalar ou desinstalar o agente.

O agente de dependência é instalado em computadores com Windows através do InstallDependencyAgent Windows.exe. Se executar este ficheiro executável sem opções, ele iniciará um assistente que pode seguir para instalar de forma interativa.  

Utilize os seguintes passos para instalar o agente de dependência em cada computador do Windows:

1.  Instalar o agente do OMS, utilizando as instruções apresentadas em [computadores Windows ligar para o serviço de análise de registos do Azure](../log-analytics/log-analytics-windows-agent.md).
2.  Transferir o agente do Windows e execute-o utilizando o seguinte comando: <br>`InstallDependencyAgent-Windows.exe`
3.  Siga o assistente para instalar o agente.
4.  Se o Agente de Dependência não conseguir iniciar, verifique os registos para obter informações de erro detalhadas. Nos agentes do Windows, o diretório de registo é %Programfiles%\Microsoft Agent\logs de dependência. 

#### <a name="windows-command-line"></a>Linha de comandos do Windows
Utilize as opções da tabela seguinte para instalar a partir de uma linha de comandos. Para ver uma lista de sinalizadores de instalação, execute o instalador com o sinalizador /? da seguinte forma.

    InstallDependencyAgent-Windows.exe /?

| Sinalizador | Descrição |
|:--|:--|
| /? | Obter uma lista das opções da linha de comandos. |
| /S | Realizar uma instalação automática sem que seja solicitada a interação do utilizador. |

Por predefinição, os ficheiros do Agente de Dependência do Windows são colocados em C:\Program Files\Microsoft Dependency Agent.

### <a name="install-the-dependency-agent-on-linux"></a>Instalar o Agente de Dependência no Linux
Para instalar ou configurar o agente é preciso acesso à raiz.

O Agente de Dependência é instalado em computadores Linux através do ficheiro InstallDependencyAgent-Linux64.bin, um script de shell com um binário de extração automática. Pode executar o ficheiro utilizando ostrar ou adicionar permissões para o próprio ficheiro de execução.
 
Utilize os seguintes passos para instalar o Agente de Dependência em cada computador Linux:

1.  Instalar o agente do OMS, utilizando as instruções apresentadas em [recolher e gerir dados a partir de computadores com Linux](https://technet.microsoft.com/library/mt622052.aspx).
2.  Instale o agente de dependência de Linux como raiz usando o seguinte comando:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Se o Agente de Dependência não conseguir iniciar, verifique os registos para obter informações de erro detalhadas. Nos agentes Linux, o diretório de registo é /var/opt/microsoft/dependency-agent/log.

Para ver uma lista dos sinalizadores de instalação, execute a instalação do programa com-ajuda sinalizador da seguinte forma.

    InstallDependencyAgent-Linux64.bin -help

| Sinalizador | Descrição |
|:--|:--|
| -ajudar | Obter uma lista das opções da linha de comandos. |
| -s | Realizar uma instalação automática sem que seja solicitada a interação do utilizador. |
| -Verifique | Verificar as permissões e o sistema operativo, mas não instalar o agente. |

Os ficheiros do Agente de Dependência são colocados nos seguintes diretórios:

| Ficheiros | Localização |
|:--|:--|
| Ficheiros principais | /opt/microsoft/dependency-agent |
| Ficheiros de registo | /var/opt/microsoft/dependency-agent/log |
| Ficheiros de configuração | /etc/opt/microsoft/dependency-agent/config |
| Ficheiros executáveis de serviço | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Ficheiros de armazenamento binários | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Exemplos de script de instalação
Para implementar facilmente o Agente de Dependência em vários servidores ao mesmo tempo, poderá ser útil utilizar um script. Pode utilizar os seguintes exemplos de script para transferir e instalar o Agente de Dependência no Windows ou Linux.

### <a name="powershell-script-for-windows"></a>Script do PowerShell para Windows
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Script de shell para Linux
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="azure-vm-extension"></a>Extensão da VM do Azure
Pode facilmente implementar o agente de dependência para as VMs do Azure utilizando um [extensão da VM do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Com a extensão de VM do Azure, pode implementar o agente de dependência para as suas VMs através de um script do PowerShell ou diretamente no modelo do Azure Resource Manager a VM.  Uma extensão está disponível para Windows (DependencyAgentWindows) e do Linux (DependencyAgentLinux).  Se implementar através da extensão da VM do Azure, os agentes podem ser atualizados automaticamente para as versões mais recentes.

Para implementar a extensão da VM do Azure através do PowerShell, pode utilizar o exemplo seguinte:
```PowerShell
#
# Deploy the Dependency Agent to every VM in a Resource Group
#

$version = "9.4"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzureRmVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzureRmVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

É uma forma ainda mais fáceis de Certifique-se de que o agente de dependência encontra-se em cada uma das suas VMs para incluir o agente no seu modelo do Azure Resource Manager.  Tenha em atenção que o agente de dependência depende ainda o agente do OMS, por isso, o [extensão de VM de agente do OMS](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-vm-extension) tem de ser implementada pela primeira vez.  É possível adicionar o seguinte fragmento de JSON para o *recursos* secção do seu modelo.
```JSON
"type": "Microsoft.Compute/virtualMachines/extensions",
"name": "[concat(parameters('vmName'), '/DependencyAgent')]",
"apiVersion": "2017-03-30",
"location": "[resourceGroup().location]",
"dependsOn": [
"[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
],
"properties": {
    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
    "type": "DependencyAgentWindows",
    "typeHandlerVersion": "9.4",
    "autoUpgradeMinorVersion": true
}

```


## <a name="desired-state-configuration"></a>Configuração do Estado Pretendido
Para implementar o Agente de Dependência através da Configuração do Estado Pretendido, pode utilizar o módulo xPSDesiredStateConfiguration e um excerto de código, por exemplo:
```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install the Dependency Agent
    xRemoteFile DAPackage 
    {
        Uri = "https://aka.ms/dependencyagentwindows"
        DestinationPath = $DAPackageLocalPath
    }

    xPackage DA
    {
        Ensure="Present"
        Name = "Dependency Agent"
        Path = $DAPackageLocalPath
        Arguments = '/S'
        ProductId = ""
        InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
        InstalledCheckRegValueName = "DisplayName"
        InstalledCheckRegValueData = "Dependency Agent"
        DependsOn = "[xRemoteFile]DAPackage"
    }
  }
}
```

## <a name="uninstallation"></a>Desinstalação
### <a name="uninstall-the-dependency-agent-on-windows"></a>Desinstalar o Agente de Dependência no Windows
Um administrador pode desinstalar o Agente de Dependência do Windows através do Painel de Controlo.

Um administrador também pode executar o ficheiro %Programfiles%\Microsoft Dependency Agent\Uninstall.exe para desinstalar o Agente de Dependência.

### <a name="uninstall-the-dependency-agent-on-linux"></a>Desinstalar o Agente de Dependência no Linux
Pode desinstalar o agente de dependência do Linux com o seguinte comando.
<br>RHEL, CentOs ou Oracle:
```
sudo rpm -e dependency-agent
```
Ubuntu:
```
sudo apt -y purge dependency-agent
```
## <a name="troubleshooting"></a>Resolução de problemas
Se tiver quaisquer problemas de instalação e execução de mapa de serviço, esta secção pode ajudá-lo. Se ainda não é possível resolver o problema, contacte Support da Microsoft.

### <a name="dependency-agent-installation-problems"></a>Problemas de instalação do agente de dependência
#### <a name="installer-asks-for-a-reboot"></a>Instalador pede-lhe uma reinicialização
O agente de dependência *geralmente* não requer um reinício após a instalação ou desinstalação. No entanto, em certos casos raros, Windows Server requer uma reinicialização para prosseguir com uma instalação. Isto acontece quando uma dependência, normalmente, o Microsoft Visual C++ Redistributable, requer uma reinicialização devido a um ficheiro de bloqueado.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Mensagem "não é possível instalar o agente de dependência: não foi possível instalar o Runtime do Visual Studio bibliotecas (código = [code_number])" é apresentada

O agente de dependência Microsoft baseia-se as bibliotecas de runtime do Microsoft Visual Studio. Irá receber uma mensagem se houver um problema durante a instalação das bibliotecas. 

Os programas de instalação de biblioteca de tempo de execução criar registos na pasta %LOCALAPPDATA%\temp. O ficheiro é dd_vcredist_arch_yyyymmddhhmmss.log, onde *arquitetura* é "x86" ou "amd64" e *yyyymmddhhmmss* é a data e hora (relógio de 24 horas) quando o registo foi criado. O registo fornece detalhes sobre o problema que está a bloquear a instalação.

Poderá ser útil instalar o [bibliotecas de tempo de execução mais recentes](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) sozinho primeiro.

A tabela seguinte apresenta uma lista de números de código e resoluções sugeridas.

| Código | Descrição | Resolução |
|:--|:--|:--|
| 0x17 | O instalador de biblioteca requer uma atualização do Windows que não foi instalada. | Procure no registo do instalador mais recente da biblioteca.<br><br>Se uma referência a "Windows8.1-KB2999226-x64.msu" seguida de uma linha "erro 0x80240017: Falha ao executar o pacote MSU," não tem os pré-requisitos para instalar KB2999226. Siga as instruções na secção pré-requisitos [Universal C tempo de execução no Windows](https://support.microsoft.com/kb/2999226). Poderá ter de executar o Windows Update e reiniciar várias vezes para instalar os pré-requisitos.<br><br>Execute novamente o instalador do agente de dependência da Microsoft. |

### <a name="post-installation-issues"></a>Problemas de pós-instalação
#### <a name="server-doesnt-appear-in-service-map"></a>Servidor não são apresentados no mapa de serviço
Se a instalação do agente de dependência com êxito, mas não vir o seu servidor na solução de mapa de serviço:
* O agente de dependência é instalado com êxito? Pode validar esta verificação para ver se o serviço está instalado e em execução.<br><br>
**Windows**: procure o serviço com o nome "Microsoft Dependency agente".<br>
**Linux**: procure para a execução processo "microsoft-dependência-agent."

* São-na [livre preços escalão do Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? O plano gratuito permite até cinco servidores exclusivos de mapa de serviço. Quaisquer servidores subsequentes não serão apresentados no mapa de serviço, mesmo que os cinco anterior já não estão a enviar dados.

* É o envio de registo do servidor e dados de desempenho a análise de registos? Aceda a pesquisa de registo e execute a seguinte consulta para o seu computador: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Obteve uma variedade de eventos nos resultados? Os dados é recente? Se Sim, o agente do OMS está a funcionar corretamente e comunicar com a análise de registos. Caso contrário, verifique o agente do OMS no seu servidor: [resolução de problemas do agente do OMS para Windows](https://support.microsoft.com/help/3126513/how-to-troubleshoot- monitoring-onboarding-issues) ou [agente do OMS para resolução de problemas do Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>O servidor é apresentado no mapa de serviço, mas não tem nenhum processo
Se vir o seu servidor no mapa de serviço, mas não tem nenhum processo ou da ligação de dados, que indica que o agente de dependência está instalado e em execução, mas não carregar o controlador de kernel. 

Verifique o ficheiro de C:\Program Files\Microsoft dependência Agent\logs\wrapper.log (Windows) ou o ficheiro /var/opt/microsoft/dependency-agent/log/service.log (Linux). As últimas linhas do ficheiro devem indicar por que motivo o kernel não carregar. Por exemplo, o kernel poderá não ser suportado no Linux, se atualizar o kernel.

## <a name="data-collection"></a>Recolha de dados
Pode esperar cada agente a transmitir aproximadamente 25 MB por dia, consoante complexos como as dependências de sistema são. Cada agente envia dados de dependência de mapa de serviço a cada 15 segundos.  

O agente de dependência normalmente consome 0.1 percentagem de memória de sistema e 0.1 percentagem de CPU do sistema.

## <a name="supported-azure-regions"></a>Regiões do Azure suportadas
Mapa de serviço está atualmente disponível em regiões do Azure do seguintes:
- EUA Leste
- Europa Ocidental
- EUA Centro-Oeste
- Sudeste Asiático


## <a name="supported-operating-systems"></a>Sistemas operativos suportados
As secções seguintes listam os sistemas operativos suportados para o Agente de Dependência. Mapa de serviço não suporta arquiteturas de 32 bits para qualquer sistema operativo.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Ambiente de trabalho do Windows
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux e Oracle Linux (com Kernel RHEL)
- Apenas as versões de kernel padrão e SMP Linux são suportadas.
- As versões kernel não padrão, tais como PAE e Xen, não são suportadas para qualquer distribuição de Linux. Por exemplo, um sistema com a cadeia de versão de "2.6.16.21-0.8-xen" não é suportado.
- Kernels personalizados, incluindo recompilações de kernels padrão, não são suportados.
- O kernel do CentOSPlus kernel não é suportado.
- O Oracle Unbreakable Enterprise Kernel (UEK) é abordado numa secção posterior deste artigo.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Versão do SO | Versão de kernel |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Versão do SO | Versão de kernel |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Versão do SO | Versão de kernel |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419<br>2.6.18-420 |

### <a name="ubuntu-server"></a>Ubuntu Server
- Kernels personalizados, incluindo recompilações de kernels padrão, não são suportados.

| Versão do SO | Versão de kernel |
|:--|:--|
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux com Unbreakable Enterprise Kernel
#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Versão do SO | Versão de kernel
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Versão do SO | Versão de kernel
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Versão do SO | Versão de kernel
|:--|:--|
| 11 SP2 | 3.0.101-0.7 |
| 11 SP3 | 3.0.101-0.47 |
| 11 SP4 | 3.0.101-65 |


## <a name="diagnostic-and-usage-data"></a>Dados de utilização e diagnóstico
A Microsoft recolhe automaticamente dados de utilização e desempenho através da utilização do serviço de mapa de serviço. A Microsoft utiliza estes dados para fornecer e melhorar a qualidade, segurança e integridade do serviço de mapa de serviço. Os dados incluem informações sobre a configuração do seu software, como o sistema operativo e versão. Também inclui endereço IP, o nome DNS e o nome da estação de trabalho para fornecer capacidades de resolução de problemas exatas e eficientes. Não recolhemos nomes, moradas ou outras informações de contacto.

Para obter mais informações sobre a utilização e recolha de dados, consulte o [declaração de privacidade do Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Passos Seguintes
- Saiba como [utilizar o mapa de serviço]( monitoring-service-map.md) depois foi implementado e configurado.
