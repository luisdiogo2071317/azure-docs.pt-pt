---
title: Resolver erros de gerenciamento de atualizações
description: Saiba como resolver problemas de gerenciamento de atualizações
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 08/08/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 2e47320d5ad88edfa8ea6122f3a0abd104230974
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42055781"
---
# <a name="troubleshooting-issues-with-update-management"></a>Resolução de problemas de gerenciamento de atualizações

Este artigo aborda soluções para resolver problemas que poderá encontrar ao utilizar a gestão de atualizações.

## <a name="general"></a>Geral

### <a name="components-enabled-not-working"></a>Cenário: Os componentes da solução de gestão de atualizações foram ativados e agora esta máquina virtual está a ser configurada

#### <a name="issue"></a>Problema

Continuar a ver a seguinte mensagem numa máquina virtual de 15 minutos após a integração:

```
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. Comunicação de volta para a conta de automatização está a ser bloqueada.
2. A VM que está a ser carregadas podem ter proveniente de uma máquina clonada que não foi processado pelo Sysprep com o Microsoft Monitoring Agent instalado.

#### <a name="resolution"></a>Resolução

1. Visitar [planeamento de rede](../automation-hybrid-runbook-worker.md#network-planning) para saber quais são os endereços e portas têm de ser permitidos para gerenciamento de atualizações trabalhar.
2. Se utilizar uma imagem clonada de sysprep a imagem em primeiro lugar e instalar o agente MMA após o fato.

## <a name="windows"></a>Windows

Se tiver problemas ao tentar carregar a solução numa máquina virtual, verifique os **Operations Manager** registo de eventos na **registos de serviços de aplicações e** no computador local para eventos com ID de evento **4502** e mensagens de evento com **hybridagent**.

A secção seguinte realça as mensagens de erro específicas e uma resolução possível para cada um. Para integração de outra problemas, consulte [resolver problemas de inclusão da solução](onboarding.md).

### <a name="machine-already-registered"></a>Cenário: A máquina já está registada para outra conta

#### <a name="issue"></a>Problema

Receberá a seguinte mensagem de erro:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Causa

A máquina já está incluído para outra área de trabalho para gestão de atualizações.

#### <a name="resolution"></a>Resolução

Executar a limpeza dos artefactos antigos na máquina por [a eliminação do grupo de runbook híbrida](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) e tente novamente.

### <a name="machine-unable-to-communicate"></a>Cenário: A máquina não consegue comunicar com o serviço

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

Pode haver um proxy, gateway ou uma firewall a bloquear a comunicação de rede.

#### <a name="resolution"></a>Resolução

Reveja o seu trabalho em rede e certifique-se de que são permitidos endereços e portas apropriadas. Ver [requisitos de rede](../automation-hybrid-runbook-worker.md#network-planning), para obter uma lista de portas e endereços que são necessários para a gestão de atualizações e os Runbook Workers híbridos.

### <a name="unable-to-create-selfsigned-cert"></a>Cenário: Não é possível criar o certificado autoassinado

#### <a name="issue"></a>Problema

Receber uma das seguintes mensagens de erro:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Causa

A função de trabalho de Runbook híbrida não foi possível gerar um certificado autoassinado

#### <a name="resolution"></a>Resolução

Verifique se conta de sistema tem acesso de leitura à pasta **C:\ProgramData\Microsoft\Crypto\RSA** e tente novamente.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Cenário: Execução de atualização Falha ao iniciar

#### <a name="issue"></a>Problema

Uma falha de execuções de atualização para iniciar uma máquina Linux.

#### <a name="cause"></a>Causa

Função de trabalho do Linux híbrida está danificada.

#### <a name="resolution"></a>Resolução

Faça uma cópia do ficheiro de registo seguinte e preservá-la para fins de resolução de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Cenário: Execução de atualização é iniciada, mas encontra erros

#### <a name="issue"></a>Problema

Uma execução de atualização é iniciada, mas encontra erros durante a execução.

#### <a name="cause"></a>Causa

Foi possível obter as causas possíveis:

* Gestor de pacotes está danificado
* Pacotes específicos podem interferir com a aplicação de patches com base na cloud
* Outras razões

#### <a name="resolution"></a>Resolução

Se ocorrerem falhas durante uma atualização executada depois de iniciar com êxito no Linux, verifique a saída do computador afetado na execução da tarefa. Pode encontrar mensagens de erro específicas do Gestor de pacotes da sua máquina que pode pesquisar e tomar medidas em. Gestão de atualizações requer o Gestor de pacotes ser bom estado de funcionamento para implementações de atualização bem sucedida.

Em alguns casos, as atualizações de pacotes podem interferir com a gestão de atualizações que impede a conclusão de uma implementação de atualização. Se vir, terá de excluir esses pacotes de execuções de atualização futura ou instalá-los manualmente por conta própria.

Se não conseguir resolver um problema de aplicação de patches, fazer uma cópia do ficheiro de registo seguinte e preservar a ele **antes de** a seguinte implementação da atualização é iniciada para fins de resolução de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Passos Seguintes

Se não VI o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.