---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 874643c5149cf6a222ab6d244dd561e5c4da5a84
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251401"
---
Este artigo utiliza cmdlets do PowerShell. Para executar os cmdlets, pode utilizar o Azure Cloud Shell, um shell interativo gratuito. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Basta clicar em **Copiar** para copiar o código, colá-lo no Cloud Shell e então prima Enter para executá-lo. Existem algumas formas de iniciar o Cloud Shell:

|  |   |
|-----------------------------------------------|---|
| Clique em **Experimentar** no canto superior direito de um bloco de código. | ![Cloud Shell neste artigo](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Abrir o Cloud Shell no seu browser. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Clique no botão **Cloud Shell**, no menu do canto superior direito do portal do Azure. | [![Cloud Shell no portal](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Se não quiser utilizar o Azure Cloud Shell, pode instalar o PowerShell localmente em vez disso. Se optar por instalar e utilizar o PowerShell localmente, certifique-se de que instala a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Cmdlets do PowerShell são atualizados frequentemente e, normalmente, tem de atualizar seus cmdlets do PowerShell para obter as últimas funcionalidades. Se não os atualizar, os valores especificados poderão falhar. 

Para localizar a versão do PowerShell que está a executar localmente, utilize o cmdlet "Get-Module - ListAvailable AzureRM". Se precisar de atualizar, veja [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps). Para obter mais informações, veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview).