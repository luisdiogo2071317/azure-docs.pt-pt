---
title: Resolver problemas de erros com gestão de atualizações
description: Saiba como resolver problemas com a gestão de atualizações
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b77d1210ff48a4bd30834fcbad64173bf77b1290
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064100"
---
# <a name="troubleshooting-issues-with-update-management"></a>Resolução de problemas com a gestão de atualizações

Este artigo aborda soluções para resolver problemas que poderá encontrar ao utilizar a gestão de atualizações

## <a name="windows"></a>Windows

Se ocorrerem problemas ao tentar carregar a solução numa máquina virtual, verifique o **do Operations Manager** registo de eventos em **registos de serviços e aplicações** no computador local para eventos com ID de evento **4502** e contém mensagens de evento **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

A secção seguinte realça uma resolução possível e as mensagens de erro específica para cada. Para integração de outra problemas, consulte [resolver problemas de integração de solução](onboarding.md).

### <a name="machine-already-registered"></a>Cenário: A máquina já está registada para uma conta diferente

#### <a name="issue"></a>Problema

Recebe a seguinte mensagem de erro:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Causa

A máquina já está integrado para outra área de trabalho para gestão de atualizações.

#### <a name="resolution"></a>Resolução

Executar limpeza de artefactos antigos na máquina por [a eliminar o grupo de runbook híbridas](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) e tente novamente.

### <a name="machine-unable-to-communicate"></a>Cenário: Não é possível máquina comunicar com o serviço

#### <a name="issue"></a>Problema

Receber uma das seguintes mensagens de erro:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Causa

Pode existir um proxy, gateway ou firewall a bloquear a comunicação de rede.

#### <a name="resolution"></a>Resolução

Reveja o funcionamento em rede e certifique-se de que as portas adequadas e endereços são permitidos. Consulte [requisitos de rede](../automation-hybrid-runbook-worker.md#network-planning), para obter uma lista de portas e endereços que são necessários para gestão de atualizações e os Runbook Workers híbridos.

### <a name="unable-to-create-selfsigned-cert"></a>Cenário: Não é possível criar certificado autoassinado

#### <a name="issue"></a>Problema

Receber uma das seguintes mensagens de erro:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Causa

O Runbook Worker híbrido não foi capaz de gerar um certificado autoassinado

#### <a name="resolution"></a>Resolução

Certifique-se de que a conta do sistema tem acesso de leitura à pasta **C:\ProgramData\Microsoft\Crypto\RSA** e tente novamente.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Cenário: Falha de execução de atualização iniciar

#### <a name="issue"></a>Problema

Uma atualização é executado falhar ao iniciar um computador Linux.

#### <a name="cause"></a>Causa

O Worker híbrido de Linux está danificado.

#### <a name="resolution"></a>Resolução

Faça uma cópia do ficheiro de registo seguinte e preservá-lo para fins de resolução de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Cenário: Execução de atualização é iniciada, mas encontra erros

#### <a name="issue"></a>Problema

Uma execução de atualização é iniciada, mas detetar erros durante a execução.

#### <a name="cause"></a>Causa

Pode ser causas possíveis:

* Gestor de pacote está danificado
* Pacotes específicos podem interferir com a aplicação de patches baseada na nuvem
* Outros motivos

#### <a name="resolution"></a>Resolução

Se ocorrerem falhas durante uma atualização executada depois de iniciar com êxito no Linux, consulte a tarefa ao computador afetado na execução de saída. Pode localizar mensagens de erro específicas do Gestor de pacotes da máquina que pode pesquisar e executar ações em. Gestão de atualizações requer o Gestor de pacotes estar em bom estado de funcionamento para implementações de atualização com êxito.

Em alguns casos, as atualizações do pacote podem interferir na gestão atualizar impedir a conclusão de uma implementação de atualização. Se vir que terá que excluir estes pacotes de execuções de atualização futura ou instalá-los manualmente por si.

Se não é possível resolver um problema de aplicação de patches, faça uma cópia do ficheiro de registo seguinte e preservar- **antes** inicia a implementação de atualização seguinte para fins de resolução de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Passos Seguintes

Se não foi possível ver o seu problema ou não conseguir resolver o problema, visite uma das seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.