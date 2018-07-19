---
title: Exemplo de Script da CLI do Service Fabric - Atualizar uma aplicação num cluster
description: Exemplo de Script da CLI do Service Fabric - Atualizar uma aplicação com uma nova versão. Este exemplo também atualiza uma aplicação implementada com as novas partes.
services: service-fabric
documentationcenter: ''
author: TylerMSFT
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: twhitney
ms.custom: ''
ms.openlocfilehash: bd0941cc6e67a759dd5fcb2e8912f8fcd6e9b18c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071169"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Adicionar um certificado de aplicação a um cluster do Service Fabric

Este script de exemplo carrega uma nova versão de uma aplicação existente e então atualiza uma aplicação implementada com as novas partes.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Passos seguintes

Para mais informações, veja a [documentação da CLI do Service Fabric](../service-fabric-cli.md).

Podem ser encontrados exemplos adicionais da CLI do Service Fabric para o Azure Service Fabric em [Exemplos da CLI do Service Fabric](../samples-cli.md).
