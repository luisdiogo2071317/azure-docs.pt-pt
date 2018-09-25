---
title: Personalizar regras de firewall de aplicações web no Gateway de aplicação do Azure - CLI do Azure | Documentos da Microsoft
description: Este artigo fornece informações sobre como personalizar regras de firewall de aplicações web no Gateway de aplicação com a CLI do Azure.
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
ms.openlocfilehash: c02e4edabdcb73bc14c64b42788cddc98d78498c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964126"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-cli"></a>Personalizar regras de firewall de aplicações web através da CLI do Azure

> [!div class="op_single_selector"]
> * [Portal do Azure](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [CLI do Azure](application-gateway-customize-waf-rules-cli.md)

A firewall de aplicações do Gateway de aplicação Azure web (WAF) fornece proteção para aplicativos web. Estas proteções são fornecidas pelo Open Web Application Security Project (OWASP) Core regra definido (CRS). Algumas regras podem causar falsos positivos e bloquear o tráfego real. Por esse motivo, o Gateway de aplicação fornece a capacidade para personalizar regras e grupos de regras. Para obter mais informações sobre os grupos de regras específicas e as regras, consulte [lista de grupos de regras CRS do web application firewall e regras](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Grupos de regras de exibição e regras

Os exemplos de código seguintes mostram como ver as regras e grupos de regras que são configuráveis.

### <a name="view-rule-groups"></a>Grupos de regras do Vista

O exemplo seguinte mostra como ver os grupos de regras:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

O resultado seguinte é uma resposta truncada do exemplo anterior:

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a>Ver regras num grupo de regras

O exemplo seguinte mostra como ver as regras num grupo de regra especificado:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

O resultado seguinte é uma resposta truncada do exemplo anterior:

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a>Desativar regras

O exemplo seguinte desativa regras `910018` e `910017` num gateway de aplicação:

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a>Passos Seguintes

Depois de configurar as regras do desativado, pode aprender a ver os registos da WAF. Para obter mais informações, consulte [diagnóstico do Gateway de aplicação](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
