---
title: Resolver erros integração gestão de atualizações, controlo de alterações e inventário
description: Saiba como resolver erros de integração com a gestão de atualizações, controlo de alterações e soluções de inventário
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 044cb56b8991a1eb2dd6a1d35be621f2ffab3250
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063911"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Resolver erros quando soluções de integração

Pode encontrar erros quando soluções de integração, como a gestão de atualizações ou de registo de alterações e de inventário. Este artigo descreve os vários erros que possam ocorrer e como resolvê-los.

## <a name="general-errors"></a>Erros gerais

### <a name="computer-grou-query-format-error"></a>Cenário: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problema

Este código de erro significa que a consulta de grupo do computador de pesquisa guardada utilizada para a solução de destino não foi formatada corretamente. 

#### <a name="cause"></a>Causa

Poderá ter alterado a consulta, ou pode ter sido alterada pelo sistema.

#### <a name="resolution"></a>Resolução

Pode eliminar a consulta para esta solução e reonboard a solução, a qual recria a consulta. A consulta pode ser encontrada na sua área de trabalho em **pesquisas guardadas**. O nome da consulta é **MicrosoftDefaultComputerGroup**, e a categoria da consulta é o nome da solução associado a esta consulta. Se várias soluções estiverem ativadas, o **MicrosoftDefaultComputerGroup** mostra várias vezes em **pesquisas guardadas**.

### <a name="policy-violation"></a>Cenário: PolicyViolation

#### <a name="issue"></a>Problema

Este código de erro significa que a implementação falha devido a violação de uma ou mais políticas.

#### <a name="cause"></a>Causa 

É uma política no local que está a bloquear a conclusão da operação.

#### <a name="resolution"></a>Resolução

Para implementar com êxito a solução, terá de considerar alterar a política indicada. Como existem vários tipos de políticas que podem ser definidas, as necessidade de alterações específicas dependem a política é violada. Por exemplo, se uma política foi definida num grupo de recursos que negou permissão ao alterar o conteúdo de determinados tipos de recursos nesse grupo de recursos, pode, por exemplo, efetuar qualquer uma das seguintes:

* Remova-a completamente.
* Tente carregar para um grupo de recursos diferentes.
* Rever a política para ao, por exemplo:
  * Novamente direcionar a política para um recurso específico (como juntar uma conta de automatização específica).
  * A revisão o conjunto de recursos, essa política foi configurada para negar.

Verifique as notificações no canto superior direito do portal do Azure ou navegue para o grupo de recursos que contém a sua conta de automatização e selecione **implementações** em **definições** para ver a implementação. Para obter mais informações sobre a visita de política do Azure: [descrição geral da política do Azure](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>Falhas de extensão MMA

Quando implementar uma solução, uma variedade de recursos relacionados são implementados. Um destes recursos é a extensão de agente de monitorização da Microsoft ou o agente do OMS para Linux. Estes são instalados pelo agente convidado da máquina virtual que é responsável por comunicar com a configurado Operations Management Suite (OMS) área de trabalho para fins de coordenação posterior de transferência de binários de extensões de Máquina Virtual e outros ficheiros que a solução são integração dependem de uma vez que começa a execução.
É, normalmente, primeiro estarão informados da MMA ou o agente do OMS para falhas de instalação de Linux a partir de uma notificação no Hub de notificações. Clicar na notificação de que fornece mais informações sobre a falha específica. Navegação para o recurso de grupos de recursos e, em seguida, o elemento de implementações dentro da mesma também fornece detalhes sobre as falhas de implementação que ocorreram.
Instalação do agente do OMS ou MMA para Linux pode falhar por diversos motivos e os passos a tomar para resolver estas falhas variam, consoante o problema. Siga os passos de resolução de problemas específicos.

A seguinte secção descreve vários problemas que podem surgir quando integração que causa uma falha na implementação da extensão MMA.

### <a name="webclient-exception"></a>Cenário: Ocorreu uma excepção durante um pedido WebClient

A extensão MMA na máquina virtual não é possível comunicar com recursos externos e a implementação falhar.

#### <a name="issue"></a>Problema

Seguem-se exemplos de mensagens de erro que são devolvidos:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Causa

Algumas causas possíveis para este erro são:

* Não há um proxy configurado na VM, que só permite portas específicas.

* Uma definição de firewall bloqueou o acesso aos endereços e portas necessários.

#### <a name="resolution"></a>Resolução

Certifique-se de que tem as portas adequadas e endereços abrir para comunicação. Para obter uma lista de endereços e portas, consulte [planeamento da sua rede](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Cenário: Falha na instalação devido a problemas de ambiente transitório

A instalação da extensão do Microsoft Monitoring Agent falhou durante a implementação devido a outra instalação ou ação bloquear a instalação

#### <a name="issue"></a>Problema

Seguem-se exemplos de mensagens de erro podem ser devolvidos:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Causa

Algumas causas possíveis para este erro são:

* Está em curso outra instalação
* O sistema é foi acionada reiniciar durante a implementação do modelo

#### <a name="resolution"></a>Resolução

Este erro é um erro transitório natureza. Repita a implementação para instalar a extensão.

### <a name="installation-timeout"></a>Cenário: Tempo limite de instalação

A instalação da extensão MMA não foi concluída devido a um tempo limite.

#### <a name="issue"></a>Problema

Segue-se um exemplo de uma mensagem de erro que pode ser devolvido:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este erro é porque a máquina virtual a ser sobrecarregado um durante a instalação.

### <a name="resolution"></a>Resolução

Tentativa de instalar a extensão MMA quando a VM está sob uma carga inferior.

## <a name="next-steps"></a>Passos Seguintes

Se não foi possível ver o seu problema ou não conseguir resolver o problema, visite uma das seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.
