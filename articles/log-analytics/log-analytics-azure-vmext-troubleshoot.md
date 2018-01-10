---
title: "Resolver problemas com a extensão de VM de análise de registos do Azure | Microsoft Docs"
description: "Descrevem os sintomas, faz com que e resolução de problemas mais comuns com a extensão de VM de análise do registo para o Windows e as VMs do Linux do Azure."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.openlocfilehash: 4e43c7a7cea903a2e94e60a519f6ead1e6f932e3
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>A extensão de VM de análise do registo de resolução de problemas
Este artigo fornece erros podem ocorrer com a extensão de VM de análise do registo para o Windows e Linux máquinas de virtuais em execução no Microsoft Azure e sugere possíveis soluções para resolvê-los de resolução de problemas.

Para verificar o estado da extensão, execute os seguintes passos no portal do Azure.

1. Inicie sessão no [Portal do Azure](http://portal.azure.com).
2. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **máquinas virtuais**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **máquinas virtuais**.
3. Na lista de máquinas virtuais, localize e selecione-o.
3. Na máquina virtual, clique em **extensões**.
4. Na lista, verifique se a extensão de análise de registos está ativada ou não.  Para Linux, o agente está listado como **OMSAgentforLinux** e para o Windows, o agente está listado como **MicrosoftMonitoringAgent**.

   ![Vista de extensão de VM](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Clique na extensão para ver os detalhes. 

   ![Detalhes da extensão de VM](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Resolução de problemas de extensão de VM do Windows Azure

Se o *Microsoft Monitoring Agent* extensão da VM não está a instalar ou relatórios, pode realizar os seguintes passos para resolver o problema.

1. Verificar se o agente da VM do Azure está instalado e a funcionar corretamente ao utilizar os passos em [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Também pode rever o ficheiro de registo do agente VM`C:\WindowsAzure\logs\WaAppAgent.log`
   * Se o registo de não existir, o agente da VM não está instalado.
   * [Instale o agente VM do Azure](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Confirme que está a executar a tarefa de heartbeat de extensão do Microsoft Monitoring Agent através dos seguintes passos:
   * Inicie sessão na máquina virtual
   * Abra o Programador de tarefas e localize o `update_azureoperationalinsight_agent_heartbeat` tarefas
   * Certifique-se a tarefa está ativada e está a executar cada um minuto
   * Verifique o ficheiro de registo de heartbeat`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Reveja os ficheiros de registo de extensão de VM do Microsoft Monitoring Agent no`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Certifique-se de que a máquina virtual pode executar scripts do PowerShell
5. Certifique-se de que as permissões num C:\Windows\temp ainda não foram alteradas
6. Ver o estado do Microsoft Monitoring Agent, escrevendo o seguinte numa janela elevada do PowerShell na máquina virtual`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Consulte os ficheiros de registo de configuração do Microsoft Monitoring Agent no`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Para obter mais informações, consulte [resolução de problemas de extensões do Windows](../virtual-machines/windows/extensions-oms.md).

## <a name="troubleshooting-linux-vm-extension"></a>Resolução de problemas de extensão de VM com Linux
Se o *agente do OMS para Linux* extensão da VM não está a instalar ou relatórios, pode realizar os seguintes passos para resolver o problema.

1. Se o estado da extensão *desconhecido* verificar se o agente da VM do Azure está instalado e a funcionar corretamente, revendo o ficheiro de registo do agente VM`/var/log/waagent.log`
   * Se o registo de não existir, o agente da VM não está instalado.
   * [Instalar o agente da VM do Azure em VMs do Linux](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Para outros Estados mau estado de funcionamento, consulte o agente do OMS para ficheiros de registo de extensão de VM com Linux `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` e`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Se o estado da extensão está em bom estado, mas não estão a ser carregados dados reveja o agente do OMS para ficheiros de registo do Linux na`/var/opt/microsoft/omsagent/log/omsagent.log`

Para obter mais informações, consulte [resolução de problemas de extensões de Linux](../virtual-machines/linux/extensions-oms.md).

## <a name="next-steps"></a>Passos Seguintes

Para orientações adicionais de resolução de problemas relacionados com o agente do OMS para Linux alojado em computadores fora do Azure, consulte [resolver problemas de registo de análise de agente Linux do Azure](log-analytics-agent-linux-support.md).  
