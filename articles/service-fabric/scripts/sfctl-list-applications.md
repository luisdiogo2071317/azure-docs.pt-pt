---
title: Exemplo de Script da CLI do Service Fabric - Listar aplicações num cluster
description: Exemplo de Script da CLI do Service Fabric - Listar as aplicações aprovisionadas num cluster do Service Fabric.
services: service-fabric
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/13/2018
ms.author: adegeo
ms.custom: ''
ms.openlocfilehash: 2a58a9d534116dbdd486f1319f3d87e1d2ba24ef
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642772"
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>Listar as aplicações a executar num cluster do Service Fabric

Este script de exemplo liga-se a um cluster do Service Fabric e, em seguida, lista todas as aplicações aprovisionadas.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-sh[main](../../../cli_scripts/service-fabric/list-application/list-application.sh "List provisioned applications from a cluster")]

## <a name="next-steps"></a>Passos seguintes

Para mais informações, veja a [documentação da CLI do Service Fabric](../service-fabric-cli.md).

Podem ser encontrados exemplos adicionais da CLI do Service Fabric para o Azure Service Fabric em [Exemplos da CLI do Service Fabric](../samples-cli.md).
