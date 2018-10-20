---
title: Como para configurar o Azure ExpressRoute direta com a CLI | Documentos da Microsoft
description: Esta página ajuda-o a configurar direta do ExpressRoute com a CLI (pré-visualização)
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: 6356154bf30fcb2557e288305fa032989e5ddc9d
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471849"
---
# <a name="how-to-configure-expressroute-direct-using-cli-preview"></a>Como configurar direta do ExpressRoute com a CLI (pré-visualização)

ExpressRoute Direct dá-lhe a capacidade de ligar diretamente para a rede global da Microsoft em localizações de peering estrategicamente distribuídas em todo o mundo. Para obter mais informações, consulte [sobre o ExpressRoute direto ligar](expressroute-erdirect-about.md).

> [!IMPORTANT]
> ExpressRoute direto está atualmente em pré-visualização.
>
> Esta Pré-visualização Pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="resources"></a>Criar o recurso

1. Inicie sessão no Azure e selecione a subscrição. O recurso direto do ExpressRoute e circuitos do ExpressRoute tem de ser na mesma subscrição.

  ```azurecli
  az login
  ```

  Verifique as subscrições da conta. 

  ```azurecli
  az account list 
  ```

  Selecione a subscrição para o qual pretende criar um circuito do ExpressRoute.
  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Liste todas as localizações onde ExpressRoute direta é suportada.
    
  ```azurecli
  az network express-route port location list
  ```

  **Exemplo de saída**
  
  ```azurecli
  [
   {
    "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
    "location": null,
    "name": "Equinix-Ashburn-DC2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA3",
    "location": null,
    "name": "Equinix-Dallas-DA3",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "111 8th Avenue, New York, NY 10011",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-New-York-NY5",
    "location": null,
    "name": "Equinix-New-York-NY5",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "11 Great Oaks Blvd, SV1, San Jose, CA 95119",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-SV1",
    "location": null,
    "name": "Equinix-San-Jose-SV1",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "2001 Sixth Ave., Suite 350, SE2, Seattle, WA 98121",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Seattle-SE2",
    "location": null,
    "name": "Equinix-Seattle-SE2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   }
  ]
  ```
3. Determinar se uma localização listada acima tem largura de banda disponível

  ```azurecli
  az network express-route port location show -l "Equinix-Ashburn-DC2"
  ```

  **Exemplo de saída**

  ```azurecli
  {
  "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
  "availableBandwidths": [
    {
      "offerName": "100 Gbps",
      "valueInGbps": 100
    }
  ],
  "contact": "support@equinix.com",
  "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
  "location": null,
  "name": "Equinix-Ashburn-DC2",
  "provisioningState": "Succeeded",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePortsLocations"
  }
  ```
4. Criar um recurso direto do ExpressRoute com base na localização selecionada acima

  ExpressRoute Direct suporta QinQ e Dot1Q encapsulamento. Se for selecionado QinQ, cada circuito ExpressRoute vai ser atribuído dinamicamente uma marca de S e seja exclusivo em todo o recurso direto do ExpressRoute. Cada etiqueta-C no circuito tem de ser exclusiva no circuito, mas não em direto do ExpressRoute.  

  Se for selecionado o encapsulamento Dot1Q, tem de gerir exclusividade de etiqueta-C (VLAN) em recursos de ExpressRoute Direct inteiro.  

  > [!IMPORTANT]
  > ExpressRoute Direct só pode ser um tipo de encapsulamento. Encapsulamento não pode ser alterado após a criação direta de ExpressRoute.
  > 
 
  ```azurecli
  az network express-route port create -n $name -g $RGName --bandwidth 100 gbps  --encapsulation QinQ | Dot1Q --peering-location $PeeringLocationName -l $AzureRegion 
  ```

  > [!NOTE]
  > O atributo de encapsulamento também poderia ser definido como Dot1Q. 
  >

  **Exemplo de saída:**

  ```azurecli
  {
  "allocationDate": "Wednesday, October 17, 2018",
  "bandwidthInGbps": 100,
  "circuits": null,
  "encapsulation": "Dot1Q",
  "etag": "W/\"<etagnumber>\"",
  "etherType": "0x8100",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
  "links": [
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
  ],
  "location": "westus",
  "mtu": "1500",
  "name": "Contoso-Direct",
  "peeringLocation": "Equinix-Ashburn-DC2",
  "provisionedBandwidthInGbps": 0.0,
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "resourceGuid": "02ee21fe-4223-4942-a6bc-8d81daabc94f",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePorts"
  }  
  ```

