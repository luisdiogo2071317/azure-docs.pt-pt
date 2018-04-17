---
title: Ver registos de contentores no Service Fabric do Azure | Microsoft Docs
description: Descreve como visualizar os registos do contentor para um serviços de contentor do Service Fabric em execução com o Service Fabric Explorer.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: 48ee54460454368deef44c8f84624e32856efafa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Ver registos para um serviço de contentor do Service Fabric
Azure Service Fabric é orchestrator um contentor e suporta [contentores de Linux e Windows](service-fabric-containers-overview.md).  Este artigo descreve como ver registos do contentor de um serviço de contentor em execução para que possam diagnosticar e resolver problemas.

## <a name="access-container-logs"></a>Registos do contentor de acesso
Os registos do contentor podem ser acedidos utilizando [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Num browser, abrir Service Fabric Explorer a partir do ponto final de gestão do cluster, navegando até [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Contentor registos estão localizados no nó de cluster que a instância do serviço de contentor está em execução. Por exemplo, obter os registos do contentor web front-end do [votar do Linux na aplicação de exemplo](service-fabric-quickstart-containers-linux.md). Na vista de árvore, expanda **Cluster**>**aplicações**>**VotingType**>**fabric: / voto / azurevotefront**.  Em seguida, expanda a partição (d1aa737e-f22a-e347-be16-eec90be24bc1, neste exemplo) e que o contentor está em execução no nó de cluster *_lnxvm_0*.

Na vista de árvore, localize o pacote do código no *_lnxvm_0* nó expandindo **nós**>**_lnxvm_0**>**fabric: / votar**  > **azurevotfrontPkg**>**código pacotes**>**código**.  Em seguida, selecione o **registos do contentor** opção para apresentar os registos do contentor.

![Plataforma do Service Fabric][Image1]


## <a name="next-steps"></a>Passos Seguintes
- Completar o [criar um tutorial de aplicação do Linux contentor](service-fabric-tutorial-create-container-images.md).
- Saiba mais sobre [Service Fabric e os contentores](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
