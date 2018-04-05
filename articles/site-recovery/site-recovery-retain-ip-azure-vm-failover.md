---
title: Manter os endereços IP quando efetuar a ativação pós-falha de máquinas virtuais do Azure para outra região do Azure | Microsoft Docs
description: Descreve como manter os endereços IP para cenários de ativação pós-falha do Azure para o Azure com o Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 8e128e057e45f6966067ebaaf039d9b14349d926
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="ip-address-retention-for-azure-virtual-machine-failover"></a>Retenção do endereço IP de ativação pós-falha da máquina virtual do Azure

O Azure Site Recovery permite a recuperação de desastre para as VMs do Azure. Quando efetuar a ativação pós-falha de uma região do Azure para outro, os clientes, muitas vezes, necessitam de retenção das respetivas configurações de IP. Recuperação de site, por predefinição, mimics rede virtual de origem e a estrutura de sub-rede durante a criação destes recursos na região de destino. Para as VMs do Azure configurada com endereços IP privados estáticos, a recuperação de sites também facilita a um melhor esforço, a tentativa de aprovisionar o mesmo IP privado no destino VM, se essa IP já não está bloqueado por um recurso do Azure ou numa VM replicada.

Para aplicações simples, a configuração predefinida acima é tudo o que é necessária. Para aplicações empresariais mais complexas, os clientes poderão ter de aprovisionar os recursos de rede adicionais para garantir a conectividade de pós-falha com outros componentes da respetiva infraestrutura. Este artigo explica os requisitos de rede para falhar através de VMs do Azure de uma região para outro, mantendo o endereços IP da VM.

## <a name="azure-to-azure-connectivity"></a>Conectividade do Azure para o Azure

Para o primeiro cenário, considere **empresa um** com toda a infraestrutura de aplicação em execução no Azure. Por motivos de continuidade e compatibilidade de negócio, **empresa um** decide utilizar o Azure Site Recovery para proteger as suas aplicações.

Tendo em conta o requisito de retenção IP (como nos enlaces de aplicação), a empresa um tem a estrutura de rede e sub-rede virtual mesma na região de destino. Para reduzir o objetivo de tempo de recuperação (RTO), **empresa um** utiliza nós de réplica para o SQL Always ON, controladores de domínio, etc. e estes réplica nós são colocados numa rede virtual diferente na região de destino. A utilização de um espaço de endereço diferente para os nós de réplica permite **empresa um** para estabelecer conectividade a VPN site a site entre regiões de origem e de destino, que caso contrário, não seria possível se o mesmo espaço de endereços é utilizada em ambas as extremidades .

Eis o aspeto a arquitetura da rede antes da ativação pós-falha:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, a utilização de uma rede virtual do Azure com o endereço espaço 10.1.0.0/16. Esta rede virtual é denominada **origem VNet**.
- Cargas de trabalho de aplicação estão divididas em três sub-redes – 10.1.0.0/24, 10.1.1.0/24, 10.1.2.0/24, respetivamente com o nome **sub-rede 1**, **sub-rede 2**, **sub-rede 3**.
- Sudeste asiático do Azure é a região de destino e tem uma rede virtual de recuperação que mimics a configuração de espaço e a sub-rede de endereços na origem. Esta rede virtual é denominada **recuperação VNet**.
- Nós de réplica, tais como as necessárias para Always On, controlador de domínio, etc. são colocados numa rede virtual com endereço espaço 20.1.0.0/16 dentro de 4 de sub-rede com o endereço 20.1.0.0/24. A rede virtual é denominada **Azure VNet** e está no Azure Sudeste asiático.
- **Origem de VNet** e **Azure VNet** estão ligados através da conectividade a VPN site a site.
- **Recuperação VNet** não está ligado com qualquer outra rede virtual.
- **Da empresa A** atribui/verifica o endereço IP de destino para itens replicados. Neste exemplo, IP de destino é igual ao IP de origem para cada VM.

