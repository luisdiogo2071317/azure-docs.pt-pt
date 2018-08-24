---
title: Ligar a aplicação do Azure SQL Database Managed Instance | Documentos da Microsoft
description: Este artigo descreve como ligar a sua aplicação para a instância gerida da base de dados SQL do Azure.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 82e8836892b033ccbb3c3ad9806257348afe3702
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818407"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Ligar a sua aplicação à Instância Gerida de Base de Dados SQL do Azure

Hoje em dia tem várias opções ao decidir como e onde hospedar seu aplicativo. 
 
Pode optar alojar aplicações na cloud utilizando o serviço de aplicações do Azure ou algumas das opções de rede virtual (VNet), integrado do Azure, como o ambiente de serviço de aplicações do Azure, máquinas virtuais, conjunto de dimensionamento da Máquina Virtual. Também pode levar a abordagem de cloud híbrida e manter as suas aplicações no local. 
 
Independentemente da sua escolha que criou, pode ligá-la para uma instância gerida (pré-visualização).  

![elevada disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Ligar uma aplicação dentro da mesma VNet 

Este cenário é a forma mais simples. Máquinas virtuais dentro da VNet pode ligar diretamente entre si mesmo que estejam em sub-redes diferentes. Isso significa que tudo o que precisa para ligar a aplicação dentro de um ambiente de aplicação do Azure ou a Máquina Virtual é definir a cadeia de ligação corretamente.  
 
No caso de não é possível estabelecer a ligação, verifique se tem um grupo de segurança de rede definida na sub-rede de aplicação. Neste caso, terá de abrir a ligação de saída na porta 1433 do SQL, bem como o intervalo de 11000 12000 de portas para o redirecionamento. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Ligar uma aplicação dentro de uma VNet diferente 

Este cenário é um pouco mais complexo, porque a instância gerida tem o endereço IP privado na sua própria VNet. Para ligar, um aplicativo precisa de acesso para a VNet onde a instância gerida está implementada. Por isso, primeiro tem de estabelecer uma ligação entre o aplicativo e a VNet de instância gerida. As VNets não tem de estar na mesma subscrição, para que este cenário funcione. 
 
Existem duas opções para ligar VNets: 
- [Peering de rede Virtual do Azure](../virtual-network/virtual-network-peering-overview.md) 
- Gateway de VPN de VNet a VNet ([portal do Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [da CLI do Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
A opção de peering é aquele preferível porque o peering utiliza a rede de backbone do Microsoft assim, da perspectiva de conectividade, não existe nenhuma diferença notável numa latência entre máquinas virtuais na VNet em modo de peering e na mesma VNet. O VNet peering é limitado às redes na mesma região.  
 
> [!IMPORTANT]
> Cenário de peering de VNet para a instância gerida está limitado às redes na mesma região devido a [restrições do peering de redes virtuais Global](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). 

## <a name="connect-an-on-premises-application"></a>Ligar uma aplicação no local 

Instância gerida só pode ser acedida através de um endereço IP privado. Para aceder a partir do local, terá de estabelecer uma ligação Site a Site entre o aplicativo e a VNet de instância gerida. 
 
Existem duas opções como ligar no local à VNet do Azure: 
- Ligação de VPN de site a Site ([portal do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [da CLI do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) ligação  
 
Se criada com êxito no local para a ligação do Azure e não é possível estabelecer ligação à instância gerida, verifique se a firewall tem de abrir ligação de saída na porta 1433 do SQL, bem como o intervalo de 11000 12000 de portas para o redirecionamento. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Ligar uma aplicação de serviço de aplicações do Azure alojada 

A instância gerida pode ser acessada apenas por meio de um endereço IP privado, portanto, para poder aceder a partir do serviço de aplicações do Azure tem primeiro de estabelecer uma ligação entre o aplicativo e a VNet de instância gerida. Ver [integrar a sua aplicação com uma Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  
 
Para resolução de problemas, consulte [VNets de resolução de problemas e aplicativos](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Se não for possível estabelecer uma ligação, tente [a sincronizar a configuração de rede](sql-database-managed-instance-sync-network-configuration.md). 
 
Um caso especial de conexão do serviço de aplicações do Azure para a instância gerida é quando integrado o que serviço de aplicações do Azure para uma rede em modo de peering de VNet de instância gerida. Esse caso requer a seguinte configuração para configurar a: 

- VNet de instância gerida não pode ter o gateway  
- VNet de instância gerida precisa de conjunto de opções do utilizar gateways remotos 
- VNet peering tem de ter a opção de trânsito de gateway de permitir definido 
 
Este cenário é ilustrado no diagrama seguinte:

![peering de aplicação integrada](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Ligar uma aplicação na caixa de desenvolvedores 

A instância gerida pode ser acedida apenas por meio de um endereço IP privado por isso, para poder acessá-lo de sua caixa de desenvolvedor, primeiro tem de estabelecer uma ligação entre a sua caixa de desenvolvedor e a VNet de instância gerida.  
 
Configurar uma ligação ponto a Site a uma VNet com artigos de autenticação de certificados nativa do Azure ([portal do Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [da CLI do Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) apresenta em detalhe como poderia ser feito. 

## <a name="required-versions-of-drivers-and-tools"></a>Versões necessárias do controlador e as ferramentas

As seguintes versões mínimas a ferramentas e os controladores são recomendadas para ligar à instância gerida:

| Ferramenta de Driver / | Versão |
| --- | --- |
|.NET Framework | 4.6.1 (ou .NET Core) | 
|Controlador ODBC    | v17 |
|Controladores PHP | 5.2.0 |
|JDBC driver    | 6.4.0 |
|Controlador node. js | 2.1.1 |
|Controlador OLEDB   | 18.0.2.0 |
|SSMS   | 17.8.1 ou [superior](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) |

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre a instância gerida, veja [o que é uma instância gerida](sql-database-managed-instance.md).
- Para obter um tutorial que mostra como criar uma nova instância gerida, veja [criar uma instância gerida](sql-database-managed-instance-get-started.md).
