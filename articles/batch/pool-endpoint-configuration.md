---
title: "Configurar pontos finais de nó no conjunto do Azure Batch | Microsoft Docs"
description: "Como configurar ou desativar o acesso ao SSH ou RDP portas em nós de computação num conjunto do Azure Batch."
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: danlep
ms.openlocfilehash: fdc68744406c3e995a2764f93d4474b807337ff5
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Configurar ou desativar o acesso remoto a nós num conjunto do Azure Batch de computação

Por predefinição, o Batch permite um [utilizador nó](/rest/api/batchservice/computenode/adduser) com conectividade de rede para ligar externamente para um nó de computação no conjunto do Batch. Por exemplo, um utilizador poder ligar ao remoto ambiente de trabalho (RDP) na porta 3389 para um nó de computação num conjunto do Windows. Da mesma forma, por predefinição, um utilizador poder ligar ao Secure Shell (SSH) na porta 22 para um nó de computação num conjunto de Linux. 

No seu ambiente, poderá ter de restringir ou desativar estas predefinições de acesso externo. Pode modificar estas definições, utilizando as APIs do Batch para definir o [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) propriedade. 

## <a name="about-the-pool-endpoint-configuration"></a>Sobre a configuração de ponto final do agrupamento
A configuração de ponto final é composta por um ou mais [conjuntos de tradução (NAT) de endereços de rede](/rest/api/batchservice/pool/add#inboundnatpool) de portas front-end. (Não confunda um conjunto NAT com o conjunto de nós de computação do Batch.) Configurar cada conjunto NAT para substituir as definições de ligação predefinido em nós de computação do conjunto. 

Cada configuração de conjunto NAT inclui um ou mais [regras de segurança de grupo (NSG) de rede](/rest/api/batchservice/pool/add#networksecuritygrouprule). Cada regra NSG permite ou nega determinado tráfego de rede para o ponto final. Pode optar por permitir ou negar todo o tráfego, tráfego identificado por um [etiqueta predefinida](../virtual-network/virtual-networks-nsg.md#default-tags) (por exemplo, "Internet"), ou tráfego de endereços IP ou sub-redes específicas.

### <a name="considerations"></a>Considerações
* A configuração de ponto final do conjunto aplicacional faz parte do conjunto [configuração de rede](/rest/api/batchservice/pool/add#NetworkConfiguration). A configuração de rede, opcionalmente, pode incluir as definições de associação do agrupamento para um [rede virtual do Azure](batch-virtual-network.md). Se configurar o agrupamento de uma rede virtual, pode criar regras NSG que utilizem as definições de endereço na rede virtual.
* Pode configurar várias regras NSG quando configurar um conjunto NAT. As regras são verificadas por ordem de prioridade. Quando for aplicada uma regra, não são testadas mais regras para correspondência.


## <a name="example-deny-all-rdp-traffic"></a>Exemplo: Todo o tráfego RDP de negação

C# fragmento seguinte mostra como configurar o ponto final RDP em nós de computação num conjunto Windows para negar todo o tráfego de rede. O ponto final utiliza um conjunto de front-end de portas no intervalo *60000 60099*. 

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
      new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 60000, 60099, new NetworkSecurityGroupRule[]
        {
            new NetworkSecurityGroupRule(162, NetworkSecurityGroupRuleAccess.Deny, "*"),
        })
    })    
};
```

## <a name="example-deny-all-ssh-traffic-from-the-internet"></a>Exemplo: Negar SSH todo o tráfego da internet

O fragmento de Python seguinte mostra como configurar o ponto final SSH em nós de computação num conjunto Linux para negar todo o tráfego de internet. O ponto final utiliza um conjunto de front-end de portas no intervalo *4000 4100*. 

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access=batchmodels.NetworkSecurityGroupRuleAccess.deny,
                source_address_prefix='Internet'
                )
            ]
        )
        ]
    ) 
)
```

## <a name="example-allow-rdp-traffic-from-a-specific-ip-address"></a>Exemplo: Permitir tráfego RDP de um endereço IP específico

C# fragmento seguinte mostra como configurar o ponto final RDP em nós de computação num conjunto Windows para permitir o acesso RDP apenas a partir do endereço IP *198.51.100.7*. A segunda regra NSG nega o tráfego que não corresponde ao endereço IP.

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
        new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 7500, 8000, new NetworkSecurityGroupRule[]
        {   
            new NetworkSecurityGroupRule(179,NetworkSecurityGroupRuleAccess.Allow, "198.51.100.7"),
            new NetworkSecurityGroupRule(180,NetworkSecurityGroupRuleAccess.Deny, "*")
        })
    })    
};
```

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Exemplo: Permitir o tráfego SSH de sub-rede específica

O fragmento de Python seguinte mostra como configurar o ponto final SSH em nós de computação num conjunto Linux para permitir o acesso apenas a partir da sub-rede *192.168.1.0/24*. A segunda regra NSG nega o tráfego que não corresponde à sub-rede.

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access='allow',
                source_address_prefix='192.168.1.0/24'
                ),
                batchmodels.NetworkSecurityGroupRule(
                priority=175,
                access='deny',
                source_address_prefix='*'
                )
            ]
        )
        ]
    )
)
```

## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre regras NSG no Azure, consulte [filtrar o tráfego de rede com grupos de segurança de rede](../virtual-network/virtual-networks-nsg.md).

- Para uma descrição geral aprofundada do Batch, consulte [paralelo em grande escala desenvolver soluções com o Batch de computação](batch-api-basics.md).

