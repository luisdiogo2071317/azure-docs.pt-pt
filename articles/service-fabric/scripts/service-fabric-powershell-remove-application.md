---
title: "Exemplo de Script do PowerShell do Azure - remover aplicações a partir de um cluster | Microsoft Docs"
description: "Azure PowerShell Script de exemplo - remover uma aplicação de um cluster do Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ea7037fc3655298fc4c03b8d9f988a55e42c9fe9
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Remover uma aplicação de um cluster do Service Fabric

Este script de exemplo elimina uma instância de aplicação de Service Fabric em execução e anula o registo de um tipo de aplicação e a versão do cluster.  A eliminar a instância da aplicação também elimina todos os a execução do serviço instâncias associadas com essa aplicação. Personalize os parâmetros conforme necessário. 

Se necessário, instale o módulo do PowerShell de Service Fabric com o [SDK de Service Fabric](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Remove uma instância de aplicação de Service Fabric em execução do cluster.  |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Anula o registo de um tipo de aplicação de Service Fabric e a versão do cluster. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do PowerShell de Service Fabric, consulte [documentação do Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Exemplos do Powershell adicionais para o Azure Service Fabric podem ser encontrados no [exemplos do PowerShell do Azure](../service-fabric-powershell-samples.md).
