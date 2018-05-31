---
title: Exemplo de Script do Azure PowerShell - Restringir o tráfego da Web | Microsoft Docs
description: Exemplo de Script do Azure PowerShell - Criar um gateway de aplicação com uma firewall de aplicações Web e um conjunto de dimensionamento de máquinas virtuais que utiliza regras OWASP para restringir o tráfego.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d6f8083e0070435e6ad8bb0dc0ff520b58c7eead
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33202291"
---
# <a name="restrict-web-traffic-using-azure-powershell"></a>Restringir o tráfego da web com o Azure PowerShell

Este script cria um gateway de aplicação com uma firewall de aplicações Web que utiliza um conjunto de dimensionamento de máquinas virtuais para servidores back-end. A firewall de aplicações Web restringe o tráfego da Web com base nas regras OWASP. Depois de executar o script, pode testar o gateway de aplicação com o respetivo endereço IP público.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss-waf.ps1 "Create application gateway with WAF")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria a configuração de sub-rede. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria a rede virtual com as configurações de sub-rede. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria o endereço IP público do gateway de aplicação. |
| [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) | Cria a configuração que associa uma sub-rede ao gateway de aplicação. |
| [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) | Cria a configuração que atribui um endereço IP público ao gateway de aplicação. |
| [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) | Atribui uma porta que vai ser utilizada para aceder ao gateway de aplicação. |
| [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) | Cria um conjunto de back-end para um gateway de aplicação. |
| [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) | Configura as definições para um conjunto de back-end. |
| [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) | Cria um serviço de escuta. |
| [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) | Cria uma regra de encaminhamento. |
| [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) | Especifica a camada e a capacidade para um gateway de aplicação. |
| [New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewaywebapplicationfirewallconfiguration) | Cria a configuração da firewall de aplicações Web. |
| [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) | Cria um gateway de aplicação. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Cria um perfil de armazenamento para o conjunto de dimensionamento. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Define o sistema operativo para o conjunto de dimensionamento. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Define a interface de rede para o conjunto de dimensionamento. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) | Criar um conjunto de dimensionamento de máquinas virtuais. |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Cria uma conta de armazenamento. |
| [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) | Configura o diagnóstico para dados de registo. |
| [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Obtém o endereço IP público de um gateway de aplicação. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. | 
## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode encontrar exemplos adicionais de script do PowerShell de gateway de aplicação na [documentação do Gateway de Aplicação do Azure](../powershell-samples.md).
