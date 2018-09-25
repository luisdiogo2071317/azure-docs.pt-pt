---
title: Personalizar regras de firewall de aplicações web no Gateway de aplicação do Azure - portal do Azure | Documentos da Microsoft
description: Este artigo fornece informações sobre como personalizar regras de firewall de aplicações do web no Gateway de aplicação com o portal do Azure.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: victorh
ms.openlocfilehash: 30df26dc3a9697d3435779f91c32b2d99a747b88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990472"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Personalizar regras de firewall de aplicações web através do portal do Azure

> [!div class="op_single_selector"]
> * [Portal do Azure](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [CLI do Azure](application-gateway-customize-waf-rules-cli.md)

A firewall de aplicações do Gateway de aplicação Azure web (WAF) fornece proteção para aplicativos web. Estas proteções são fornecidas pelo Open Web Application Security Project (OWASP) Core regra definido (CRS). Algumas regras podem causar falsos positivos e bloquear o tráfego real. Por esse motivo, o Gateway de aplicação fornece a capacidade para personalizar regras e grupos de regras. Para obter mais informações sobre os grupos de regras específicas e as regras, consulte [lista de grupos de regras CRS do web application firewall e regras](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Se o gateway de aplicação não está a utilizar a camada de WAF, a opção de atualizar o gateway de aplicação para o escalão WAF é apresentada no painel da direita. 

![Ativar a WAF][fig1]

## <a name="view-rule-groups-and-rules"></a>Grupos de regras de exibição e regras

**Para ver os grupos de regras e regras**
   1. Navegue para o gateway de aplicação e, em seguida, selecione **firewall de aplicações Web**.  
   2. Selecione **configuração avançada de regra**.  
   Esta vista mostra uma tabela na página de todos os grupos de regra fornecido com o conjunto de regras escolhido. Todas as caixas de verificação da regra estão selecionadas.

![Configurar regras desativadas][1]

## <a name="search-for-rules-to-disable"></a>Procurar regras desativar

O **definições de firewall de aplicação Web** painel fornece a capacidade de filtrar as regras através de uma pesquisa de texto. O resultado mostra apenas os grupos de regras e regras que contêm o texto que está à procura.

![Procurar regras][2]

## <a name="disable-rule-groups-and-rules"></a>Desativar grupos de regras e regras

Quando seu está desabilitando regras, pode desativar um grupo inteiro de regra ou regras específicas num ou mais grupos de regras. 

**Para desativar a grupos de regra ou regras específicas**

   1. Procure as regras ou grupos de regras que pretende desativar.
   2. Desmarque as caixas de verificação para as regras que pretende desativar. 
   2. Selecione **Guardar**. 

![Guardar alterações][3]

## <a name="next-steps"></a>Passos Seguintes

Depois de configurar as regras do desativado, pode aprender a ver os registos da WAF. Para obter mais informações, consulte [diagnóstico do Gateway de aplicação](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
