---
title: "Exemplo de implementação do Script de CLI de recursos de infraestrutura de serviço do Azure"
description: Crie um cluster de Service Fabric Linux seguro no Azure utilizando a CLI do Azure Service Fabric.
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
ms.openlocfilehash: d49383b4f7b3b13beb9ea36ae725938e17ef1456
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Criar um cluster de Service Fabric Linux seguro no Azure

Este comando cria um certificado autoassinado, adiciona-o para um cofre de chaves e transfere o certificado localmente.  O novo certificado é utilizado para proteger o cluster quando implementa.  Também pode utilizar um certificado existente em vez de criar um novo.  Qualquer forma, o nome do requerente do certificado tem de corresponder ao domínio que utilizar para aceder ao cluster do Service Fabric. Esta correspondência é obrigada a fornecer um SSL para o cluster pontos finais de gestão HTTPS e Service Fabric Explorer. Não é possível obter um certificado SSL a partir de uma AC para o `.cloudapp.azure.com` domínio. Tem de obter um nome de domínio personalizado para o cluster. Quando solicitar um certificado a partir de uma AC, o nome de requerente do certificado tem de corresponder ao nome de domínio personalizado que utilizar para o cluster.

Se necessário, instale o [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Script de exemplo

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, o seguinte comando pode ser utilizado para remover o grupo de recursos, cluster e todos os recursos relacionados.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Cria um novo cluster de Service Fabric.  |

## <a name="next-steps"></a>Passos seguintes

Amostras de CLI de recursos de infraestrutura de serviço adicionais para o Azure Service Fabric podem ser encontradas no [exemplos de Service Fabric CLI](../samples-cli.md).
