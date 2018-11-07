---
title: incluir ficheiro
description: incluir ficheiro
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 11/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fab00e5281bb91bce10228b3bc2e9cfd503d5d5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219822"
---
Tem de criar uma VNet e uma sub-rede de gateway em primeiro lugar, antes de trabalhar nas seguintes tarefas.

> [!NOTE]
> Estes exemplos não se aplicam ao S2S/ExpressRoute coexistir configurações.
> Para obter mais informações sobre como trabalhar com gateways numa configuração coexist, consulte [configurar ligações coexistentes](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Adicionar um gateway

Utilize o comando abaixo para criar um gateway. Não se esqueça de substituir quaisquer valores pelos seus próprios.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Certifique-se de que a criação do gateway

Utilize o comando abaixo para verificar que o gateway foi criado. Este comando obtém também o ID de gateway, o que precisa para outras operações.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Redimensionar um gateway

Existem diversas [SKUs de Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Pode utilizar o seguinte comando para alterar o SKU de Gateway em qualquer altura.

> [!IMPORTANT]
> Este comando não funcionar para o gateway de UltraPerformance. Para alterar o gateway para um gateway de UltraPerformance, remova primeiro o gateway do ExpressRoute existente e, em seguida, crie um novo gateway de UltraPerformance. Para mudar o gateway de um gateway de UltraPerformance, para remover primeiro o gateway de UltraPerformance e, em seguida, crie um novo gateway. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Remover um gateway

Utilize o comando abaixo para remover um gateway

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```
