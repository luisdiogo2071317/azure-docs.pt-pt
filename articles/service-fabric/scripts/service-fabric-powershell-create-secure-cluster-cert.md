---
title: Script do PowerShell do Azure de exemplo - criar um cluster do Service Fabric | Microsoft Docs
description: Script do PowerShell do Azure de exemplo - criar um cluster do Service Fabric.
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
ms.date: 01/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: b0d33b714092a826012677c95124d74bf2c72999
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="create-a-service-fabric-cluster"></a>Criar um cluster do Service Fabric

Este script de exemplo cria um cluster do Service Fabric cinco nós protegido por um certificado x. 509.  O comando cria um certificado autoassinado e carrega-o para um novo cofre de chaves. O certificado é também copiado para um diretório local.  Defina o parâmetro *-OS* para escolher a versão do Windows ou Linux que é executada nos nós do cluster.  Personalize os parâmetros conforme necessário.

Se necessário, instale o Azure PowerShell com a instrução foi encontrada no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Depois de executar o script de exemplo, o seguinte comando pode ser utilizado para remover o grupo de recursos, cluster e todos os recursos relacionados.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Cria um novo cluster de Service Fabric. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo Azure PowerShell, consulte [documentação do Azure PowerShell](/powershell/azure/overview).

Amostras de Azure Powershell adicionais para o Azure Service Fabric podem ser encontradas no [exemplos do PowerShell do Azure](../service-fabric-powershell-samples.md).
