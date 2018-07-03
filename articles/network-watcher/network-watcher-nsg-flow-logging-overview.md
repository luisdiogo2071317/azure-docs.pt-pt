---
title: Introdução ao registo do fluxo para segurança de rede de grupos com o observador de rede do Azure | Documentos da Microsoft
description: Este artigo explica como utilizar a funcionalidade de registos de fluxo NSG do observador de rede do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: ae4edb82fa5e192a30d297dae82199bb7efca0c2
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344976"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Introdução ao registo do fluxo para grupos de segurança de rede

Registos de fluxo de grupo (NSG) de segurança de rede são uma funcionalidade do observador de rede permite-lhe ver informações sobre o tráfego IP de entrada e de saída através de um NSG. Registos de fluxo são escritos no formato json e mostram fluxos de saída e entrados numa base por regra, o fluxo de mensagens em fila aplica-se a interface de rede (NIC), informações de 5 cadeias de identificação sobre o fluxo (IP de origem/destino, porta de origem/destino e protocolo), e se o tráfego foi permitido ou negado.

![Descrição geral de registos de fluxo](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Embora os registos de fluxo NSGs de destino, não são apresentados os mesmos como os outros registos. Os registos de fluxo são armazenados apenas dentro de uma conta de armazenamento e siga o caminho de registo mostrado no exemplo a seguir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

As mesmas políticas de retenção vistas para outros registos aplicam-se aos registos de fluxo. Pode definir a política de retenção do registo de 1 dia para dias 2147483647. Se não definir uma política de retenção, os registos são mantidos para sempre.

## <a name="log-file"></a>Ficheiro de registo

Os registos de fluxo incluem as seguintes propriedades:

* **tempo** - hora a que o evento foi registado
* **systemId** -ID de recurso do grupo de segurança de rede
* **categoria** -a categoria do evento. A categoria é sempre **NetworkSecurityGroupFlowEvent**
* **ResourceId** -o Id de recurso do NSG
* **operationName** -NetworkSecurityGroupFlowEvents sempre
* **propriedades** -uma coleção de propriedades do fluxo
    * **Versão** -número de versão do esquema de eventos de registo de fluxo
    * **fluxos** -uma coleção de fluxos. Esta propriedade tem múltiplas entradas para diferentes regras
        * **regra** -da regra para que os fluxos são listados
            * **fluxos** -uma coleção de fluxos
                * **Mac** -endereço do MAC da NIC da VM em que o fluxo foi recolhido
                * **flowTuples** -uma cadeia que contém várias propriedades para a cadeia de identificação de fluxo no formato separados por vírgulas
                    * **Carimbo de hora** -este valor é o carimbo de data / hora de quando o fluxo ocorreu no formato da ÉPOCA do UNIX
                    * **IP de origem** -o IP de origem
                    * **Destino IP** -o IP de destino
                    * **Porta de origem** -a porta de origem
                    * **Porta de destino** -a porta de destino
                    * **Protocolo** -o protocolo do fluxo. Os valores válidos são **T** para TCP e **U** para UDP
                    * **Fluxo de tráfego** -a direção do fluxo de tráfego. Os valores válidos são **eu** de entrada e **s** para saída.
                    * **Tráfego** - se o tráfego foi permitido ou negado. Os valores válidos são **uma** para permitido e **1!d** para negado.

O texto que se segue é um exemplo de um registo de fluxo. Como pode ver, existem vários registos que se seguem a lista de propriedades descrita na secção anterior.

> [!NOTE]
> Os valores no **flowTuples* propriedade são uma lista separada por vírgulas.
 
```json
{
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Passos Seguintes

- Para saber como ativar os registos de fluxo, veja [registo do fluxo do NSG ativar](network-watcher-nsg-flow-logging-portal.md).
- Para saber mais sobre o registo de NSG, consulte [do Log analytics para grupos de segurança de rede (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Para determinar se o tráfego é permitido ou negado para ou a partir de uma VM, veja [diagnosticar um problema de filtragem de tráfego de rede VM](diagnose-vm-network-traffic-filtering-problem.md)
