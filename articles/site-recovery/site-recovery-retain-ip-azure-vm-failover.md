---
title: Manter os endereços IP para ativação pós-falha da VM do Azure | Documentos da Microsoft
description: Descreve como manter os endereços IP quando efetuar a ativação pós-falha de VMs do Azure para recuperação após desastre para uma região secundária com o Azure Site Recovery
ms.service: site-recovery
ms.date: 10/16/2018
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 86adaa21a069c168b512231ba231940bfa2ef9e8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50213037"
---
# <a name="ip-address-retention-for-azure-vm-failover"></a>Retenção de endereço IP para a ativação pós-falha da VM do Azure

O Azure Site Recovery permite a recuperação após desastre para VMs do Azure. Quando efetuar a ativação pós-falha de uma região do Azure para outra, os clientes precisarão da retenção de suas configurações de IP. Recuperação de site, por predefinição, imita rede virtual de origem e a estrutura de sub-rede quando criar esses recursos na região de destino. Para as VMs do Azure configurada com endereços IP privados estáticos, o Site Recovery também torna dentro dos melhor esforços tentar aprovisionar o mesmo IP privado na VM, de destino se este IP já não está bloqueado por um recurso do Azure ou uma VM replicada.

Para aplicativos simples, a configuração predefinida acima é tudo o que é necessário. Para aplicações empresariais mais complexas, os clientes podem ter de aprovisionar recursos de rede adicionais para garantir a conectividade de após a ativação pós-falha com outros componentes de sua infra-estrutura. Este artigo explica os requisitos de rede para ativação pós-falha de VMs do Azure de uma região para outro, mantendo os endereços IP da VM.

## <a name="azure-to-azure-connectivity"></a>Conectividade do Azure para o Azure

Para o primeiro cenário, consideramos **empresa A** que tem todos os sua infra-estrutura de aplicativo em execução no Azure. Por motivos de continuidade e conformidade de negócios, **empresa A** decide usar o Azure Site Recovery para proteger seus aplicativos.

Dado o requisito de retenção IP (por exemplo, para enlaces do aplicativo), a empresa A tem a estrutura de rede e sub-rede virtual mesmo na região de destino. Para reduzir ainda mais o objetivo de tempo de recuperação (RTO), **empresa A** utiliza nós de réplica para o SQL Always ON, controladores de domínio, etc. e estes nós são colocados numa rede virtual diferente na região de destino de réplica. A utilização de um espaço de endereço diferente para os nós de réplica permite **empresa A** para estabelecer a conectividade de site-site VPN entre regiões de origem e destino, que caso contrário, não seria possível se o mesmo espaço de endereços é utilizada em ambas as extremidades .

Eis o aspeto de arquitetura de rede antes da ativação pós-falha:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, utilizando uma rede virtual com endereço espaço 10.1.0.0/16. Esta rede virtual com o nome **VNet de origem**.
- Cargas de trabalho de aplicação são divididas em três sub-redes – 10.1.1.0/24, 10.1.2.0/24, 10.1.3.0/24, com o nome respectivamente **sub-rede 1**, **sub-rede 2**, **sub-rede 3**.
- Azure Sudeste asiático é a região de destino e tem uma rede virtual de recuperação que simule a configuração de espaço e a sub-rede do endereço na origem. Esta rede virtual com o nome **VNet de recuperação**.
- Nós de réplica, como os que necessitam de Always On, controlador de domínio, etc. são colocados numa rede virtual com endereço espaço 10.2.0.0/16 dentro de 4 de sub-rede com o endereço 10.2.4.0/24. A rede virtual com o nome **VNet do Azure** e está no Azure Sudeste asiático.
- **VNet de origem** e **VNet do Azure** estão ligados através de conectividade da VPN site a site.
- **VNet de recuperação** não está ligado com qualquer outra rede virtual.
- **Da empresa A** atribui/verifica o endereço IP de destino para itens replicados. Neste exemplo, o IP de destino é o mesmo que o IP de origem para cada VM.

![Conectividade do Azure para o Azure antes da ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Ativação pós-falha de região completo

Em caso de uma falha regional, **empresa** pode recuperar seu toda a implementação rápida e facilmente com o Azure Site Recovery poderosas [planos de recuperação](site-recovery-create-recovery-plans.md). Já ter configurado o endereço IP de destino para cada VM antes da ativação pós-falha, **empresa A** pode coordenar a ativação pós-falha e automatizar estabelecimento da conexão entre a VNet de recuperação e a Vnet do Azure, como mostra o diagrama abaixo.

![Ativação pós-falha de região completa de conectividade do Azure para o Azure](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)

Consoante os requisitos de aplicações, ligações entre as duas VNets na região de destino podem ser estabelecido antes, durante a (como passo intermédio) ou após a ativação pós-falha. Uso [planos de recuperação](site-recovery-create-recovery-plans.md) para adicionar scripts e definir a ordem de ativação pós-falha.

A empresa A tem também a opção de usar o peering de VNet ou VPN de Site a Site para estabelecer a conectividade entre a VNet de recuperação e a VNet do Azure. O VNet peering não utiliza um gateway de VPN e tem restrições de diferentes. Além disso, o [preço do VNet peering](https://azure.microsoft.com/pricing/details/virtual-network) é calculado de forma diferente que os [preços dos Gateways VPN de VNet a VNet](https://azure.microsoft.com/pricing/details/vpn-gateway). Para as ativações pós-falha, é geralmente recomendado para imitar a conectividade de origem, incluindo o tipo de ligação, para minimizar imprevisíveis incidentes que advenham do alterações de rede.

### <a name="isolated-application-failover"></a>Ativação pós-falha de aplicação isolado

Sob determinadas condições, os utilizadores poderão precisar a partes de ativação pós-falha de sua infra-estrutura de aplicativo. Um exemplo está a falhar ao longo de uma aplicação específica ou escalão que é hospedado dentro de uma sub-rede dedicada. Embora seja possível uma ativação pós-falha de sub-rede com retenção IP, não é recomendado para a maioria das situações, à medida que ele aumenta substancialmente inconsistências de conectividade. Também irá perder a conectividade de sub-rede para outras sub-redes dentro da mesma rede virtual do Azure.

Uma maneira melhor para levar em conta os requisitos de ativação pós-falha do aplicativo de nível de sub-rede é utilizar endereços IP de destino diferente para a ativação pós-falha (se a conectividade é necessária para outras sub-redes na rede virtual de origem) ou isolar cada aplicativo em seu próprio dedicado virtual rede na origem. Com a última abordagem pode estabelecer conectividade de rede entre a origem e emular o mesmo quando efetuar a ativação pós-falha para a região de destino.

Projetar aplicativos individuais para resiliência, recomenda-se para hospedar um aplicativo na sua própria rede virtual dedicado e estabelecer a conectividade entre estas redes virtuais conforme necessário. Assim, para ativação pós-falha de aplicação isolado, mantendo o originais endereços IP privados.

A configuração de ativação pós-falha, em seguida, será semelhante ao seguinte:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, utilizando uma rede virtual do Azure com o espaço de endereço 10.1.0.0/16 para a primeira aplicação e 10.2.0.0/16 para o segundo aplicativo. As redes virtuais são nomeadas **VNet1 de origem** e **origem VNet2** para o aplicativo de primeiro e segundo, respetivamente.
- Cada VNet adicional é dividido em duas sub-redes.
- Azure Sudeste asiático é a região de destino e tem redes virtuais de recuperação VNet1 de recuperação e VNet2 de recuperação.
- Nós de réplica, como os que necessitam de Always On, controlador de domínio, etc. são colocados numa rede virtual com o espaço de endereço 10.3.0.0/16 dentro **sub-rede 4** com endereço 10.3.4.0/24. A rede virtual é chamada de VNet do Azure e está no Azure Sudeste asiático.
- **Origem VNet1** e **VNet do Azure** estão ligados através de conectividade da VPN site a site. Da mesma forma, **origem VNet2** e **VNet do Azure** também estão ligados através de conectividade da VPN site a site.
- **Origem VNet1** e **origem VNet2** , neste exemplo, também estão ligados através de S2S VPN. Uma vez que são as duas VNets na mesma região, o VNet peering também pode ser utilizado em vez de S2S VPN.
- **Recuperação VNet1** e **recuperação VNet2** não estão ligados com qualquer outra rede virtual.
- Para reduzir o objetivo de tempo de recuperação (RTO), os gateways de VPN são configurados no **recuperação VNet1** e **recuperação VNet2** antes da ativação pós-falha.

![Aplicação de conectividade do Azure para o Azure isolada antes de ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

Em caso de uma situação de desastre que afeta apenas um aplicativo (neste exemplo hospedado em VNet2 de origem), empresa pode recuperar o aplicativo afetado da seguinte forma:
- Ligações de VPN entre **origem VNet1** e **origem VNet2**e entre **origem VNet2** e **VNet do Azure** está ligado à Internet.
- São estabelecidas ligações de VPN entre **origem VNet1** e **recuperação VNet2**e entre **recuperação VNet2** e **VNet do Azure**.
- VMs a partir de **origem VNet2** efetuar a ativação pós-falha para **recuperação VNet2**.

![Aplicação de conectividade do Azure para o Azure isolada após a ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)

O acima de ativação pós-falha isolada exemplo pode ser expandido para incluir mais aplicações e ligações de rede. Recomenda-se seguir um modelo de como o tipo de ligação, máximo possível, quando efetuar a ativação pós-falha de origem ao destino.

### <a name="further-considerations"></a>Considerações finais

Gateways de VPN utilizam endereços IP públicos e saltos de gateway para estabelecer ligações. Se não pretender utilizar o IP público, e/ou quiser evitar saltos Extras, pode utilizar o Azure [peering de rede Virtual](../virtual-network/virtual-network-peering-overview.md) para configurar o peering entre redes virtuais entre [regiões do Azure suportadas](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="on-premises-to-azure-connectivity"></a>Conectividade no local-para-Azure

Para o segundo cenário, consideramos **empresa B** que tem uma parte da sua infraestrutura de aplicativo em execução no Azure e o resto em execução no local. Por motivos de continuidade e conformidade de negócios, **empresa B** decide usar o Azure Site Recovery para proteger seus aplicativos em execução no Azure.

Eis o aspeto de arquitetura de rede antes da ativação pós-falha:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, utilizando uma rede virtual com endereço espaço 10.1.0.0/16. Esta rede virtual com o nome **VNet de origem**.
- Cargas de trabalho de aplicação são divididas em três sub-redes – 10.1.1.0/24, 10.1.2.0/24, 10.1.3.0/24, com o nome respectivamente **sub-rede 1**, **sub-rede 2**, **sub-rede 3**.
- Azure Sudeste asiático é a região de destino e tem uma rede virtual de recuperação que simule a configuração de espaço e a sub-rede do endereço na origem. Esta rede virtual com o nome **VNet de recuperação**.
- As VMs no Azure Ásia Oriental estão ligadas ao centro de dados no local através do ExpressRoute ou VPN de Site a Site.
- Para reduzir o objetivo de tempo de recuperação (RTO), a empresa B Aprovisiona gateways de VNet de recuperação no Azure Sudeste asiático antes da ativação pós-falha.
- **Empresa B** atribui/verifica o endereço IP de destino para itens replicados. Neste exemplo, o IP de destino é o mesmo que o IP de origem para cada VM

![Conectividade no local-para-Azure antes da ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Ativação pós-falha de região completo

Em caso de uma falha regional, **empresa B** pode recuperar seu toda a implementação rápida e facilmente com o Azure Site Recovery poderosas [planos de recuperação](site-recovery-create-recovery-plans.md). Já ter configurado o endereço IP de destino para cada VM antes da ativação pós-falha, **B de empresa** pode coordenar a ativação pós-falha e automatizar estabelecimento da conexão entre a VNet de recuperação e o datacenter no local, como mostra o diagrama abaixo.

A ligação original entre Azure Ásia Oriental e o datacenter no local deve ser desligada antes de estabelecer a ligação entre o Azure Sudeste asiático e o datacenter no local. O encaminhamento no local também é reconfigurados para apontar para a região de destino e gateways após a ativação pós-falha.

![Conectividade no local-para-Azure após a ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

### <a name="subnet-failover"></a>Ativação pós-falha de sub-rede

Ao contrário do cenário do Azure para o Azure descrito para **empresa**, uma ativação pós-falha de nível de sub-rede não é possível neste caso para **empresa B**. Isto acontece porque o espaço de endereços em redes virtuais de origem e de recuperação é a mesma e a origem original para a ligação no local está ativa.

Para obter resiliência de aplicação, recomenda-se que cada aplicativo está hospedado em sua própria rede virtual do Azure dedicado. Aplicativos podem, em seguida, efetuar a ativação pós-falha em isolamento e no local necessária para ligações de origem pode ser encaminhado para a região de destino, conforme descrito acima.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md).
