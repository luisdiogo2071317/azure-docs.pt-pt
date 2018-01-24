---
title: "Gerir o agente de análise de registos do Azure | Microsoft Docs"
description: "Este artigo descreve as tarefas de gestão diferentes que, normalmente, irá executar durante o ciclo de vida da monitorização do agente Microsoft (MMA) implementado numa máquina."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: magoedte
ms.openlocfilehash: a17418142fb5f52a93d7a56cb2e6e6e97a250002
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Gerir e manter o agente de análise de registos para o Windows e Linux

Após a implementação inicial do agente do Windows ou Linux para análise de registos, poderá ter de reconfigurar o agente dependendo da situação ou removê-lo a partir do computador se atingiu a fase de extinção no respetivo ciclo de vida.  Pode gerir facilmente estas tarefas de manutenção de rotina manualmente ou através da automatização, o que reduz o erro operacional e as despesas.

## <a name="adding-or-removing-a-workspace"></a>Adição ou remoção de uma área de trabalho 

### <a name="windows-agent"></a>Agente do Windows

#### <a name="update-settings-from-control-panel"></a>Atualizar as definições no painel de controlo

1. Inicie sessão computador com uma conta que tenha direitos administrativos.
2. Abra o **Painel de Controlo**.
3. Selecione **Microsoft Monitoring Agent** e, em seguida, clique em de **análise de registos do Azure (OMS)** separador.
4. Se remover uma área de trabalho, selecione-o e, em seguida, clique em **remover**. Repita este passo para qualquer outra área de trabalho que pretende que o agente pare de reportar.
5. Se adicionar uma área de trabalho, clique em **adicionar** e o **adicionar uma área de trabalho de análise do registo** caixa de diálogo, cole o ID da área de trabalho e a chave de área de trabalho (chave primária). Se o computador deve reportar uma área de trabalho de análise de registos na nuvem do Azure Government, selecione Azure US Government a nuvem do Azure na lista pendente. 
6. Clique em **OK** para guardar as alterações.

#### <a name="remove-a-workspace-using-powershell"></a>Remover uma área de trabalho com o PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Adicionar uma área de trabalho no Azure através do PowerShell comercial 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>Adicionar uma área de trabalho no Azure para US Government através do PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Se utilizou a linha de comandos ou script anteriormente para instalar ou configurar o agente, `EnableAzureOperationalInsights` foi substituído por `AddCloudWorkspace` e `RemoveCloudWorkspace`.
>

## <a name="update-proxy-settings"></a>Atualizar as definições de proxy 
Para configurar o agente para comunicar com o serviço através de um servidor proxy ou [OMS Gateway](log-analytics-oms-gateway.md) após a implementação, utilize um dos seguintes métodos para concluir esta tarefa.

### <a name="windows-agent"></a>Agente do Windows

#### <a name="update-settings-using-control-panel"></a>Definições de atualização utilizando o painel de controlo

1. Inicie sessão computador com uma conta que tenha direitos administrativos.
2. Abra o **Painel de Controlo**.
3. Selecione **Microsoft Monitoring Agent** e, em seguida, clique em de **as definições de Proxy** separador.
4. Clique em **utilizar um servidor proxy** e forneça o URL e o número de porta do servidor proxy ou gateway. Se o servidor proxy ou Gateway do OMS requer autenticação, escreva o nome de utilizador e palavra-passe para autenticar e, em seguida, clique em **OK**. 

#### <a name="update-settings-using-powershell"></a>Definições de atualização utilizando o PowerShell 

