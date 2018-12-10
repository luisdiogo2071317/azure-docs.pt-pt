---
title: 'Configurar o alcance Global - ExpressRoute: Azure | Documentos da Microsoft'
description: Este artigo ajuda-o a ligar os circuitos do ExpressRoute em conjunto para garantir uma privada, de rede entre as redes no local e ativar o alcance Global.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: dca2001fda7658b422bbceb14612dec1f7be6cd2
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140902"
---
# <a name="configure-expressroute-global-reach-preview"></a>Configurar o ExpressRoute alcance Global (pré-visualização)
Este artigo ajuda-o a configurar o ExpressRoute alcance Global com o PowerShell. Para obter mais informações, consulte [alcance Global do ExpressRouteRoute](expressroute-global-reach.md).
 
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
Pode ativar o alcance Global do ExpressRoute entre quaisquer dois circuitos do ExpressRoute, desde que eles estão localizados em países/regiões suportados e ocorre em diferentes localizações de peering. Se ambos os circuitos é proprietário de sua assinatura, pode escolher qualquer circuito para executar a configuração nas secções abaixo. Se dois circuitos estiverem em diferentes subscrições do Azure, irá precisar de autorização de uma subscrição do Azure e passar a chave de autorização ao executar o comando de configuração na subscrição do Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Ativar a conectividade entre as redes no local

Utilize os seguintes comandos para obter o circuito 1 e 2 do circuito. Dois circuitos estão na mesma subscrição.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Execute o seguinte comando na circuito 1 e passar no ID de. do circuito do 2 peering privado

> [!NOTE]
> O peering privado do aspeto de Id */subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering*
> 
>

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

> [!IMPORTANT]
> *-AddressPrefix* tem de ser/29 IPv4 sub-rede, por exemplo, "10.0.0.0/29". Utilizaremos os endereços IP nesta sub-rede para estabelecer a conectividade entre os dois circuitos do ExpressRoute. Não tem de utilizar endereços nesta sub-rede nas suas VNets do Azure ou nas suas redes no local.
> 



Guardar a configuração no circuito 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Quando a operação acima estiver concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos do ExpressRoute em diferentes subscrições do Azure

Se dois circuitos não estiverem na mesma subscrição do Azure, terá de autorização. A seguinte configuração autorização é gerada na subscrição de circuito do 2 e a chave de autorização é passada para o circuito 1.

Gere uma chave de autorização. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Tome nota do circuito do 2 Id do peering privado, bem como a chave de autorização.

Execute o seguinte comando na circuito 1. Passar Id do circuito do 2 peering privado e a chave de autorização 
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Guardar a configuração no circuito 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Quando a operação acima estiver concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Obter e verificar a configuração

Utilize o seguinte comando para verificar a configuração no circuito onde a configuração foi feita, ou seja, o circuito 1 no exemplo acima.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Se executar simplesmente *us $ckt1* no PowerShell, verá *CircuitConnectionStatus* na saída. Informá-lo se a conectividade for estabelecida, "Ligado", ou não, "Desligado". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Desativar a conectividade entre as redes no local

Para desativá-la, execute os comandos contra o circuito onde a configuração foi feita, ou seja, o circuito 1 no exemplo acima.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Pode executar a operação Get para verificar o estado. 

Depois de concluída a operação acima, deixará de ter conectividade entre a rede no local através de circuitos de ExpressRoute. 


## <a name="next-steps"></a>Próximos Passos
1. [Saiba mais sobre o alcance Global do ExpressRoute](expressroute-global-reach.md)
2. [Verificar a conectividade do ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
3. [Ligar o circuito do ExpressRoute a rede virtual do Azure](expressroute-howto-linkvnet-arm.md)