## <a name="state"></a>Alterar o estado de administrador de ligações

Este processo deve ser utilizado para conduzir um teste de camada 1, garantindo que cada ligação cruzada é corretamente corrigida em cada router principal e secundário.

1. Defina o Link para ativado. Repita este passo para definir cada ligação ativada.

  Ligações [0] é a porta primária e Links [1] é a porta secundária.

  ```azurecli
  az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[0].adminState="Enabled"
  ```
  ```azurecli
  az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[1].adminState="Enabled"
  ```
  **Exemplo de saída:**

  ```azurecli
  {
  "allocationDate": "Wednesday, October 17, 2018",
  "bandwidthInGbps": 100,
  "circuits": null,
  "encapsulation": "Dot1Q",
  "etag": "W/\"<etagnumber>\"",
  "etherType": "0x8100",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
  "links": [
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
  ],
  "location": "westus",
  "mtu": "1500",
  "name": "Contoso-Direct",
  "peeringLocation": "Equinix-Ashburn-DC2",
  "provisionedBandwidthInGbps": 0.0,
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "resourceGuid": "<resourceGUID>",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePorts"
  }
  ```

  Utilize o mesmo procedimento com `AdminState = “Disabled”` para ativar às portas.

## <a name="circuit"></a>Criar um circuito

Por predefinição, pode criar 10 circuitos na subscrição em que é o recurso direto do ExpressRoute. Isso pode ser aumentado em suporte. É responsável por controlar aprovisionado e largura de banda utilizada. Largura de banda aprovisionada é a soma de largura de banda de todos os circuitos no recurso direto do ExpressRoute e utilizada de largura de banda é a utilização física das interfaces físicas subjacentes.

Existem larguras de banda do circuito adicionais que podem ser utilizadas no ExpressRoute Direct apenas para suportar os cenários descritos acima. Estes são: 40Gbps e 100Gbps.

Circuitos do Standard ou premium podem ser criados. Circuitos padrão são incluídos no custo, enquanto os circuitos premium têm um custo com base na largura de banda selecionada. Circuitos só podem ser criados como tráfego limitado, como ilimitado, não é suportado em direto do ExpressRoute.

Crie um circuito no recurso direto do ExpressRoute.

  ```azurecli
  az network express-route create --express-route-port "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct" -n "Contoso-Direct-ckt" -g "Contoso-Direct-rg" --sku-family MeteredData --sku-tier Standard --bandwidth 100 Gbps
  ```

  Incluem outras larguras de banda: 1.0, 2.0, 5.0, 10.0 e 40.0

  **Exemplo de saída:**

  ```azurecli
  {
  "allowClassicOperations": false,
  "allowGlobalReach": false,
  "authorizations": [],
  "bandwidthInGbps": 100.0,
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"<etagnumber>\"",
  "expressRoutePort": {
    "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
    "resourceGroup": "Contoso-Direct-rg"
  },
  "gatewayManagerEtag": "",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRouteCircuits/ERDirect-ckt-cli",
  "location": "westus",
  "name": "ERDirect-ckt-cli",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "serviceKey": "<serviceKey>",
  "serviceProviderNotes": null,
  "serviceProviderProperties": null,
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "MeteredData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "stag": null,
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits"
  }  
  ```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Direct do ExpressRoute, consulte a [descrição geral](expressroute-erdirect-about.md).
