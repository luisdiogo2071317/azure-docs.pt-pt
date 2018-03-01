---
title: Exemplo do Script do Azure PowerShell - Criar um cluster do Service Fabric | Microsoft Docs
description: "Exemplo do Script do Azure PowerShell - Crie um cluster de teste do Service Fabric com três nós."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/29/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: fd94a5dd9630cc65dedc180cdfd7aafea83c4866
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Criar um cluster de teste do Service Fabric com três nós

Este script de exemplo cria um cluster de teste do Service Fabric com três nós protegido por um certificado X.509. A configuração do cluster de três nós é suportada para dev/test, uma vez que pode realizar atualizações em segurança e sobreviver a falhas de nó individual (desde que não aconteçam em simultâneo). O cluster de produção precisa de cinco ou mais nós, para que possa resistir a falhas em simultâneo.  

O comando cria um certificado autoassinado e carrega-o para um novo cofre de chaves que é criado no mesmo grupo de recursos do cluster. O certificado é também copiado para um diretório local.  Defina o parâmetro *-OS* para escolher a versão do Windows ou Linux que é executada nos nós do cluster.  Personalize os parâmetros conforme necessário.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Login-AzureRmAccount` para criar uma ligação ao Azure. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos, o cluster e todos os recursos relacionados.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Cria um novo cluster do Service Fabric. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para o Azure Service Fabric em [Exemplos do Azure PowerShell](../service-fabric-powershell-samples.md).
