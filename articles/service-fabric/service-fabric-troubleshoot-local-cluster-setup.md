---
title: Resolver problemas relacionados com a configuração de cluster do Azure Service Fabric local | Microsoft Docs
description: Este artigo aborda um conjunto de sugestões de resolução de problemas de cluster de desenvolvimento local
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: a7f58914fd6e498e717e19bfea11c9e3fcfc0399
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Resolver problemas relacionados com a configuração de cluster de desenvolvimento local
Caso se depare com um problema ao interagir com o cluster de desenvolvimento local do Azure Service Fabric, reveja as seguintes sugestões para possíveis soluções.

## <a name="cluster-setup-failures"></a>Falhas de configuração de cluster
### <a name="cannot-clean-up-service-fabric-logs"></a>Não é possível limpar os registos de Service Fabric
#### <a name="problem"></a>Problema
Ao executar o script de DevClusterSetup, consulte o erro seguinte:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Solução
Feche a janela do PowerShell atual e abrir uma nova janela do PowerShell como administrador. Agora pode executar o script com êxito.

## <a name="cluster-connection-failures"></a>Falhas de ligação de cluster

### <a name="type-initialization-exception"></a>Exceção de inicialização do tipo
#### <a name="problem"></a>Problema
Quando estiverem a ligar ao cluster no PowerShell, consulte o erro TypeInitializationException para System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Solução
A variável de caminho não foi corretamente definida durante a instalação. Terminar a sessão do Windows e inicie sessão novamente. Isto atualiza o seu caminho.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Falha da ligação de cluster com "Objeto está fechado"
#### <a name="problem"></a>Problema
Uma chamada para Connect-ServiceFabricCluster falha com o erro seguinte:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Solução
Feche a janela do PowerShell atual e abrir uma nova janela do PowerShell como administrador.

### <a name="fabric-connection-denied-exception"></a>Excepção de ligação negado de recursos de infraestrutura
#### <a name="problem"></a>Problema
Quando a depuração do Visual Studio, receberá um erro de FabricConnectionDeniedException.

#### <a name="solution"></a>Solução
Normalmente, este erro ocorre quando tenta iniciar manualmente a um processo de anfitrião do serviço.

Certifique-se de que não têm qualquer projetos do serviço definido como projetos de arranque na sua solução. Projetos de aplicação de Service Fabric só devem ser definidos como projetos de arranque.

> [!TIP]
> Se, a seguir o programa de configuração, o cluster local começa a comportar-se muito, pode repô-lo utilizando a aplicação de tabuleiro de sistema de Gestor de local cluster. Esta ação remove o cluster existente e configurar um novo. Tenha em atenção que todas as aplicações implementadas e dados associados são removidos.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
* [Compreender e resolver problemas de cluster com relatórios de estado de funcionamento do sistema](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Visualizar o cluster com o Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

