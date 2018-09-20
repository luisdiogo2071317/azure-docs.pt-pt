---
title: Implementar modelos com a linha de comandos no Azure Stack | Documentos da Microsoft
description: Saiba como utilizar a interface de linha de comandos para várias plataformas (CLI) para implementar modelos do Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: d40f9e8bf319f327879a71b23f9d9b82da924c2f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365439"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Implementar modelos no Azure Stack, utilizando a linha de comandos

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Utilize a linha de comandos para implementar modelos Azure Resource Manager no ambiente do Kit de desenvolvimento do Azure Stack. Modelos Azure Resource Manager implementar e aprovisionar todos os recursos para a sua aplicação numa operação única e coordenada.

## <a name="before-you-begin"></a>Antes de começar

 - [Instalar e ligar](azure-stack-version-profiles-azurecli2.md) ao Azure Stack com a CLI do Azure.
 - Transferir os ficheiros *azuredeploy. JSON* e *azuredeploy* partir os [criar modelo de exemplo de conta de armazenamento](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Implementar o modelo

Navegue para a pasta em que estes ficheiros foram transferidos e execute o seguinte comando para implementar o modelo:

```azurecli
az group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json
```

Este comando implementa o modelo para o grupo de recursos **cliRG** na localização predefinida da prova de conceito do Azure Stack.

## <a name="validate-template-deployment"></a>Validar a implementação do modelo

Para ver esta conta de armazenamento e de grupo de recursos, utilize os seguintes comandos:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a implementação de modelos, veja:

[Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)

