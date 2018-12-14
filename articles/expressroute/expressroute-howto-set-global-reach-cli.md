---
title: 'Configure o alcance Global do ExpressRoute: CLI do Azure | Documentos da Microsoft'
description: Este artigo ajuda-o a ligar os circuitos do ExpressRoute em conjunto para garantir uma privada, de rede entre as redes no local e ativar o alcance Global.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9a8e0a5df9383d8e3d7159aa916b0e4fbfeea948
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384067"
---
# <a name="configure-expressroute-global-reach-using-azure-cli-preview"></a>Configurar o alcance Global do ExpressRoute com a CLI do Azure (pré-visualização)
Este artigo ajuda-o a configurar o ExpressRoute alcance Global com a CLI do Azure. Para obter mais informações, consulte [alcance Global do ExpressRouteRoute](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Antes de começar
> [!IMPORTANT]
> Esta pré-visualização pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.
> 


Antes de iniciar a configuração, precisa verificar os seguintes requisitos.

* Instale a versão mais recente da CLI do Azure. Ver [instalar a CLI do Azure](/cli/azure/install-azure-cli) e [introdução à CLI do Azure](/cli/azure/get-started-with-azure-cli).
* Compreender o aprovisionamento do circuito de ExpressRoute [fluxos de trabalho](expressroute-workflows.md).
* Certifique-se de que circuitos de ExpressRoute estão num Estado aprovisionado.
* Certifique-se de peering privado do Azure está configurado em seus circuitos do ExpressRoute.  

### <a name="log-into-your-azure-account"></a>Iniciar sessão na sua conta do Azure
Para iniciar a configuração, tem de iniciar sessão na conta do Azure. O comando irá abrir o browser predefinido e solicitar-lhe as credenciais de início de sessão para a sua conta do Azure.  

```azurecli
az login
```

Se tiver múltiplas subscrições do Azure, verifique as subscrições da conta.

```azurecli
az account list
```

Especifique a subscrição que pretende utilizar.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificar os circuitos do ExpressRoute para configuração
Pode ativar o alcance Global do ExpressRoute entre quaisquer dois circuitos do ExpressRoute, desde que eles estão localizados em países/regiões suportados e ocorre em diferentes localizações de peering. Se ambos os circuitos é proprietário de sua assinatura, pode escolher qualquer circuito para executar a configuração nas secções abaixo. Se dois circuitos estiverem em diferentes subscrições do Azure, irá precisar de autorização de uma subscrição do Azure e passar a chave de autorização ao executar o comando de configuração na subscrição do Azure.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Ativar a conectividade entre as redes no local

Ao executar o comando para ativar a conectividade, considere os seguintes valores:

* *circuito do elemento de rede* deve ser o ID de recurso completo. Por exemplo: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}
  ```
* *-AddressPrefix* tem de ser/29 IPv4 sub-rede, por exemplo, "10.0.0.0/29". Utilizaremos os endereços IP nesta sub-rede para estabelecer a conectividade entre os dois circuitos do ExpressRoute. Não tem de utilizar endereços nesta sub-rede nas suas VNets do Azure ou nas suas redes no local.

Execute a CLI seguinte para ligar dois circuitos do ExpressRoute. Utilize o seguinte comando de exemplo:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

O resultado CLI tem um aspeto semelhante ao seguinte exemplo:

```azurecli
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Quando a operação acima estiver concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Circuitos do ExpressRoute em diferentes subscrições do Azure

Se dois circuitos não estiverem na mesma subscrição do Azure, terá de autorização. A seguinte configuração autorização é gerada na subscrição de circuito do 2 e a chave de autorização é passada para o circuito 1.

Gere uma chave de autorização. 
```azurecli
az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
```

A saída da CLI é semelhante ao seguinte.

```azurecli
{
  "authorizationKey": "<authorizationKey>",
  "authorizationUseStatus": "Available",
  "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
  "name": "<AuthorizationName>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<Circuit2ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/authorizations"
}
```

Tome nota do circuito do 2 Id de recurso, bem como a chave de autorização.

Execute o seguinte comando na circuito 1. Passar no Id de recurso de circuito do 2 e a chave de autorização 
```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
```

Quando a operação acima estiver concluída, deve ter conectividade entre as redes no local em ambos os lados por meio de seus dois circuitos do ExpressRoute.

## <a name="get-and-verify-the-configuration"></a>Obter e verificar a configuração

Utilize o seguinte comando para verificar a configuração no circuito onde a configuração foi feita, ou seja, o circuito 1 no exemplo acima.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

Na CLI de saída, verá *CircuitConnectionStatus*. Informá-lo se a conectividade entre os dois circuitos é estabelecida, "Ligado", ou não, "Desligado". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Desativar a conectividade entre as redes no local

Para desativá-la, execute os comandos contra o circuito onde a configuração foi feita, ou seja, o circuito 1 no exemplo acima.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Pode executar a CLI Mostrar para verificar o estado. 

Depois de concluída a operação acima, deixará de ter conectividade entre a rede no local através de circuitos de ExpressRoute. 


## <a name="next-steps"></a>Passos Seguintes
* [Saiba mais sobre o alcance Global do ExpressRoute](expressroute-global-reach.md)
* [Verificar a conectividade do ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Ligar o circuito do ExpressRoute a rede virtual do Azure](expressroute-howto-linkvnet-arm.md)


