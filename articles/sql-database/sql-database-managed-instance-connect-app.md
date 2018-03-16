---
title: "Instância de geridos de base de dados de SQL do Azure ligar aplicação | Microsoft Docs"
description: "Este artigo descreve como ligar a aplicação à base de dados geridos instância do SQL do Azure."
ms.service: sql-database
author: srdjan-bozovic
manager: craigg
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: f02311026e3f28d4cf41dfe9b155f928885ae938
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Ligar a aplicação à base de dados geridos instância do SQL do Azure

Hoje em dia tem várias opções quando decidir como e onde alojar a aplicação. 
 
Pode optar por anfitrião de aplicações na nuvem utilizando o App Service do Azure ou algumas das opções de rede virtual (VNet) integrada do Azure, como o ambiente de serviço de aplicações do Azure, Máquina Virtual, conjunto de dimensionamento da Máquina Virtual. Foi também adotar abordagem de nuvem híbrida e manter as aplicações no local. 
 
Qualquer opção efetuada, pode ligá-la a uma instância geridos (pré-visualização).  

![Elevada disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Ligar uma aplicação dentro da mesma VNet 

Este cenário é a mais simples. Máquinas virtuais dentro da VNet pode ligar diretamente a si mesmo que se encontrem dentro de sub-redes diferentes. Isto significa que tudo o que precisa para ligar a aplicações dentro de um ambiente de aplicação do Azure ou a Máquina Virtual está a definir adequadamente a cadeia de ligação.  
 
No caso de não é possível estabelecer a ligação, verifique se tem um grupo de segurança de rede definida na sub-rede da aplicação. Neste caso, terá de abrir a ligação saída na porta 1433 do SQL Server, bem como 11000 12000 intervalo de portas para o redirecionamento. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Ligar uma aplicação dentro de uma VNet diferentes 

Este cenário é um pouco mais complexo, porque a instância gerido tem o endereço IP privado na sua própria VNet. Para ligar, uma aplicação precisa de acesso para a VNet onde a instância geridos é implementada. Por isso, primeiro tem de estabelecer uma ligação entre a aplicação e a VNet de instância geridos. As VNets não têm de estar na mesma subscrição por ordem para este cenário funcionem. 
 
Existem duas opções para ligar VNets: 
- [Peering de rede Virtual do Azure](../virtual-network/virtual-network-peering-overview.md) 
- Gateway de VPN de VNet a VNet ([portal do Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [CLI do Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
A opção de peering é o preferível porque peering utiliza da rede principal do Microsoft por isso, a perspetiva de conectividade, não existe nenhuma diferença percetível de latência entre máquinas virtuais na VNet em modo de peering e na mesma VNet. O VNet peering é limitado para as redes na mesma região, embora por várias regiões peering está ativado em algumas regiões como uma pré-visualização.  
 
> [!IMPORTANT]
> Peerings de VNet criada por várias regiões poderá não ter o mesmo nível de disponibilidade e fiabilidade como peerings uma versão de disponibilidade geral. Peerings de VNet pode ter restrita capacidades e poderão não estar disponíveis em todas as regiões do Azure. Para as notificações mais atualizadas à sua disponibilidade e o estado desta funcionalidade, consulte o [Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network) página atualizações. 

## <a name="connect-an-on-premises-application"></a>Ligar uma aplicação no local 

Instância gerida só pode ser acedida através de um endereço IP privado. Para poder aceder ao mesmo no local, tem de estabelecer uma ligação Site a Site entre a aplicação e a VNet de instância geridos. 
 
Existem duas opções como ligar no local a VNet do Azure: 
- Ligação de VPN de site para Site ([portal do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [CLI do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) ligação  
 
Se estabeleceu no local para Azure ligação com êxito e não é possível estabelecer ligação à instância geridos, verifique se a firewall tem ligação aberta de saída na porta 1433 do SQL Server, bem como 11000 12000 intervalo de portas para redirecionamento. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Ligar uma aplicação de serviço de aplicações do Azure alojada 

Gerido instância pode ser acedida apenas através de um endereço IP privado para poder aceder a partir do App Service do Azure terá primeiro de estabelecer uma ligação entre a aplicação e a VNet de instância geridos. Consulte [integrar a sua aplicação com uma Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  
 
Para resolução de problemas, consulte [VNets de resolução de problemas e aplicações](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Se não é possível estabelecer uma ligação, tente [sincronizar a configuração de rede](sql-database-managed-instance-sync-network-configuration.md). 
 
Num caso especial do App Service do Azure para ligar à instância geridos é quando é integrado que do serviço de aplicações do Azure a uma rede em modo de peering para VNet de instância geridos. Nesse caso requer a configuração seguinte configuração: 

- Gerido VNet de instância não pode ter o gateway  
- Gerido de instância de VNet tem de ter a opção definida utilizar gateways remoto 
- VNet em modo de peering tem de ter a opção de trânsito do gateway do permitir definida 
 
Este cenário é ilustrado no diagrama seguinte:

![peering de aplicação integrada](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Ligar uma aplicação na caixa de programadores 

Gerido instância pode ser acedida apenas através de um endereço IP privado, por isso, para poder aceder a partir da sua caixa de programador, primeiro tem de estabelecer uma ligação entre a sua caixa de programador e a VNet de instância geridos.  
 
Configurar uma ligação ponto a Site para uma VNet com artigos de autenticação do certificado Azure nativo ([portal do Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [CLI do Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) apresenta em detalhe como foi feita.  

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre a instância geridos, consulte [o que é uma instância geridos](sql-database-managed-instance.md).
- Para um tutorial, consulte [criar uma instância geridos](sql-database-managed-instance-tutorial-portal.md).
