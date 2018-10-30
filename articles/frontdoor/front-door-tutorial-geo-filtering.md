---
title: Tutorial - Configurar a filtragem geográfica num domínio para o Azure Front Door Service | Microsoft Docs
description: Neste tutorial, vai aprender a criar uma política de filtragem geográfica de exemplo e associá-la ao anfitrião de front-end do Front Door existente
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2018
ms.author: sharadag
ms.openlocfilehash: c4032f7c33cec7b7a7864ccff07a05b87c945949
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988606"
---
# <a name="how-to-set-up-a-geo-filtering-policy-for-your-front-door"></a>Como configurar uma política de filtragem geográfica para o Front Door
Este tutorial mostra como utilizar o Azure PowerShell para criar uma política de filtragem geográfica de exemplo e associá-la ao anfitrião de front-end do Front Door existente. Esta política de filtragem geográfica de exemplo irá bloquear os pedidos de todos os outros países, exceto dos Estados Unidos.

## <a name="1-set-up-your-powershell-environment"></a>1. Configurar o ambiente do PowerShell
O Azure PowerShell fornece um conjunto de cmdlets que utilizam o modelo do [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) para gerir os recursos do Azure. 

Pode instalar o [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) no seu computador local e utilizá-lo em qualquer sessão do PowerShell. Siga as instruções na página, para iniciar sessão com as credenciais do Azure, e instale o AzureRM.
```
# Connect to Azure with an interactive dialog for sign-in
Connect-AzureRmAccount
Install-Module -Name AzureRM
```
> [!NOTE]
>  O suporte do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) estará disponível em breve.

Antes de instalar o módulo do Front Door, certifique-se de que tem a versão atual do PowerShellGet instalada. Execute o comando abaixo e reabra o PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

Instale o módulo AzureRM.FrontDoor. 

```
Install-Module -Name AzureRM.FrontDoor -AllowPrerelease
```

## <a name="2-define-geo-filtering-match-conditions"></a>2. Definir as condições de correspondência da filtragem geográfica
Primeiro, crie uma condição de correspondência de exemplo que selecione os pedidos não provenientes de "US". Veja o [guia](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoormatchconditionobject) do PowerShell sobre parâmetros quando criar uma condição de correspondência. É fornecido um código de país de duas letras para o mapeamento de países [aqui](/Protection/GeoFiltering).

```
$nonUSGeoMatchCondition = New-AzureRmFrontDoorMatchConditionObject -MatchVariable RemoteAddr -OperatorProperty GeoMatch -NegateCondition $true -MatchValue "US"
```
 
## <a name="3-add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>3. Adicionar condições de correspondência da filtragem geográfica a uma regra com Ação e Prioridade

Em seguida, crie um objeto CustomRule `nonUSBlockRule` com base na condição de correspondência, numa Ação e numa Prioridade.  Um objeto CustomRule pode ter vários MatchCondition.  Neste exemplo, a Ação está definida como Bloquear e Prioridade como 1, a prioridade mais alta.

```
$nonUSBlockRule = New-AzureRmFrontDoorCustomRuleObject -Name "geoFilterRule" -RuleType MatchRule -MatchCondition $nonUSGeoMatchCondition -Action Block -Priority 1
```

Veja o [guia](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorcustomruleobject) do PowerShell sobre parâmetros quando criar um CustomRuleObject.

## <a name="4-add-rules-to-a-policy"></a>4. Adicionar Regras a uma Política
Este passo cria um objeto de política `geoPolicy` com `nonUSBlockRule` do passo anterior no grupo de recursos especificado. Utilize `Get-AzureRmResourceGroup` para encontrar o $resourceGroup de ResourceGroupName.

```
$geoPolicy = New-AzureRmFrontDoorFireWallPolicy -Name "geoPolicyAllowUSOnly" -resourceGroupName $resourceGroup -Customrule $nonUSBlockRule  -Mode Prevention -EnabledState Enabled
```

Veja o [guia](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorfirewallpolicy) do PowerShell sobre parâmetros quando criar uma política.

## <a name="5-link-policy-to-a-front-door-frontend-host"></a>5. Ligar uma Política a um anfitrião de front-end do Front Door
Os últimos passos são ligar o objeto de política de proteção a um anfitrião de front-end do Front Door existente e atualizar as propriedades do Front Door. Primeiro, vai obter o objeto do Front Door com [Get-AzureRmFrontDoor](https://docs.microsoft.com/azure/frontdoor/get-azurermfrontdoor) e, em seguida, definir a respetiva propriedade WebApplicationFirewallPolicyLink de front-end como resourceId de `geoPolicy`.

```
$geoFrontDoorObjectExample = Get-AzureRmFrontDoor -ResourceGroupName $resourceGroup
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Utilize o seguinte [comando](https://docs.microsoft.com/azure/frontdoor/set-azurermfrontdoor) para atualizar o objeto do Front Door.

```
Set-AzureRmFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Apenas tem de definir a propriedade WebApplicationFirewallPolicyLink uma vez para ligar uma política de proteção a um anfitrião de front-end do Front Door. As atualizações de política subsequentes serão aplicadas automaticamente ao anfitrião de front-end.

## <a name="next-steps"></a>Passos seguintes

- Saiba mais sobre a [segurança de camada de aplicação com o Front Door](front-door-application-security.md).
- Saiba como [criar um Front Door](quickstart-create-front-door.md).
