---
title: Azure Service Fabric CLI Script de exemplo remover
description: "Remover uma aplicação de um cluster do Service Fabric do Azure utilizando a CLI do Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 95a6bac7ee79e3490584305e78d410104363b780
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Remover uma aplicação de um cluster do Service Fabric

Este script de exemplo elimina uma instância de aplicação de Service Fabric em execução, anula o registo de um tipo de aplicação e a versão do cluster.  A eliminar a instância da aplicação também elimina todos os a execução do serviço instâncias associadas com essa aplicação. Em seguida, são eliminados os ficheiros da aplicação da loja de imagem. 

Se necessário, instale o [CLI de recursos de infraestrutura de serviço](../service-fabric-cli.md).

## <a name="sample-script"></a>Script de exemplo

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações, consulte o [documentação do Service Fabric CLI](../service-fabric-cli.md).

Amostras de CLI de recursos de infraestrutura de serviço adicionais para o Azure Service Fabric podem ser encontradas no [exemplos de Service Fabric CLI](../samples-cli.md).
