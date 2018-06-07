---
title: Verifique a utilização de recursos do Azure contra limites | Microsoft Docs
description: Saiba como verificar a utilização de recursos do Azure com os limites de subscrição do Azure.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: networking
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jdial
ms.openlocfilehash: 0c51f48576c665fbe67f2f18198d6422fe872895
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811749"
---
# <a name="check-resource-usage-against-limits"></a>Verifique a utilização de recursos em relação a limites

Neste artigo, irá aprender a ver o número de cada tipo de recurso de rede que tenha implementado na sua subscrição e o seu [limites de subscrição](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) são. A capacidade de ver contra os limites de utilização de recursos é útil para controlar a utilização atual e planear para utilização futura. Pode utilizar o [Portal do Azure](#azure-portal), [PowerShell](#powershell), ou o [CLI do Azure](#azure-cli) para controlar a utilização.

## <a name="azure-portal"></a>Portal do Azure

1. Iniciar sessão do Azure [portal](https://portal.azure.com).
2. No canto superior, à esquerda do portal do Azure, selecione **todos os serviços**.
3. Introduza *subscrições* no **filtro** caixa. Quando **subscrições** aparece nos resultados da pesquisa, selecionados-lo.
4. Selecione o nome da subscrição que pretende visualizar as informações de utilização.
5. Em **definições**, selecione **utilização + quota**.
6. Pode selecionar as seguintes opções:
    - **Tipos de recursos**: pode selecionar todos os tipos de recursos ou selecione os tipos específicos de recursos que pretende visualizar.
    - **Fornecedores**: pode selecionar todos os fornecedores de recursos ou selecione **computação**, **rede**, ou **armazenamento**.
    - **Localizações**: pode selecionar todas as localizações do Azure, ou selecione localizações específicas.
    - Pode selecionar para mostrar todos os recursos ou apenas os recursos em que pelo menos um é implementado.

    O exemplo na imagem seguinte mostra todos os recursos de rede pelo menos um recurso implementado nos E.U.A. Leste:

        ![View usage data](./media/check-usage-against-limits/view-usage.png)

    Pode ordenar as colunas selecionando no cabeçalho da coluna. Os limites apresentados são os limites para a sua subscrição. Se precisar de aumentar o limite do predefinido, selecione **pedido aumentar**, em seguida, concluir e submeter o pedido de suporte. Todos os recursos tenham um limite máximo listado no Azure [limites](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Se o limite atual já está no número máximo, não é possível aumentar o limite.

## <a name="powershell"></a>PowerShell

Pode executar os comandos que se seguem na [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. A Shell de nuvem do Azure é uma shell interativa livre. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Se executar o PowerShell a partir do seu computador, terá do *AzureRM* módulo do PowerShell, versão 6.0.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` no seu computador, para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar o PowerShell localmente, terá também de executar `Login-AzureRmAccount` para iniciar sessão no Azure.

Ver a utilização de limites com [Get-AzureRmNetworkUsage](/powershell/module/azurerm.network/powershell/module/azurerm.network/get-azurermnetworkusage). O exemplo seguinte obtém a utilização de recursos em que pelo menos um recurso é implementado na localização EUA Leste:

```azurepowershell-interactive
Get-AzureRmNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Poderá recebe um resultado formatado com o mesmo que o resultado de exemplo seguinte:

```powershell
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>CLI do Azure

Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este artigo requer a CLI do Azure versão 2.0.32 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar a CLI do Azure localmente, terá também de executar `az login` para iniciar sessão no Azure.

Ver a utilização de limites com [az rede lista-utilizações](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). O exemplo seguinte obtém a utilização de recursos na localização EUA Leste:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Poderá recebe um resultado formatado com o mesmo que o resultado de exemplo seguinte:

```azurecli
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
