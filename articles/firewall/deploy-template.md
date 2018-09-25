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
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991339"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Implantar o Firewall do Azure através de um modelo

Este modelo cria uma firewall e um ambiente de rede de teste. A rede tem uma VNet, com três sub-redes: *AzureFirewallSubnet*, *ServersSubnet*e um *JumpboxSubnet*. ServersSubnet e JumpboxSubnet têm um Windows Server de 2 núcleos em cada um.

A firewall está em AzureFirewallSubnet configurada com uma Coleção de Regras de Aplicação com uma única regra que permite o acesso a www.microsoft.com.

É criada uma rota definida pelo utilizador que aponta o tráfego de rede de ServersSubnet através da firewall, em que são aplicadas as regras de firewall.

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

- [Tutorial: monitorizar registos do Azure Firewall](./tutorial-diagnostics.md)

