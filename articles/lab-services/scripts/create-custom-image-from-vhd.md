---
title: 'Script do PowerShell: criar uma imagem personalizada de um ficheiro VHD nos serviços de laboratório do Azure | Microsoft Docs'
description: Este script do PowerShell cria uma imagem personalizada de um ficheiro VHD nos serviços de laboratório do Azure.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: e63265d7fea18736bf5c85bcc8954a575d70a51f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-create-a-custom-image-from-a-vhd-file-in-azure-lab-services"></a>Utilize o PowerShell para criar uma imagem personalizada de um ficheiro VHD nos serviços de laboratório do Azure

Este script do PowerShell de exemplo cria uma imagem personalizada de um ficheiro VHD nos serviços de laboratório do Azure

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório personalizado**. O script requer que tenha um laboratório personalizado existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-image-from-vhd/create-custom-image-from-vhd.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Obtém os recursos. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Obtém as chaves de acesso para uma conta de Armazenamento do Azure. |
| [Novo-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | Adiciona uma implementação do Azure para um grupo de recursos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos de script do PowerShell do Azure laboratório serviços adicionais podem ser encontrados no [exemplos do PowerShell de serviços do Azure laboratório](../samples-powershell.md).