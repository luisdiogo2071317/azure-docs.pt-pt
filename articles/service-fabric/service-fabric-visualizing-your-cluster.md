---
title: Visualizar o cluster utilizando o Azure Service Fabric Explorer | Microsoft Docs
description: "Explorador do Service Fabric é uma aplicação para inspecionar e gerir aplicações em nuvem e de nós num cluster do Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: mikhegn
ms.openlocfilehash: 34e00058591bc5a0a02bc408cfc3fcc11010f17c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualizar o cluster com o Service Fabric Explorer

Explorador de recursos de infraestrutura de serviço (SFX) é uma ferramenta de open source para inspecionar e gerir clusters de Service Fabric do Azure. Explorador do Service Fabric é uma aplicação de ambiente de trabalho para o Windows e Linux. Suporte para MacOS estará disponível brevemente.

## <a name="service-fabric-explorer-download"></a>Transferências de Service Fabric Explorer

Utilize as seguintes ligações para transferir Service Fabric Explorer como uma aplicação de ambiente de trabalho:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

> [!NOTE]
> A versão de ambiente de trabalho do Service Fabric Explorer, pode ter mais ou menos funcionalidades que o suporte de cluster. Pode reverter para a versão de Service Fabric Explorer implementada para o cluster para garantir a compatibilidade de funcionalidade completa.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Com o Service Fabric Explorer do cluster

Service Fabric Explorer também está alojado num ponto final de gestão de HTTP de um cluster de Service Fabric. Para iniciar SFX num browser, navegue até ao ponto final de gestão de HTTP do cluster a partir de qualquer browser - por exemplo https://clusterFQDN:19080.

Para a configuração da estação de trabalho de programador, pode iniciar o Explorador do Service Fabric no seu cluster local, navegando até http://localhost:19080/Explorer. Observe este artigo para [preparar o ambiente de desenvolvimento](service-fabric-get-started.md).

## <a name="connect-to-a-service-fabric-cluster"></a>Ligar a um cluster do Service Fabric
Para ligar a um cluster do Service Fabric, terá do ponto de final de gestão de clusters (FQDN/IP) e a porta de ponto final de gestão de HTTP (19080 por predefinição). Por exemplo, https://mysfcluster.westus.cloudapp.azure.com:19080. Utilize a caixa de verificação "Ligar ao localhost" para ligar a um cluster local na sua estação de trabalho.

### <a name="connect-to-a-secure-cluster"></a>Ligar a um cluster seguro
Pode controlar o acesso de cliente ao cluster do Service Fabric com certificados ou utilizar o Azure Active Directory (AAD).

Se tentar ligar a um cluster seguro, em seguida, consoante a configuração do cluster será necessário para apresentar um certificado de cliente ou inicie sessão com o AAD.

## <a name="video-tutorial"></a>Tutorial de vídeo

Para saber como utilizar o Service Fabric Explorer, veja o vídeo seguinte do Microsoft Virtual Academy:

> [!NOTE]
> Este vídeo mostra que Service Fabric Explorer alojadas num cluster de Service Fabric, não a versão de ambiente de trabalho.
>
>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="understand-the-service-fabric-explorer-layout"></a>Compreender o esquema de Service Fabric Explorer
Pode navegar através do Explorador de recursos de infraestrutura de serviço através da utilização de árvore à esquerda. Na raiz da árvore, o dashboard do cluster fornece uma descrição geral do cluster, incluindo um resumo das aplicações e estado de funcionamento do nó.

![Dashboard de cluster do Service Fabric Explorer][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Ver o esquema do cluster
Nós num cluster de Service Fabric são colocados entre uma grelha two-dimensional de domínios de falhas e domínios de atualização. Esta colocação assegura que as aplicações permanecem disponíveis na presença de falhas de hardware e as atualizações de aplicações. Pode ver o modo como o cluster atual é disposto utilizando o mapa de cluster.

![Mapa de cluster do Service Fabric Explorer][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Ver aplicações e serviços
O cluster contém dois subárvores: um para aplicações e outra para nós.

Pode utilizar a vista da aplicação para navegar através da hierarquia de lógica de Service Fabric: aplicações, serviços, partições e réplicas.

No exemplo abaixo, a aplicação **MyApp** é composta por dois serviços, **MyStatefulService** e **WebService**. Uma vez que **MyStatefulService** tem o estado monitorizado, inclui uma partição com um site primário e de duas réplicas secundárias. Por outro lado, WebSvcService é sem monitorização de estado e contém uma única instância.

![Vista de aplicação de Service Fabric Explorer][sfx-application-tree]

Cada nível de árvore, o painel principal mostra pertinentes informações sobre o item. Por exemplo, pode ver o estado de funcionamento e a versão para um serviço específico.

![Painel do Service Fabric Explorer essentials][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Visualizar os nós do cluster
A vista do nó mostra o esquema físico do cluster. Para um determinado nó, pode inspecionar as aplicações que têm um código implementado nesse nó. Mais especificamente, pode ver as réplicas estão atualmente em execução não existe.

## <a name="actions"></a>Ações
Service Fabric Explorer proporciona uma forma rápida de invocar ações em nós, aplicações e serviços no seu cluster.

Por exemplo, para eliminar uma instância de aplicação, escolha a aplicação de árvore à esquerda e, em seguida, escolha **ações** > **eliminar aplicação**.

![Eliminar uma aplicação no Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> Pode efetuar as mesmas ações clicando no botão de reticências junto a cada elemento.
>
> Cada ação que pode ser realizada através do Service Fabric Explorer também pode ser efetuada através do PowerShell ou uma API REST, para ativar a automatização.
>
>

Também pode utilizar o Service Fabric Explorer para criar instâncias da aplicação para um tipo de aplicação e versão. Escolha o tipo de aplicação na vista de árvore, em seguida, clique em de **criar instância de aplicação** ligação junto a versão que pretende no painel direito.

![Criar uma instância de aplicação no Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Explorador de recursos de infraestrutura de serviço não suporta parâmetros durante a criação de instâncias da aplicação. Instâncias de aplicações utilizam valores de parâmetros de predefinição.
>
>

## <a name="next-steps"></a>Passos Seguintes
* [Gerir as aplicações de Service Fabric no Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implementação de aplicação de Service Fabric com o PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png