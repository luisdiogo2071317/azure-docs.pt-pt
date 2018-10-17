---
title: Personalizar regras de firewall de aplicações web no Gateway de aplicação do Azure - PowerShell | Documentos da Microsoft
description: Este artigo fornece informações sobre como personalizar regras de firewall de aplicações web no Gateway de aplicação com o PowerShell.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: victorh
ms.openlocfilehash: b341bdbe6611187b158f353d00077d33f317f374
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365429"
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Personalizar regras de firewall de aplicações web através do PowerShell

> [!div class="op_single_selector"]
> * [Portal do Azure](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [CLI do Azure](application-gateway-customize-waf-rules-cli.md)

A firewall de aplicações do Gateway de aplicação Azure web (WAF) fornece proteção para aplicativos web. Estas proteções são fornecidas pelo Open Web Application Security Project (OWASP) Core regra definido (CRS). Algumas regras podem causar falsos positivos e bloquear o tráfego real. Por esse motivo, o Gateway de aplicação fornece a capacidade para personalizar regras e grupos de regras. Para obter mais informações sobre os grupos de regras específicas e as regras, consulte [lista de grupos de regras de CRS da firewall de aplicação de web e regras](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Grupos de regras de exibição e regras

Os exemplos de código seguintes mostram como ver as regras e grupos de regras que são configuráveis num gateway de aplicação com capacidade de WAF.

### <a name="view-rule-groups"></a>Grupos de regras do Vista

O exemplo seguinte mostra como ver grupos de regras:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

O resultado seguinte é uma resposta truncada do exemplo anterior:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Desativar regras

O exemplo seguinte desativa regras `910018` e `910017` num gateway de aplicação:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="next-steps"></a>Passos Seguintes

Depois de configurar as regras do desativado, pode aprender a ver os registos da WAF. Para obter mais informações, consulte [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
