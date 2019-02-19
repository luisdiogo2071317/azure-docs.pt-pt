---
title: Resolver problemas de integração de erros a gestão de atualizações, controlo de alterações e inventário
description: Saiba como resolver erros de integração com a gestão de atualizações, controlo de alterações e soluções de inventário
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 01/25/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ac11b1a2b625d1fc7b62130580d1f188ead21051
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342733"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Resolver erros quando soluções de integração

Pode encontrar erros quando soluções de integração como gestão de atualizações ou controlo de alterações e inventário. Este artigo descreve os vários erros que possam ocorrer e como resolvê-los.

## <a name="general-errors"></a>Erros de geral

### <a name="missing-write-permissions"></a>Cenário: Falha de integração com a mensagem - a solução não pode ser ativada

#### <a name="issue"></a>Problema

Recebe a seguinte mensagem quando está tentando carregar uma máquina virtual para uma solução:

```
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

#### <a name="cause"></a>Causa

Este erro é causado por permissões incorretas ou ausentes na máquina virtual ou para o utilizador.

#### <a name="resolution"></a>Resolução

Certifique-se de que tem permissões corretas para carregar a máquina virtual. Reveja os [permissões necessárias para carregar máquinas](../automation-role-based-access-control.md#onboarding) e repita para integrar a solução.

### <a name="computer-group-query-format-error"></a>Cenário: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problema

Este código de erro significa que a consulta de pesquisa guardada de grupo de computador, usada para indicar a solução não estava devidamente formatada. 

#### <a name="cause"></a>Causa

Pode ter alterado a consulta ou ele pode foram alterado pelo sistema.

#### <a name="resolution"></a>Resolução

É possível eliminar a consulta para esta solução e reonboard a solução, que recria a consulta. A consulta pode ser encontrada em sua área de trabalho, em **pesquisas guardadas**. O nome da consulta é **MicrosoftDefaultComputerGroup**, e a categoria da consulta é o nome da solução associada a esta consulta. Se várias soluções estão ativadas, o **MicrosoftDefaultComputerGroup** mostra várias vezes sob **pesquisas guardadas**.

### <a name="policy-violation"></a>Cenário: PolicyViolation

#### <a name="issue"></a>Problema

Este código de erro significa que a implementação falhou devido a violação de uma ou mais políticas.

#### <a name="cause"></a>Causa 

É uma política que está a bloquear a conclusão da operação.

#### <a name="resolution"></a>Resolução

Para poder implementar com êxito a solução, precisa considerar alterar a política indicada. Como existem muitos tipos diferentes de políticas que podem ser definidos, as alterações específicas necessárias dependem da política que é violada. Por exemplo, se foi definida uma política no grupo de recursos que recusada permissão para alterar o conteúdo de certos tipos de recursos no grupo de recursos, poderia, por exemplo, faça qualquer um dos seguintes:

* Remova completamente a política.
* Tente carregar a um grupo de recursos diferente.
* Rever a política para por, por exemplo:
  * Filtragem novamente a política a um recurso específico (como para uma conta de automatização específica).
  * Revisando o conjunto de recursos que política foi configurada para negar.

Verifique as notificações no canto superior direito do portal do Azure ou navegue para o grupo de recursos que contém a sua conta de automatização e selecione **implementações** sob **definições** para ver a implementação. Para saber mais sobre o Azure Policy, visite: [Descrição geral do Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>Falhas de extensão do MMA

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Ao implementar uma solução, uma variedade de recursos relacionados são implementadas. Um destes recursos é o agente de extensão de agente de monitorização da Microsoft ou o Log Analytics para Linux. Estes são instalados pelo agente convidado da máquina virtual que é responsável por comunicar com a Log Analytics área de trabalho configurada, para efeitos de coordenação posterior de transferência de binários de extensões de máquinas virtuais e outros ficheiros que o solução estiver a integrar dependem de uma vez que ele inicia a execução.
Normalmente primeiro ficar ciente do agente MMA ou o Log Analytics para Linux de falhas de instalação de uma notificação que aparece no Hub de notificações. Clicar em que a notificação fornece ainda mais informações sobre a falha específica. Navegação para o recurso de grupos de recursos e, em seguida, o elemento de implementações que ele fornece também detalhes sobre as falhas de implementação que ocorreram.
Instalação do agente MMA ou o Log Analytics para Linux pode falhar por vários motivos e os passos para resolver essas falhas variam, dependendo do problema. Siga os passos de resolução de problemas específicos.

A seguinte secção descreve vários problemas que pode encontrar ao integração que causar uma falha na implementação da extensão do MMA.

### <a name="webclient-exception"></a>Cenário: Ocorreu uma exceção durante uma solicitação WebClient

A extensão MMA na máquina virtual não consegue comunicar com recursos externos e a implementação falhar.

#### <a name="issue"></a>Problema

Seguem-se exemplos de mensagens de erro que são devolvidos:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Causa

Algumas causas possíveis para este erro são:

* Há um proxy configurado na VM, que só permite portas específicas.

* Uma definição de firewall bloqueou o acesso para os endereços e portas necessárias.

#### <a name="resolution"></a>Resolução

Certifique-se de que tem as portas apropriadas e endereços abrir para a comunicação. Para obter uma lista de endereços e portas, consulte [planear a sua rede](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Cenário: A instalação falhou devido a problemas de ambiente transitório

A instalação da extensão do Microsoft Monitoring Agent falhou durante a implementação devido a outra instalação ou ação bloqueando a instalação

#### <a name="issue"></a>Problema

Seguem-se exemplos de mensagens de erro podem ser devolvidos:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Causa

Algumas causas possíveis para este erro são:

* Está em curso outra instalação
* O sistema é foi acionada para reiniciar o computador durante a implementação de modelo

#### <a name="resolution"></a>Resolução

Este erro é um erro transitório por natureza. Repetir a implementação para instalar a extensão.

### <a name="installation-timeout"></a>Cenário: Tempo limite da instalação

A instalação da extensão do MMA não foi concluída devido a um tempo limite.

#### <a name="issue"></a>Problema

Segue-se um exemplo de uma mensagem de erro que pode ser devolvido:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este erro ocorre devido a máquina virtual a ser sob uma pesada carga durante a instalação.

### <a name="resolution"></a>Resolução

Tente instalar a extensão MMA quando a VM está sob uma carga menor.

## <a name="next-steps"></a>Passos Seguintes

Se não VI o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.
