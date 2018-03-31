---
title: Consola de série de máquina virtual do Azure | Microsoft Docs
description: Consola de série bidirecional para máquinas virtuais do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 2ff0dcba0912461d8528fc76c7c67d90febc0324
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Consola de série de máquina virtual (pré-visualização) 


A consola de série de máquina virtual no Azure fornece acesso a uma consola baseada em texto para as máquinas virtuais de Linux e Windows. Esta ligação série é COM1 porta série da máquina virtual e fornece acesso à máquina virtual e não estão relacionadas com a rede da máquina virtual / estado do sistema de operativo. Acesso à consola de série para uma máquina virtual pode ser feito atualmente apenas através do portal do Azure e permitido apenas para os utilizadores que tenham contribuinte de VM ou acima acesso à máquina virtual. 

> [!Note] 
> Pré-visualizações ficam disponíveis para si condition que aceita os termos de utilização. Para obter mais informações, consulte [Microsoft suplementares termos de utilização do Azure para a pré-visualizações do Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Este serviço está atualmente no **pré-visualização pública** e acesso à consola de série para máquinas virtuais está disponível para global regiões do Azure. Neste momento a consola de série não está disponível na nuvem do Azure Government, Datacenters do Azure e Azure China.

 

## <a name="prerequisites"></a>Pré-requisitos 

* Máquina virtual tem de ter [diagnóstico de arranque](boot-diagnostics.md) ativada 
* A conta utilizando a consola de série tem de ter [função de contribuinte](../../active-directory/role-based-access-built-in-roles.md) para a VM e o [diagnóstico de arranque](boot-diagnostics.md) conta de armazenamento. 

## <a name="open-the-serial-console"></a>Abra a consola de série
consola de série para máquinas virtuais só é acessível através de [portal do Azure](https://portal.azure.com). Seguem-se os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu à esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + secção de resolução de problemas e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série irá abrir e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Desativar a funcionalidade
A funcionalidade de consola de série, pode ser desativada para específicas VMs, desativando a definição de diagnóstico de arranque essa VM.

## <a name="serial-console-security"></a>Segurança da consola de série 

### <a name="access-security"></a>Segurança de acesso 
Acesso à consola de série está limitado a utilizadores que tenham [VM contribuintes](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) ou posterior acesso para a máquina virtual. Se o inquilino do AAD requer multi-factor Authentication, em seguida, acesso à consola de série também terá de MFA como o acesso é através de [portal do Azure](https://portal.azure.com).

### <a name="channel-security"></a>Segurança de canal
Todos os dados são enviados de volta e estabelecido são encriptados em risco.

### <a name="audit-logs"></a>Registos de auditoria
Todo o acesso à consola de série tem sessão iniciado [diagnóstico de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) registos da máquina virtual. Acesso a estes registos são propriedade e é controlado pelo administrador de máquina virtual do Azure.  

>[!CAUTION] 
Enquanto nenhuma palavra-passe de acesso para a consola sessão iniciada, se executar a consola de comandos contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, às serão escritos para o diagnóstico de arranque de máquina virtual funcionalidade de cópia de registos, juntamente com todos os outro texto visível, como parte da implementação do deslocamento da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura para a conta de armazenamento do diagnostics têm acesso aos mesmos, no entanto, recomendamos a seguir a melhor prática utilizando o ambiente de trabalho remoto para tudo o que poderá envolver o método segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um utilizador está ligado à consola de série e outro utilizador com êxito os pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligados de forma akin to o primeiro utilizador colocado cópias de segurança e mantendo a consola física e um novo utilizador junto para baixo.

>[!CAUTION] 
Isto significa que o utilizador for desligado não será registado! A capacidade de impor um fim de sessão após desligar (através de SIGHUP ou mecanismo semelhante) ainda está a ser plano. Para o Windows, não há um limite de tempo automática ativada no SAC, no entanto, para Linux, pode configurar a definição de tempo limite de terminal. 


## <a name="accessing-serial-console-for-windows"></a>Aceder à consola de série para Windows 
Terão de imagens do Windows Server mais recentes no Azure [especial consola administrativa](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) ativada por predefinição. SAC é suportada em versões de servidor do Windows, mas não está disponível nas versões de cliente (por exemplo, Windows 10, Windows 8 ou Windows 7). Para ativar a consola de série para máquinas virtuais do Windows criada com a utilização Feb2018 ou imagens inferiores utilizam os seguintes passos: 

1. Ligar a sua máquina virtual do Windows através do ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Reiniciar o sistema para a consola de SAC esteja ativado

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Se SAC necessárias pode ser ativadas offline bem, 

1. Anexe o disco do windows que pretende SAC configurado para como um disco de dados para a VM existente. 
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Como saber se SAC está ativada ou não 

Se [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) não está ativada a consola de série não apresentará uma linha de comandos da SAC. Esta operação pode mostrar informações de um Estado de funcionamento da VM em alguns casos ou seria em branco.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Ativar o menu de arranque Mostrar na consola de série 

Se precisar de ativar o carregador de arranque do Windows pede-lhe para mostrar na consola de série, pode adicionar as seguintes opções adicionais para o carregador de arranque do Windows.

1. Ligar a sua máquina virtual do Windows através do ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Reiniciar o sistema para o menu de arranque esteja ativado

> [!NOTE] 
> No suporte neste ponto para a função de chaves não está ativado, se necessitar de opções de arranque avançadas utilizam bcdedit /set {atual} onetimeadvancedoptions, consulte [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) para obter mais detalhes

## <a name="windows-commands---cmd"></a>Comandos do Windows - CMD 

Esta secção inclui comandos de exemplo para realizar tarefas comuns em cenários em que poderá ter de utilizar SAC para aceder a VM, por exemplo, quando tiver de resolver problemas de falhas de ligação de RDP.

SAC foi incluído em todas as versões do Windows desde o Windows Server 2003, mas está desativada por predefinição. SAC depende o `sacdrv.sys` controladores de kernel, o `Special Administration Console Helper` serviço (`sacsvr`) e o `sacsess.exe` processo. Para obter mais informações, consulte [definições e ferramentas de serviços de gestão de emergência](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10)).

SAC permite-lhe ligar ao seu SO em execução através de porta série. Quando iniciar CMD de SAC, `sacsess.exe` inicia `cmd.exe` no seu SO em execução. Pode ver que na tarefa de gestor se RDP para a VM ao mesmo tempo estão ligadas a SAC através da funcionalidade de consola de série. CMD aceder através de SAC é o mesmo `cmd.exe` utilizar quando estiver ligado através de RDP. As mesmas ferramentas e comandos estão disponíveis, incluindo a capacidade de inicie o PowerShell a partir dessa instância CMD. Que é uma principal diferença entre SAC e do ambiente de recuperação do Windows (WinRE) em que SAC é permitindo-lhe gerir o seu SO em execução, onde o WinRE arranca num SO diferente, mínimo. Enquanto as VMs do Azure não suportam a capacidade de aceder WinRE, com a funcionalidade de consola de série, as VMs do Azure podem ser geridas através de SAC.

Uma vez que SAC está limitado a uma memória intermédia de ecrã de 80 x 24 com nenhum deslocamento anterior, adicione `| more` para comandos para apresentar a página de um de saída a uma hora. Utilize `<spacebar>` para ver a página seguinte, ou `<enter>` para ver a linha seguinte.  

`SHIFT+INSERT` é o atalho de colar para a janela de consola de série.

Devido a memória intermédia de ecrã limitada do SAC, mais comandos podem ser mais fácil para o tipo num editor de texto local e, em seguida, colados SAC.

### <a name="view-and-edit-windows-registry-settings"></a>Ver e editar as definições de registo do Windows
#### <a name="verify-rdp-is-enabled"></a>Certifique-se de que está ativado RDP
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

A segunda chave (em \Policies) existirá apenas se estiver configurada a definição de política de grupo relevantes.

#### <a name="enable-rdp"></a>Ativar RDP
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0` 

A segunda chave (em \Policies) seria apenas necessário se a definição de política de grupo relevantes tinha sido configurada. Irá ser foi reescrito valor na seguinte atualização de política de grupo se estiver configurado na política de grupo.

### <a name="manage-windows-services"></a>Gerir serviços do Windows

#### <a name="view-service-state"></a>Ver estado do serviço
`sc query termservice`
####  <a name="view-service-logon-account"></a>Conta de início de sessão do serviço de vista
`sc qc termservice`
#### <a name="set-service-logon-account"></a>Definir a conta de início de sessão do serviço 
`sc config termservice obj= "NT Authority\NetworkService"`

É necessário um espaço após o sinal de igual.
#### <a name="set-service-start-type"></a>Tipo de início do conjunto de serviço
`sc config termservice start= demand` 

É necessário um espaço após o sinal de igual. Os valores de início possíveis incluem `boot`, `system`, `auto`, `demand`, `disabled`, `delayed-auto`.
#### <a name="set-service-dependencies"></a>Dependências do serviço de conjunto
`sc config termservice depend= RPCSS`

É necessário um espaço após o sinal de igual.
#### <a name="start-service"></a>Iniciar o serviço
`net start termservice`

ou

`sc start termservice`
#### <a name="stop-service"></a>Parar serviço
`net stop termservice`

ou

`sc stop termservice`
### <a name="manage-networking-features"></a>Gerir as funcionalidades de redes
#### <a name="show-nic-properties"></a>Mostrar propriedades NIC
`netsh interface show interface` 
#### <a name="show-ip-properties"></a>Mostrar propriedades IP
`netsh interface ip show config`
#### <a name="show-ipsec-configuration"></a>Mostrar a configuração de IPSec
`netsh nap client show configuration`  
#### <a name="enable-nic"></a>Ativar NIC
`netsh interface set interface name="<interface name>" admin=enabled`
#### <a name="set-nic-to-use-dhcp"></a>Definir o NIC para utilizar DHCP
`netsh interface ip set address name="<interface name>" source=dhcp`

As VMs do Azure devem sempre ser configuradas do SO para utilizar DHCP para obter um endereço IP do convidado. A definição de IP estática do Azure ainda utiliza DHCP para atribuir o IP estático para a VM.
#### <a name="ping"></a>Ping
`ping 8.8.8.8` 
#### <a name="port-ping"></a>Ping de porta  
Instalar o cliente telnet

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Testar conectividade

`telnet bing.com 80`

Para remover o cliente telnet

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Quando limitada para métodos disponíveis no Windows, por predefinição, o PowerShell pode ser uma abordagem de melhor para testar a conectividade de porta. Consulte a secção do PowerShell abaixo para obter exemplos.
#### <a name="test-dns-name-resolution"></a>Testar a resolução de nome DNS
`nslookup bing.com`
#### <a name="show-windows-firewall-rule"></a>Mostrar a regra de Firewall do Windows
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
#### <a name="disable-windows-firewall"></a>Desativar a Firewall do Windows
`netsh advfirewall set allprofiles state off`

Pode utilizar este comando quando a resolução de problemas para temporariamente regra de saída a Firewall do Windows. Irá ser ativar no reinício seguinte ou quando a enaable-la utilizando o comando abaixo. Não pare o serviço de Windows Firewall (MPSSVC) ou o serviço de motor de filtragem de Base (BFE) como forma de regra de Firewall do Windows. Parar MPSSVC ou BFE resultará na conectividade todas as ser bloqueada.
#### <a name="enable-windows-firewall"></a>Ativar a Firewall do Windows
`netsh advfirewall set allprofiles state on`
### <a name="manage-users-and-groups"></a>Gerir utilizadores e grupos
#### <a name="create-local-user-account"></a>Criar conta de utilizador local
`net user /add <username> <password>`
#### <a name="add-local-user-to-local-group"></a>Adicionar utilizador local ao grupo local
`net localgroup Administrators <username> /add`
#### <a name="verify-user-account-is-enabled"></a>Certifique-se de que a conta de utilizador está ativada
`net user <username> | find /i "active"`

As VMs do Azure criadas a partir de imagem generalizada terão a conta de administrador local, mudar o nome para o nome especificado durante o aprovisionamento de VM. Para que, normalmente, não será `Administrator`.
#### <a name="enable-user-account"></a>Ativar a conta de utilizador
`net user <username> /active:yes`  
#### <a name="view-user-account-properties"></a>Ver propriedades da conta de utilizador
`net user <username>`

Exemplos de linhas de interesse de uma conta de administrador local:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

#### <a name="view-local-groups"></a>Vista de grupos locais
`net localgroup`
### <a name="manage-the-windows-event-log"></a>Gerir o registo de eventos do Windows
#### <a name="query-event-log-errors"></a>Erros de registo de eventos de consulta
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Alteração `/c:10` para o número pretendido de eventos para regressar ou movê-la para devolver todos os eventos correspondentes o filtro.
#### <a name="query-event-log-by-event-id"></a>Consultar o registo de eventos por ID de evento
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Registo de eventos por ID de evento e o fornecedor de consulta
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Consultar o registo de eventos por ID de evento e o fornecedor de últimas 24 horas
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Utilize `604800000` parecer novamente 7 dias, em vez de 24 horas.
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Consultar o registo de eventos por ID de evento, o fornecedor e o EventData nos últimos 7 dias
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Visualizar ou remover aplicações instaladas
#### <a name="list-installed-applications"></a>Aplicações de lista instalada
`wmic product get Name,InstallDate | sort /r | more`

O `sort /r` ordena descendente por instalar data torna mais fácil ver o que foi recentemente instalado. Utilize `<spacebar>` para avançar para a página seguinte do resultado, ou `<enter>` para produzir uma linha.
#### <a name="uninstall-an-application"></a>Desinstalar uma aplicação
`wmic path win32_product where name="<name>" call uninstall`

Substitua `<name>` com o nome devolvido no comando acima para a aplicação que pretende remover.

### <a name="file-system-management"></a>Gestão do sistema de ficheiros
#### <a name="get-file-version"></a>Obter a versão do ficheiro
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

Neste exemplo devolve a versão do ficheiro do controlador NIC virtual, que é netvsc.sys, netvsc63.sys ou netvsc60.sys consoante a versão do Windows.
#### <a name="scan-for-system-file-corruption"></a>Análise de danos no sistema de ficheiros
`sfc /scannow`

Consulte também [reparar uma imagem do Windows](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="scan-for-system-file-corruption"></a>Análise de danos no sistema de ficheiros
`dism /online /cleanup-image /scanhealth`

Consulte também [reparar uma imagem do Windows](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="export-file-permissions-to-text-file"></a>Permissões de ficheiros de exportação para o ficheiro de texto
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
#### <a name="save-file-permissions-to-acl-file"></a>Guardar as permissões do ficheiro no ficheiro ACL
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`  
#### <a name="restore-file-permissions-from-acl-file"></a>Restaurar as permissões do ficheiro a partir do ficheiro ACL
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

O caminho quando utilizar `/restore` tem de ser a pasta principal da pasta que especificou quando utilizar `/save`. Neste exemplo, `\RSA` é o principal do `\MachineKeys` pasta especificada no `/save` exemplo acima.
#### <a name="take-ntfs-ownership-of-a-folder"></a>Assumir a propriedade NTFS de uma pasta
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`  
#### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>Conceder permissões de NTFS para uma pasta recursivamente
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`  
### <a name="manage-devices"></a>Gerir dispositivos
#### <a name="remove-non-present-pnp-devices"></a>Remover dispositivos PNP não presente
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
### <a name="manage-group-policy"></a>Gerir a política de grupo
#### <a name="force-group-policy-update"></a>Forçar a atualização da política de grupo
`gpupdate /force /wait:-1`
### <a name="miscellaneous-tasks"></a>Diversas tarefas
#### <a name="show-os-version"></a>Mostrar a versão do SO
`ver`

ou 

`wmic os get caption,version,buildnumber /format:list`

ou 

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
#### <a name="view-os-install-date"></a>Data de instalação do SO de vista
`systeminfo | find /i "original"`

ou 

`wmic os get installdate`
#### <a name="view-last-boot-time"></a>Vista de hora do último arranque
`systeminfo | find /i "system boot time"`
#### <a name="view-time-zone"></a>Vista fuso horário
`systeminfo | find /i "time zone"`

ou

`wmic timezone get caption,standardname /format:list`
#### <a name="restart-windows"></a>Reiniciar o Windows
`shutdown /r /t 0`

Adicionar `/f` irá forçar as aplicações em execução para fechar sem utilizadores de aviso.
#### <a name="detect-safe-mode-boot"></a>Detetar o arranque do modo de segurança
`bcdedit /enum | find /i "safeboot"` 

## <a name="windows-commands---powershell"></a>Comandos do Windows - PowerShell

Para executar o PowerShell no SAC, depois de atingir uma linha de comandos, escreva:

`powershell <enter>`

>[!CAUTION]
Remova o módulo de PSReadLine da sessão do PowerShell antes de executar outros comandos do PowerShell. Não há um problema conhecido onde carateres adicionais podem ser introduzidos no texto colado da área de transferência, se estiver a executar numa sessão do PowerShell SAC PSReadLine.

Verifique primeiro se PSReadLine é carregado. Que é carregado por predefinição no Windows Server 2016, o Windows 10 e versões posteriores do Windows. -Só estarão presente em versões anteriores do Windows se tinha sido manualmente instalada. 

Se este comando devolve para uma linha de comandos sem saída de, em seguida, o módulo não foi carregado e pode continuar a utilizar a sessão do PowerShell no SAC como habitualmente.

`get-module psreadline`

Se o comando acima devolver a versão do módulo PSReadLine, execute o seguinte comando ao descarregá-lo. Este comando não eliminar ou desinstalar o módulo, apenas efetua a descarga-la na sessão atual do PowerShell.

`remove-module psreadline`

### <a name="view-and-edit-windows-registry-settings"></a>Ver e editar as definições de registo do Windows
#### <a name="verify-rdp-is-enabled"></a>Certifique-se de que está ativado RDP
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

A segunda chave (em \Policies) existirá apenas se estiver configurada a definição de política de grupo relevantes.
#### <a name="enable-rdp"></a>Ativar RDP
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

A segunda chave (em \Policies) seria apenas necessário se a definição de política de grupo relevantes tinha sido configurada. Irá ser foi reescrito valor na seguinte atualização de política de grupo se estiver configurado na política de grupo.
### <a name="manage-windows-services"></a>Gerir serviços do Windows
#### <a name="view-service-details"></a>Ver detalhes do serviço
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service` pode ser utilizado, mas não inclui a conta de início de sessão do serviço. `Get-WmiObject win32-service` efetua.
#### <a name="set-service-logon-account"></a>Definir a conta de início de sessão do serviço
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

Ao utilizar uma conta de serviço diferente de `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, ou `LocalSystem`, especifique a palavra-passe de conta como último argumento (oitavo) após o nome da conta.
#### <a name="set-service-startup-type"></a>Definir tipo de arranque do serviço
`set-service termservice -startuptype Manual`

`Set-service` aceita `Automatic`, `Manual`, ou `Disabled` para o tipo de arranque.
#### <a name="set-service-dependencies"></a>Dependências do serviço de conjunto
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
#### <a name="start-service"></a>Iniciar o serviço
`start-service termservice`
#### <a name="stop-service"></a>Parar serviço
`stop-service termservice`
### <a name="manage-networking-features"></a>Gerir as funcionalidades de redes
#### <a name="show-nic-properties"></a>Mostrar propriedades NIC
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

ou 

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter` está disponível no 2012 +, para utilização 2008 R2 `Get-WmiObject`.
#### <a name="show-ip-properties"></a>Mostrar propriedades IP
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
#### <a name="enable-nic"></a>Ativar NIC
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

ou

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter` está disponível no 2012 +, para utilização 2008 R2 `Get-WmiObject`.
#### <a name="set-nic-to-use-dhcp"></a>Definir o NIC para utilizar DHCP
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter` está disponível no 2012 +. Para 2008 R2 utilizar `Get-WmiObject`. As VMs do Azure devem sempre ser configuradas do SO para utilizar DHCP para obter um endereço IP do convidado. A definição de IP estática do Azure ainda utiliza DHCP para atribuir o IP para a VM.
#### <a name="ping"></a>Ping
`test-netconnection`

ou

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection` sem quaisquer parâmetros irão tentar enviar um ping `internetbeacon.msedge.net`. Está disponível no 2012 +. Para 2008 R2 utilizar `Get-WmiObject` do segundo exemplo.
#### <a name="port-ping"></a>Ping de porta
`test-netconnection -ComputerName bing.com -Port 80`

ou

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection` está disponível no 2012 +. Para utilização 2008 R2 `Net.Sockets.TcpClient`
#### <a name="test-dns-name-resolution"></a>Testar a resolução de nome DNS
`resolve-dnsname bing.com` 

ou 

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName` está disponível no 2012 +. Para 2008 R2 utilizar `System.Net.DNS`.
#### <a name="show-windows-firewall-rule-by-name"></a>Mostrar a regra de firewall do Windows por nome
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP` 
#### <a name="show-windows-firewall-rule-by-port"></a>Mostrar a regra de firewall do Windows através da porta
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

ou

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter` está disponível no 2012 +. Para 2008 R2, utilize o `hnetcfg.fwpolicy2` objecto COM. 
#### <a name="disable-windows-firewall"></a>Desativar a firewall do Windows
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile` está disponível no 2012 +. Para 2008 R2 utilizar `netsh advfirewall` como referenciado na secção CMD acima.
### <a name="manage-users-and-groups"></a>Gerir utilizadores e grupos
#### <a name="create-local-user-account"></a>Criar conta de utilizador local
`new-localuser <name>`
#### <a name="verify-user-account-is-enabled"></a>Certifique-se de que a conta de utilizador está ativada
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

ou 

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser` está disponível no 2012 +. Para 2008 R2 utilizar `Get-WmiObject`. Este exemplo mostra a conta de administrador local incorporada, que tem sempre SID `S-1-5-21-*-500`. As VMs do Azure criadas a partir de imagem generalizada terão a conta de administrador local, mudar o nome para o nome especificado durante o aprovisionamento de VM. Para que, normalmente, não será `Administrator`.
#### <a name="add-local-user-to-local-group"></a>Adicionar utilizador local ao grupo local
`add-localgroupmember -group Administrators -member <username>`
#### <a name="enable-local-user-account"></a>Ativar a conta de utilizador local
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser` 

Este exemplo permite que a conta de administrador local incorporada, que tem sempre SID `S-1-5-21-*-500`. As VMs do Azure criadas a partir de imagem generalizada terão a conta de administrador local, mudar o nome para o nome especificado durante o aprovisionamento de VM. Para que, normalmente, não será `Administrator`.
#### <a name="view-user-account-properties"></a>Ver propriedades da conta de utilizador
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

ou 

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser` está disponível no 2012 +. Para 2008 R2 utilizar `Get-WmiObject`. Este exemplo mostra a conta de administrador local incorporada, que tem sempre SID `S-1-5-21-*-500`.
#### <a name="view-local-groups"></a>Vista de grupos locais
`(get-localgroup).name | sort` `(get-wmiobject win32_group).Name | sort`

`Get-LocalUser` está disponível no 2012 +. Para 2008 R2 utilizar `Get-WmiObject`.
### <a name="manage-the-windows-event-log"></a>Gerir o registo de eventos do Windows
#### <a name="query-event-log-errors"></a>Erros de registo de eventos de consulta
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Alteração `/c:10` para o número pretendido de eventos para regressar ou movê-la para devolver todos os eventos correspondentes o filtro.
#### <a name="query-event-log-by-event-id"></a>Consultar o registo de eventos por ID de evento
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Registo de eventos por ID de evento e o fornecedor de consulta
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Consultar o registo de eventos por ID de evento e o fornecedor de últimas 24 horas
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Utilize `604800000` parecer novamente 7 dias, em vez de 24 horas. |
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Consultar o registo de eventos por ID de evento, o fornecedor e o EventData nos últimos 7 dias
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Visualizar ou remover aplicações instaladas
#### <a name="list-installed-software"></a>Software de lista instalada
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
#### <a name="uninstall-software"></a>Desinstalar o software
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
### <a name="file-system-management"></a>Gestão do sistema de ficheiros
#### <a name="get-file-version"></a>Obter a versão do ficheiro
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

Neste exemplo devolve a versão do ficheiro do controlador NIC virtual, que é denominado netvsc.sys, netvsc63.sys ou netvsc60.sys consoante a versão do Windows.
#### <a name="download-and-extract-file"></a>Transferir e extrair ficheiros
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

Este exemplo cria um `c:\bin` pasta, em seguida, transfere e extrai o conjunto de Sysinternals de ferramentas para `c:\bin`.
### <a name="miscellaneous-tasks"></a>Diversas tarefas
#### <a name="show-os-version"></a>Mostrar a versão do SO
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber` 
#### <a name="view-os-install-date"></a>Data de instalação do SO de vista
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
#### <a name="view-last-boot-time"></a>Vista de hora do último arranque
`(get-wmiobject win32_operatingsystem).lastbootuptime`
#### <a name="view-windows-uptime"></a>Ver o tempo de atividade do Windows
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Devolve o tempo de atividade como `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, por exemplo `49:16:48:00.00`. 
#### <a name="restart-windows"></a>Reiniciar o Windows
`restart-computer`

Adicionar `-force` irá forçar as aplicações em execução para fechar sem utilizadores de aviso.
### <a name="instance-metadata"></a>Metadados de instância

Pode consultar os metadados de instância do Azure de dentro da sua VM do Azure para ver os detalhes como osType, localização, vmSize, vmId, nome, resourceGroupName, subscriptionId, privateIpAddress e publicIpAddress.

Consulta de metadados de instância requer conectividade de rede de convidado bom estado de funcionamento, porque faz uma chamada REST através do anfitrião do Azure para o serviço de metadados de instância. Por isso, se é possível consultar os metadados de instância, que indica que o convidado é capaz de comunicar através da rede para um serviço alojado no Azure.

Para obter mais informações, consulte [metadados de instância do Azure service](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service).

#### <a name="instance-metadata"></a>Metadados de instância
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
#### <a name="os-type-instance-metadata"></a>Tipo de SO (metadados de instância)
`$im.Compute.osType`
#### <a name="location-instance-metadata"></a>Localização (metadados de instância)
`$im.Compute.Location`
#### <a name="size-instance-metadata"></a>Tamanho (metadados de instância)
`$im.Compute.vmSize`
#### <a name="vm-id-instance-metadata"></a>ID de VM (metadados de instância)
`$im.Compute.vmId`
#### <a name="vm-name-instance-metadata"></a>Nome da VM (metadados de instância)
`$im.Compute.name`
#### <a name="resource-group-name-instance-metadata"></a>Nome do grupo de recursos (metadados de instância)
`$im.Compute.resourceGroupName`
#### <a name="subscription-id-instance-metadata"></a>ID de subscrição (metadados de instância)
`$im.Compute.subscriptionId`
#### <a name="tags-instance-metadata"></a>Etiquetas (metadados de instância)
`$im.Compute.tags`
#### <a name="placement-group-id-instance-metadata"></a>ID do grupo de colocação de (metadados de instância)
`$im.Compute.placementGroupId`
#### <a name="platform-fault-domain-instance-metadata"></a>Domínio de falhas da plataforma (metadados de instância)
`$im.Compute.platformFaultDomain`
#### <a name="platform-update-domain-instance-metadata"></a>Domínio de atualização de plataforma (metadados de instância)
`$im.Compute.platformUpdateDomain`
#### <a name="ipv4-private-ip-address-instance-metadata"></a>Endereço de IP privado de IPv4 (metadados de instância)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
#### <a name="ipv4-public-ip-address-instance-metadata"></a>Endereço IP público de IPv4 (metadados de instância)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
#### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>Endereço de sub-rede IPv4 / prefixo (metadados de instância)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
#### <a name="ipv6-ip-address-instance-metadata"></a>Endereço IP de IPv6 (metadados de instância)
`$im.network.interface.ipv6.ipAddress`
#### <a name="mac-address-instance-metadata"></a>Endereço MAC (metadados de instância)
`$im.network.interface.macAddress`

## <a name="errors"></a>Erros
A maioria dos erros são transitórios na natureza e o endereço de ligação de repetir estes. Tabela abaixo mostra uma lista de erros e atenuação 

Erro                            |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não é possível obter as definições de diagnóstico de arranque para '<VMNAME>'. Para utilizar a consola de série, certifique-se de que esse diagnóstico de arranque está ativado para esta VM. | Certifique-se de que a VM possui [diagnóstico de arranque](boot-diagnostics.md) ativada. 
A VM está no estado parado desalocado. Iniciar a VM e repita a ligação da consola de série. | Máquina virtual deve estar num estado iniciado para aceder à consola de série
Não tem as permissões necessárias para utilizar esta consola de série de VM. Certifique-se de que tem, pelo menos, permissões de função de contribuinte de VM.| O acesso à consola de série requer determinada permissão para aceder. Consulte [aceder aos requisitos](#prerequisites) para obter detalhes
Não é possível determinar o grupo de recursos para a conta de armazenamento de diagnóstico de arranque '<STORAGEACCOUNTNAME>'. Certifique-se de que o diagnóstico de arranque está ativado para esta VM e que tem acesso a esta conta de armazenamento. | O acesso à consola de série requer determinada permissão para aceder. Consulte [aceder aos requisitos](#prerequisites) para obter detalhes

## <a name="known-issues"></a>Problemas conhecidos 
Tal como estamos ainda nas fases pré-visualização de acesso à consola de série, iremos estão a funcionar através de alguns problemas conhecidos, segue-se a lista de estas com possíveis soluções 

Problema                           |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não há nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine série | No momento da pré-visualização, o acesso à consola de série de instâncias de conjunto de dimensionamento de máquina virtual não é suportado.
Atingir introduza depois da faixa de ligação não mostra um registo numa linha de comandos | [Atingir introduza faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Apenas as informações de estado de funcionamento são apresentadas ao ligar a uma VM do Windows| [Sinais de estado de funcionamento do Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)
Não é possível escreva SAC perguntar se a depuração de kernel está ativada | RDP para a VM e execute `bcdedit /debug {current} off` da linha de comandos elevada. Se não for possível RDP em vez disso, pode anexar o disco de SO para outra VM do Azure e modificá-lo enquanto ligado como um disco de dados utilizando `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, em seguida, trocar o disco novamente.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**Q. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder à https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através do azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com 

**Q. Posso obter um erro "consola existente tem de tipo de SO"Windows"com o tipo de SO pedido do Linux em conflito?**

A. Este é um problema conhecido para corrigir isto, basta abrir [Shell de nuvem do Azure](https://docs.microsoft.com/azure/cloud-shell/overview) no modo de bash e tente novamente.

**Q. Não consigo aceder à consola de série, onde ficheiro um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida através de termos de pré-visualização do Azure. Suporte para isto é melhor processado através dos canais mencionados acima. 

## <a name="next-steps"></a>Passos Seguintes
* Também está disponível para a consola de série [Linux](../linux/serial-console.md) VMs
* Saiba mais sobre [bootdiagnostics](boot-diagnostics.md)
