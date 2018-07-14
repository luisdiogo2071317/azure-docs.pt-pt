---
title: Implantar o Firewall do Azure através de um modelo
description: Implantar o Firewall do Azure através de um modelo
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 1a732e22d72c36afe11030e42bae529baa35df1a
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991550"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Implantar o Firewall do Azure através de um modelo

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

Os exemplos nos artigos de Firewall do Azure partem do princípio de que já ativou a pré-visualização pública do Firewall do Azure. Para obter mais informações, consulte [ativar a pré-visualização pública do Firewall do Azure](public-preview.md).

Este modelo cria uma firewall e um ambiente de rede de teste. A rede tem uma VNet, com três sub-redes: *AzureFirewallSubnet*, *ServersSubnet*e um *JumpboxSubnet*. O ServersSubnet e JumpboxSubnet tem um servidor de Windows de 2 núcleos nas mesmas.

A firewall está a AzureFirewallSubnet e está configurada com uma coleção de regras de aplicação com uma única regra que permite o acesso a www.microsoft.com.

Uma rota definida pelo utilizador é criada que aponte o tráfego de rede do ServersSubnet através da firewall, em que são aplicadas as regras de firewall.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="template-location"></a>Localização do modelo

O modelo está localizado em:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Leia a introdução e, quando estiver pronto para implementar, clique em **implementar no Azure**.

## <a name="clean-up-resources"></a>Limpar recursos

Primeiro, explore os recursos que foram criados com o firewall e, em seguida, ao já não for necessário, pode utilizar o [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) comando para remover o grupo de recursos, o firewall e a todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Passos Seguintes

Em seguida, pode monitorizar os registos de Firewall do Azure:

- [Tutorial: Registos de Firewall do Azure Monitor](./tutorial-diagnostics.md)

