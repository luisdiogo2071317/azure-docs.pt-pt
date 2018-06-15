---
title: Implementar modelos com o PowerShell na pilha do Azure | Microsoft Docs
description: Implemente um modelo de pilha do Azure com o PowerShell.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 4af82deef029120aa2699e7c69c501ae61a1e8bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359820"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Implementar um modelo para a pilha do Azure com o PowerShell

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar o PowerShell para implementar modelos Azure Resource Manager para a pilha do Azure. Este artigo mostra como utilizar o PowerShell para implementar um modelo.

## <a name="run-azurerm-powershell-cmdlets"></a>Executar os cmdlets do AzureRM PowerShell

Este exemplo utiliza os cmdlets do AzureRM PowerShell e um modelo armazenado no GitHub. O modelo cria uma máquina virtual do Windows Server 2012 R2 Datacenter.

>[!NOTE]
>Antes de tentar neste exemplo, certifique-se de que já [configurado PowerShell](azure-stack-powershell-configure-user.md) para um utilizador de pilha do Azure.

1. Aceda a <http://aka.ms/AzureStackGitHub> e localize o **101-simples-windows-vm** modelo. Guardar o modelo a esta localização: c:\\modelos\\azuredeploy-101-simples-windows-vm.json.
2. Abra uma linha de comandos elevada do PowerShell.
3. Substitua *username* e *palavra-passe* no seguinte script com o nome de utilizador e palavra-passe e, em seguida, execute o script.

   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```

   >[!IMPORTANT]
   >Everytime tem de executar este script, aumentar o valor do parâmetro "$myNum" para impedir a sua implementação de substituição.

4. Abra selecionar portal, Azure pilha **procurar**e, em seguida, selecione **máquinas virtuais** para localizar a máquina virtual nova (*myDeployment001*).

## <a name="next-steps"></a>Passos Seguintes

[Implementar modelos com o Visual Studio](azure-stack-deploy-template-visual-studio.md)