![Conectividade do Azure para o Azure antes de ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Ativação pós-falha de região completa

Em caso de uma falha regional, **empresa um** pode recuperar a sua implementação completa rápida e facilmente utilizando o Azure Site Recovery powerful [planos de recuperação](site-recovery-create-recovery-plans.md). Já ter configurado o endereço IP de destino para cada VM antes da ativação pós-falha, **empresa um** pode orquestrar ativação pós-falha e automatizar estabelecimento da ligação entre a VNet de recuperação e a Vnet do Azure, conforme mostrado no diagrama abaixo.

![Ativação pós-falha de região completo de conectividade do Azure para o Azure](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover.png)

Dependendo dos requisitos da aplicação, ligações entre as duas VNets na região de destino podem ser estabelecida antes, durante a (como passo intermédio) ou após a ativação pós-falha. Utilize [planos de recuperação](site-recovery-create-recovery-plans.md) para adicionar scripts e definir a ordem de ativação pós-falha.

A empresa um também tem a opção de utilizar o VNet peering ou VPN Site a Site para estabelecer conectividade entre a VNet de recuperação e a VNet do Azure. O VNet peering não utiliza um gateway de VPN e tem restrições de diferentes. Além disso, o [preço do VNet peering](https://azure.microsoft.com/pricing/details/virtual-network) é calculado de forma diferente que os [preços dos Gateways VPN de VNet a VNet](https://azure.microsoft.com/pricing/details/vpn-gateway). Para as ativações pós-falha, é, geralmente, recomendado para imitar a conectividade da origem, incluindo o tipo de ligação, para minimizar a incidentes imprevisíveis resultantes sem alterações de rede.

### <a name="isolated-application-failover"></a>Ativação pós-falha de aplicação isolado

Em determinadas condições, os utilizadores poderão ter de partes de ativação pós-falha da respetiva infraestrutura de aplicação. Um exemplo desse tipo é efetuar a ativação pós-falha de uma aplicação específica ou camadas que estão alojada numa sub-rede dedicada. Embora seja possível uma ativação pós-falha de sub-rede com retenção IP, não é recomendado para a maioria das situações, como aumenta substancialmente inconsistências de conectividade. Também irá perder a conectividade de sub-rede a outras sub-redes na mesma rede virtual do Azure.

É uma melhor forma de conta para os requisitos de ativação pós-falha de aplicação de nível de sub-rede a utilizar endereços IP de destino diferente para ativação pós-falha (se a conectividade for necessária a outras sub-redes na rede virtual de origem) ou isolar cada aplicação na sua própria dedicado virtual rede de origem. Com a última abordagem pode estabelecer conectividade de rede entre a origem e emular o mesmo quando efetuar a ativação pós-falha para a região de destino.

Para aplicações individuais para resiliência de architect, recomenda-alojam uma aplicação na sua própria rede virtual dedicado e estabelecer conectividade entre estas redes virtuais conforme necessário. Isto permite a ativação pós-falha de aplicação isolado, mantendo o originais endereços IP privados.

A configuração de pré-ativação pós-falha, em seguida, procura da seguinte forma:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, a utilização de uma rede virtual do Azure com o espaço de endereço 10.1.0.0/16 para a primeira aplicação e 15.1.0.0/16 para a aplicação de segundo. As redes virtuais são denominadas **origem VNet1** e **origem VNet2** para a aplicação primeira e segunda, respetivamente.
- Cada VNet adicional é dividido em duas sub-redes.
- Sudeste asiático do Azure é a região de destino e tem redes virtuais de recuperação VNet1 de recuperação e VNet2 de recuperação.
- Nós de réplica, tais como as necessárias para Always On, controlador de domínio, etc. são colocados numa rede virtual com o espaço de endereço 20.1.0.0/16 dentro **sub-rede 4** com 20.1.0.0/24 endereço. A rede virtual é denominada VNet do Azure e está no Azure Sudeste asiático.
- **Origem VNet1** e **Azure VNet** estão ligados através da conectividade a VPN site a site. Da mesma forma, **origem VNet2** e **Azure VNet** também estão ligados através da conectividade a VPN site a site.
- **Origem VNet1** e **origem VNet2** , neste exemplo, também estão ligados através de S2S VPN. Uma vez que as duas VNets estiverem na mesma região, o VNet peering também pode ser utilizado em vez de uma VPN S2S.
- **Recuperação VNet1** e **recuperação VNet2** não estão ligados com qualquer outra rede virtual.
- Para reduzir o objetivo de tempo de recuperação (RTO), gateways de VPN são configurados no **recuperação VNet1** e **recuperação VNet2** antes da ativação pós-falha.

![Aplicação de conectividade do Azure para o Azure isolada antes da ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover.png)

Em caso de uma situação de desastres que afeta apenas uma aplicação (neste exemplo alojado em VNet2 de origem), empresa um pode recuperar a aplicação afetada da seguinte forma:
- Ligações de VPN entre **origem VNet1** e **origem VNet2**e entre **origem VNet2** e **VNet do Azure** estiver desligado.
- Ligações VPN são estabelecidas entre **origem VNet1** e **recuperação VNet2**e entre **recuperação VNet2** e **VNet do Azure**.
- VMs de **origem VNet2** pós-falha para **recuperação VNet2**.

![Aplicação de conectividade do Azure para o Azure isolada após a ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover.png)

O acima ativação pós-falha isolado exemplo pode ser expandido para incluir mais aplicações e ligações de rede. Recomenda-se a seguir um modelo de como o tipo de ligação, as far as possível, ao efetuar a ativação pós-falha de origem para destino.

### <a name="further-considerations"></a>Considerações adicionais

Gateways de VPN utilizar endereços IP públicos e saltos de gateway para estabelecer ligações. Se não pretender utilizar o IP público, e/ou pretende evitar saltos adicionais, pode utilizar o Azure [peering de rede Virtual](../virtual-network/virtual-network-peering-overview.md) a redes virtuais através de ponto [regiões do Azure suportadas](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="on-premises-to-azure-connectivity"></a>Conectividade no local-para o Azure

Para o segundo cenário, considere **empresa B** que tem uma parte da respetiva infraestrutura de aplicação em execução no Azure e o resto em execução no local. Por motivos de continuidade e compatibilidade de negócio, **empresa B** decide utilizar o Azure Site Recovery para proteger as suas aplicações em execução no Azure.

Eis o aspeto a arquitetura da rede antes da ativação pós-falha:
- VMs de aplicação estão alojadas no Azure Ásia Oriental, a utilização de uma rede virtual do Azure com o endereço espaço 10.1.0.0/16. Esta rede virtual é denominada **origem VNet**.
- Cargas de trabalho de aplicação estão divididas em três sub-redes – 10.1.0.0/24, 10.1.1.0/24, 10.1.2.0/24, respetivamente com o nome **sub-rede 1**, **sub-rede 2**, **sub-rede 3**.
- Sudeste asiático do Azure é a região de destino e tem uma rede virtual de recuperação que mimics a configuração de espaço e a sub-rede de endereços na origem. Esta rede virtual é denominada **recuperação VNet**.
- VMs no Azure Oriental estão ligadas ao centro de dados no local através do ExpressRoute ou VPN de Site para Site.
- Para reduzir o objetivo de tempo de recuperação (RTO), a empresa B Aprovisiona gateways vnet de recuperação no Azure Sudeste asiático antes da ativação pós-falha.
- **Empresa B** atribui/verifica o endereço IP de destino para itens replicados. Neste exemplo, IP de destino é igual ao IP de origem para cada VM

![Conectividade no local-para o Azure antes de ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Ativação pós-falha de região completa

Em caso de uma falha regional, **empresa B** pode recuperar a sua implementação completa rápida e facilmente utilizando o Azure Site Recovery powerful [planos de recuperação](site-recovery-create-recovery-plans.md). Já ter configurado o endereço IP de destino para cada VM antes da ativação pós-falha, **empresa B** pode orquestrar ativação pós-falha e automatizar estabelecimento da ligação entre a VNet de recuperação e datacenter no local, conforme mostrado no diagrama abaixo.

A ligação original entre Oriental do Azure e o Centro de dados no local deve ser interrompida antes de estabelecer a ligação entre o Azure Sudeste asiático e datacenter no local. O local encaminhamento também é reconfigurados para apontar para a região de destino e gateways após a ativação pós-falha.

![Conectividade no local-para o Azure após a ativação pós-falha](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover.png)

### <a name="subnet-failover"></a>Ativação pós-falha de sub-rede

Ao contrário do cenário do Azure para o Azure descrito para **empresa um**, uma ativação pós-falha de nível de sub-rede não é possível neste caso, para **empresa B**. Isto acontece porque o espaço de endereços em redes virtuais de origem e de recuperação é a mesma e a origem original para a ligação no local está ativa.

Para alcançar a resiliência da aplicação, recomenda-se que cada aplicação está alojada na sua própria rede virtual do Azure dedicada. As aplicações podem, em seguida, a ativação pós-falha de forma isolada e o necessário no local para ligações à origem pode ser encaminhado para a região de destino, tal como descrito acima.

## <a name="next-steps"></a>Passos seguintes
- Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md).