Copie o seguinte código do PowerShell de exemplo, a atualização com informações específicas para o seu ambiente e guarde-o com uma extensão de nome de ficheiro PS1.  Execute o script em cada computador que estabelece ligação diretamente para o serviço de análise de registos.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object.  We need to determine if we
#have the right update rollup with the API we need.  If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
     Write-Output 'Health Service proxy API not present, will not update settings.'
     return
}

Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username

Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```  

### <a name="linux-agent"></a>Agente Linux
Execute os passos seguintes se os computadores Linux têm de comunicar através de um servidor proxy ou Gateway do OMS à análise de registos.  O valor de configuração do proxy tem a seguinte sintaxe `[protocol://][user:password@]proxyhost[:port]`.  O *proxyhost* propriedade aceita um nome de domínio completamente qualificado ou o endereço IP do servidor proxy.

1. Edite o ficheiro `/etc/opt/microsoft/omsagent/proxy.conf` ao executar os comandos seguintes e altere os valores para as definições específicas.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Reinicie o agente ao executar o seguinte comando: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="uninstall-agent"></a>Desinstalar agente
Utilize um dos seguintes procedimentos para desinstalar o agente do Windows ou Linux utilizando o Assistente de linha de comandos ou de configuração.

### <a name="windows-agent"></a>Agente do Windows

#### <a name="uninstall-from-control-panel"></a>Desinstalar a partir do painel de controlo
1. Inicie sessão computador com uma conta que tenha direitos administrativos.  
2. No **painel de controlo**, clique em **programas e funcionalidades**.
3. No **programas e funcionalidades**, clique em **Microsoft Monitoring Agent**, clique em **desinstalação**e, em seguida, clique em **Sim**.

>[!NOTE]
>O Assistente de configuração do agente também pode ser executado fazendo duplo clique em **Mmasetup-i386.exe -<platform>.exe**, que está disponível para transferência a partir de uma área de trabalho no portal do Azure.

#### <a name="uninstall-from-the-command-line"></a>Desinstalar a partir da linha de comandos
O ficheiro transferido para o agente é um pacote de instalação autónomo criado com IExpress.  O programa de configuração para o agente e os ficheiros de suporte estão contidos no pacote e tem de ser extraído para desinstalar corretamente com a linha de comandos mostrada no exemplo seguinte. 

1. Inicie sessão computador com uma conta que tenha direitos administrativos.  
2. Para extrair os ficheiros de instalação do agente, a partir de uma linha de comandos elevada, execute `extract MMASetup-<platform>.exe` e que irá solicitar que o caminho extrair ficheiros para.  Em alternativa, pode especificar o caminho transferindo os argumentos `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`.  Para obter mais informações sobre a linha de comandos swtiches suportado pelo IExpress, consulte [comutadores da linha de comandos para IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) e, em seguida, atualize o exemplo de acordo com as suas necessidades.
3. Na linha de comandos, escreva `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.  

### <a name="linux-agent"></a>Agente Linux
Para remover o agente, efetue os passos seguintes.

1. Transfira o agente Linux [universal script](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) para o computador.
2. Execute o ficheiro .sh do pacote com o argumento *--purge* no computador, o que remove completamente o agente e a respetiva configuração.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Configurar o agente para reportar a um grupo de gestão do Operations Manager

### <a name="windows-agent"></a>Agente do Windows
Execute os seguintes passos para configurar o agente do OMS para Windows para que reportem a um grupo de gestão do System Center Operations Manager. 

1. Inicie sessão computador com uma conta que tenha direitos administrativos.
2. Abra o **Painel de Controlo**. 
3. Clique em **Microsoft Monitoring Agent** e, em seguida, clique em de **do Operations Manager** separador.
4. Se os servidores do Operations Manager tem integração com o Active Directory, clique em **atualizar automaticamente atribuições de grupo de gestão do AD DS**.
5. Clique em **adicionar** para abrir o **adicionar um grupo de gestão** caixa de diálogo.
6. No **nome do grupo de gestão** campo, escreva o nome do seu grupo de gestão.
7. No **servidor de gestão principal** campo, escreva o nome de computador do servidor de gestão principal.
8. No **porta do servidor de gestão** campo, escreva o número de porta TCP.
9. Em **conta de ação do agente**, escolha a conta de sistema Local ou uma conta de domínio local.
10. Clique em **OK** para fechar o **adicionar um grupo de gestão** caixa de diálogo e, em seguida, clique em **OK** para fechar o **propriedades do agente de monitorização Microsoft** caixa de diálogo.

### <a name="linux-agent"></a>Agente Linux
Execute os seguintes passos para configurar o agente do OMS para Linux para que reportem a um grupo de gestão do System Center Operations Manager. 

1. Edite o ficheiro`/etc/opt/omi/conf/omiserver.conf`
2. Certifique-se de que o início da linha com `httpsport=` define a porta 1270. Tal como:`httpsport=1270`
3. Reinicie o servidor OMI:`sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Passos Seguintes

Reveja [resolução de problemas do agente Linux](log-analytics-agent-linux-support.md) se ocorrerem problemas ao instalar ou gerir o agente.  