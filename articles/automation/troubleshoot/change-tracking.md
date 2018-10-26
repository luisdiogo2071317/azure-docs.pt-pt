---
title: Resolução de problemas com o controlo de alterações do Azure
description: Este artigo fornece informações sobre resolução de problemas de controlo de alterações
services: automation
ms.service: automation
ms.component: change-tracking
author: georgewallace
ms.author: gwallace
ms.date: 10/24/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2f2e66a1fab31ce6099ab426d6e8ce144e155efb
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088877"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Resolver problemas de controlo de alterações e inventário

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Cenário: Registos de controlo de alterações não apresentarem no portal do Azure

#### <a name="issue"></a>Problema

Não vê quaisquer resultados de inventário ou de controlo de alterações para as máquinas que são carregadas para controlo de alterações.

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. O **Microsoft Monitoring Agent** não está em execução
2. Comunicação de volta para a conta de automatização está a ser bloqueada.
3. Os pacotes de gestão para controlo de alterações não são transferidos.
4. A VM que se pretende incluir talvez proveniente de uma máquina clonada que não foi processado pelo Sysprep com o Microsoft Monitoring Agent instalado.

#### <a name="resolution"></a>Resolução

1. Verifique se o **Microsoft Monitoring Agent** (HealthService.exe) está em execução na máquina.
2. Visitar [planeamento de rede](../automation-hybrid-runbook-worker.md#network-planning) para saber quais são os endereços e portas têm de ser permitidos para controlo de alterações trabalhar.
3. Certifique-se de que os seguintes pacotes de gestão do controlo de alterações e inventário existem localmente:
    * Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
    * Microsoft.IntelligencePacks.InventoryChangeTracking.*
    * Microsoft.IntelligencePacks.SingletonInventoryCollection.*
4. Se utilizar uma imagem clonada de sysprep a imagem em primeiro lugar e instalar o agente do Microsoft Monitoring Agent após o fato.

Se essas soluções não resolverem o problema e contacte o suporte, pode executar os comandos seguintes para recolher o diagnóstico no agente

No computador agente, navegue para `C:\Program Files\Microsoft Monitoring Agent\Agent\Tools` e execute os seguintes comandos:

```cmd
set stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Devido a erro de predefinição o rastreio está ativado, se quiser permitir mensagens de erro detalhado como no exemplo anterior, utilize `VER` parâmetro. Para que os rastreios de informações, utilize `INF` ao invocar `StartTracing.cmd`.

## <a name="next-steps"></a>Passos Seguintes

Se não vir o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.