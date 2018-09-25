---
title: Alcance Global do ExpressRoute do Azure | Documentos da Microsoft
description: Este artigo ajuda-o a ligar os circuitos do ExpressRoute em conjunto para garantir uma privada, de rede entre as redes no local e ativar o alcance Global.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 90f34da10e0193e31a0f70187463799013f52be8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966820"
---
# <a name="link-expressroute-circuits-to-enable-expressroute-global-reach-preview"></a>Circuitos do ExpressRoute de ligação para ativar o alcance Global ExpressRoute (pré-visualização)

O ExpressRoute é uma forma privada e resiliente para ligar as suas redes no local para a Cloud da Microsoft. Pode aceder muitos serviços cloud da Microsoft, como o Azure, Office 365 e Dynamics 365 a partir do seu centro de dados privada ou a sua rede empresarial. Por exemplo, pode ter uma filial em são Francisco com um circuito do ExpressRoute no vale do silício e outra sucursal de Baltimore com um circuito do ExpressRoute em Washington DC. 

Ambas as filiais podem ter conectividade de alta velocidade para recursos do Azure nos E.U.A. leste e E.U.A. oeste. No entanto, as filiais não podem trocar dados diretamente entre si. Em outras palavras, 10.0.1.0/24 podem trocar dados com 10.0.3.0/24 e 10.0.4.0/24, mas não com 10.0.2.0/24.

![sem][1]

Com o **alcance Global do ExpressRoute**, pode associar os circuitos do ExpressRoute em conjunto para garantir uma rede de dados privada entre as redes no local. No exemplo acima, com a adição do ExpressRoute alcance Global, seu escritório de San Francisco (10.0.1.0/24) diretamente pode trocar dados com o seu escritório de Baltimore (10.0.2.0/24) por meio dos circuitos do ExpressRoute existentes e via rede global da Microsoft. 

![com o][2]

Alcance Global do ExpressRoute atualmente é suportada nos seguintes países:

* Estados Unidos
* Reino Unido 
* Japão

Os circuitos do ExpressRoute tem de ser criados no [localizações de peering de ExpressRoute](expressroute-locations.md) nos países acima. Para ativar o ExpressRoute alcance Global entre [diferentes regiões geopolíticas](expressroute-locations.md), seus circuitos tem de ser Premium SKU.


## <a name="before-you-begin"></a>Antes de começar

> [!IMPORTANT]
> Esta pré-visualização pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.
>

Antes de iniciar a configuração, precisa verificar os seguintes requisitos.

* Instale a versão mais recente do Azure PowerShell. Ver [instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Compreender o aprovisionamento do circuito de ExpressRoute [fluxos de trabalho](expressroute-workflows.md).
* Certifique-se de que circuitos de ExpressRoute estão num Estado aprovisionado.
* Certifique-se de peering privado do Azure está configurado em seus circuitos do ExpressRoute.  

### <a name="log-into-your-azure-account"></a>Iniciar sessão na sua conta do Azure
Para iniciar a configuração, tem de iniciar sessão na conta do Azure. 

Abra a consola do PowerShell com privilégios elevados e ligue-se à sua conta. O comando irá solicitar-lhe para as credenciais de início de sessão para a sua conta do Azure.  

```powershell
Connect-AzureRmAccount
```

Se tiver múltiplas subscrições do Azure, verifique as subscrições da conta.

```powershell
Get-AzureRmSubscription
```

Especifique a subscrição que pretende utilizar.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificar os circuitos do ExpressRoute para configuração
Pode ativar o alcance Global do ExpressRoute entre quaisquer dois circuitos do ExpressRoute, desde que eles estão localizados em países/regiões suportados e ocorre em diferentes localizações de peering. Se a sua subscrição é a proprietária de ambos os circuitos, pode escolher qualquer circuito para executar a configuração nas secções abaixo. Se dois circuitos estiverem em diferentes subscrições do Azure, irá precisar de autorização de uma subscrição do Azure e passar a chave de autorização ao executar o comando de configuração na subscrição do Azure.

## <a name="to-enable-connectivity-between-your-on-premises-networks"></a>Para ativar a conectividade entre as redes no local

Utilize os seguintes comandos para obter o circuito 1 e 2 do circuito. Dois circuitos estão na mesma subscrição.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Execute o seguinte comando na circuito 1 e passar no ID de. do circuito do 2 peering privado

Tenha em atenção que Id do peering privado é semelhante a:

`/subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering`


```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

O *- AddressPrefix* tem de ser/29 IPv4 sub-rede, por exemplo "10.0.0.0/29". Utilizaremos os endereços IP nesta sub-rede para estabelecer a conectividade entre os dois circuitos do ExpressRoute. Não deve utilizar endereços nesta sub-rede nas suas VNets do Azure ou nas suas redes no local. 


Guarde a configuração no circuito 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Quando a operação anterior está concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos do ExpressRoute em diferentes subscrições do Azure

Se dois circuitos não estiverem na mesma subscrição do Azure, terá de autorização. A seguinte configuração autorização é gerada na subscrição de circuito do 2 e a chave de autorização é passada para o circuito 1.

Gere uma chave de autorização. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Tome nota do ID do circuito do 2 peering privado, bem como a chave de autorização.

Execute o seguinte comando na circuito 1. Passar ID do circuito do 2 peering privado e a chave de autorização

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Guarde a configuração no circuito 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Quando as operações anteriores for concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

## <a name="to-get-and-verify-the-configuration"></a>Para obter e verificar a configuração

Utilize o seguinte comando para verificar a configuração no circuito onde foi efetuada a configuração. Este exemplo utiliza o circuito 1 do exemplo anterior.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Se executar simplesmente *us $ckt1* no PowerShell, verá *CircuitConnectionStatus* na saída. Informá-lo se a conectividade for estabelecida, "Ligado", ou não, "Desligado". 

## <a name="to-disable-connectivity-between-your-on-premises-networks"></a>Para desativar a conectividade entre as redes no local

Para desativar, execute os comandos contra o circuito onde foi efetuada a configuração. Este exemplo utiliza o circuito 1 a partir dos exemplos anteriores.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Pode executar a operação Get para verificar o estado. 

Depois de concluída a operação acima, deixará de ter conectividade entre a rede no local através de circuitos de ExpressRoute. 

## <a name="next-steps"></a>Passos Seguintes
1. [Saiba mais sobre o alcance Global do ExpressRoute](expressroute-faqs.md#globalreach)
2. [Verificar a conectividade do ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
3. [Ligar o circuito do ExpressRoute a rede virtual do Azure](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "diagrama sem alcance global"
[2]: ./media/expressroute-global-reach/2.png "diagrama com alcance global"