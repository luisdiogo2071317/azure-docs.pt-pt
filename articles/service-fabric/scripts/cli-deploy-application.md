---
title: Exemplo de implementação do Script da CLI do Service Fabric do Azure (sfctl)
description: Implementar uma aplicação num cluster do Service Fabric do Azure com a CLI do Service Fabric do Azure
services: service-fabric
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/16/2018
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 7e2bd212e88b3efcf7f3aad3ef1555b3fc48ef1b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641735"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Implementar uma aplicação num cluster do Service Fabric

Este script de exemplo copia um pacote de aplicações para um arquivo de imagens do cluster, regista o tipo de aplicação no cluster e cria uma instância de aplicação no tipo de aplicação. Todos os serviços predefinidos também são criados nesta fase.

Se necessário, instale a [CLI do Service Fabric](../service-fabric-cli.md).

## <a name="sample-script"></a>Script de exemplo

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Quando terminar, o script [remover](cli-remove-application.md) pode ser utilizado para remover a aplicação. O script remover elimina a instância de aplicação, anula o registo do tipo de aplicação e elimina o pacote de aplicações do arquivo de imagens.

## <a name="next-steps"></a>Passos seguintes

Para mais informações, veja a [documentação da CLI do Service Fabric](../service-fabric-cli.md).

Podem ser encontrados exemplos adicionais da CLI do Service Fabric para o Azure Service Fabric em [Exemplos da CLI do Service Fabric](../samples-cli.md).
