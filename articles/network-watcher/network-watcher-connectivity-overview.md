---
title: Resolver problemas de introdução à ligação de observador de rede do Azure | Microsoft Docs
description: Esta página fornece uma descrição geral da capacidade de resolução de problemas de ligação de observador de rede
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 0268c7e54aa82df12243f98fd72de836fbc82070
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30834004"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Introdução à ligação de resolução de problemas no observador de rede do Azure

Resolver problemas relacionados com a ligação a funcionalidade de observador de rede fornece a capacidade para verificar uma ligação de TCP direta de uma máquina virtual a uma máquina virtual (VM), o nome de domínio completamente qualificado (FQDN), URI, ou o endereço IPv4. Cenários de rede são complexos, estão implementados utilizando grupos de segurança de rede, firewalls, rotas definidas pelo utilizador e de recursos fornecidos pelo Azure. Configurações complexas efetua a resolução de problemas de conectividade um desafio. Observador de rede ajuda a reduzir a quantidade de tempo para localizar e detetar problemas de conectividade. Os resultados devolvidos podem fornecer informações sobre se o problema de conectividade é devido a uma plataforma ou um problema de configuração do utilizador. Conectividade pode ser verificada com [PowerShell](network-watcher-connectivity-powershell.md), [CLI do Azure](network-watcher-connectivity-cli.md), e [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Resolver problemas de ligação requer que a VM resolver problemas de tenha a `AzureNetworkWatcherExtension` extensão da VM instalado. Para instalar a extensão numa Windows VM visite [extensão da máquina virtual de agente de observador de rede do Azure para Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e para, visite VM com Linux [extensão da máquina virtual de agente de observador de rede do Azure para Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). A extensão não é necessário no ponto final de destino.

## <a name="response"></a>Resposta

A tabela seguinte mostra as propriedades devolvidas quando resolver problemas de ligação concluiu a execução.

|Propriedade  |Descrição  |
|---------|---------|
|ConnectionStatus     | O estado da verificação de conectividade. São possíveis resultados **Reachable** e **Unreachable**.        |
|AvgLatencyInMs     | Latência média de durante a verificação de conectividade em milissegundos. (Apenas apresentar se verifique o estado é acessível)        |
|MinLatencyInMs     | Latência mínima durante a verificação de conectividade em milissegundos. (Apenas apresentar se verifique o estado é acessível)        |
|MaxLatencyInMs     | Latência máxima durante a verificação de conectividade em milissegundos. (Apenas apresentar se verifique o estado é acessível)        |
|ProbesSent     | Número de sondas enviada durante a verificação. O valor máximo é 100.        |
|ProbesFailed     | Número de sondas que falharam durante a verificação. O valor máximo é 100.        |
|Saltos     | Salto pelo caminho de salto de origem para destino.        |
|Hops[].Type     | Tipo de recurso. Os valores possíveis são **origem**, **VirtualAppliance**, **VnetLocal**, e **Internet**.        |
|Hops[].Id | Identificador exclusivo do salto.|
|Hops[].Address | Endereço IP do salto.|
|Hops[].ResourceId | ResourceID do salto se o salto é um recurso do Azure. Se se tratar de um recurso de internet, ResourceID é **Internet**. |
|Hops[].NextHopIds | O identificador exclusivo do próximo salto executado.|
|Hops[].Issues | Uma coleção de problemas encontrados durante a verificação em que hop. Se ocorreram sem problemas, o valor está em branco.|
|Hops[].Issues[].Origin | Em salto atual, em que ocorreu o problema. Os valores possíveis são:<br/> **Entrada** -problema está na ligação do salto anterior para o salto atual<br/>**Saída** -problema está na ligação do salto atual para o salto seguinte<br/>**Local** -problema está no salto atual.|
|Hops[].Issues[].Severity | A gravidade do problema detetado. Os valores possíveis são **erro** e **aviso**. |
|Hops[].Issues[].Type |O tipo de problema encontrado. Os valores possíveis são: <br/>**CPU**<br/>**Memória**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hops[].Issues[].Context |Detalhes sobre o problema encontrado.|
|Hops[].Issues[].Context[].key |Chave do par chave-valor devolvido.|
|Hops[].Issues[].Context[].value |Valor do par chave-valor devolvido.|

Segue-se um exemplo de um problema encontrado um salto.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Tipos de falhas

Ligação resolver tipos de falhas de devolve sobre a ligação. A tabela seguinte fornece uma lista dos tipos de falhas atual devolvido.

|Tipo  |Descrição  |
|---------|---------|
|CPU     | Elevada utilização da CPU.       |
|Memória     | Utilização elevada da memória.       |
|GuestFirewall     | O tráfego está bloqueado devido a uma configuração de firewall de máquina virtual.        |
|DNSResolution     | Falha na resolução DNS para o endereço de destino.        |
|NetworkSecurityRule    | O tráfego está bloqueado por uma regra de NSG (regra será devolvida)        |
|UserDefinedRoute|O tráfego é removido devido a um definidas pelo utilizador ou a rota de sistema. |

### <a name="next-steps"></a>Passos Seguintes

Saiba como resolver problemas de ligações utilizando o [portal do Azure](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), a [CLI do Azure](network-watcher-connectivity-cli.md), ou [REST API](network-watcher-connectivity-rest.md).