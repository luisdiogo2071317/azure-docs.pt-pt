---
title: Depurar contentores do Windows com o Service Fabric e VS | Microsoft Docs
description: Saiba como depurar a contentores do Windows no Azure Service Fabric com o Visual Studio 2017.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/14/2018
ms.author: mikhegn
ms.openlocfilehash: bca33fe187668d38d4451b2de5b9e54d86e40ba9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655753"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Como: depurar contentores do Windows no Azure Service Fabric com o Visual Studio 2017

Com o Visual Studio 2017 atualização 7 (15.7), pode depurar aplicações de .NET nos contentores como serviços do Service Fabric. Este artigo mostra como configurar o seu ambiente e, em seguida, depurar uma aplicação .NET num contentor em execução num cluster de Service Fabric local.

## <a name="prerequisites"></a>Pré-requisitos

* No Windows 10, seguir este guia de introdução para [configurar o Windows 10 para executar os contentores do Windows](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* No Windows Server 2016, seguir este guia de introdução para [configurar 2016 do Windows para executar os contentores do Windows](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Configurar o ambiente de Service Fabric local, seguindo [preparar o ambiente de desenvolvimento do Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Configurar o ambiente do programador para depurar contentores

1. Certifique-se de que docker para o serviço de janela está a ser executado antes de continuar com o passo seguinte.

1. Para suportar a resolução DNS entre contentores, terá que configurar o cluster de desenvolvimento local, utilizando o nome da máquina.
    1. Abra PowerShell como administrador
    1. Navegue para a pasta de configuração de Cluster do SDK, normalmente `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`
    1. Execute o script `DevClusterSetup.ps1` com o parâmetro `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > Pode utilizar o `-CreateOneNodeCluster` para um cluster de um nó de configuração. A predefinição irá criar um cluster de cinco nós local.
    >

    Para saber mais sobre o serviço de DNS no Service Fabric, consulte [serviço DNS do Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-dnsservice).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Limitações conhecidas quando a depuração de contentores no Service Fabric

Segue-se uma lista de limitações conhecidas com contentores de depuração no Service Fabric e resoluções possíveis:

* Utilizar localhost para ClusterFQDNorIP não suportarão a resolução de DNS nos contentores.
    * Resolução: Configurar o cluster local com o nome do computador (consultar acima)
* Windows10 a ser executado numa máquina Virtual não irá obter resposta DNS para o contentor.
    * Resolução: Desativar a descarga da soma de verificação de UDP para IPv4 no NIC máquinas virtuais
    * Tenha em atenção de que este irá degradar o desempenho de rede na máquina.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Serviços na mesma aplicação utilizando o DNS de resolver o nome do serviço não funciona em Windows10, se a aplicação foi implementada utilizando o Docker Compose
    * Resolução: Utilizar servicename.applicationname resolver pontos finais de serviço
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Se utilizar o endereço IP para ClusterFQDNorIP, a alteração de IP primária no anfitrião irá interromper a funcionalidade DNS.
    * Resolução: Recrie o cluster utilizando o novo IP primário no anfitrião ou utilize o nome da máquina. Esta ação é propositada.
* Se o FQDN do cluster foi criado com não é resolvível na rede, o DNS falhará.
    * Resolução: Recrie o cluster local com o IP principal do anfitrião. Esta ação é propositada.
* Quando um contentor de depuração, registos de docker só estará disponíveis na janela de saída do Visual Studio, não através de APIs de recursos de infraestrutura de serviço, incluindo o Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Depurar uma aplicação .NET em execução nos contentores do docker no Service Fabric

1. Execute o Visual Studio como administrador.

1. Abrir uma aplicação .NET existente ou crie um novo.

1. Clique com o botão direito no projeto e selecione **adicionar -> contentor Orchestrator suporte -> Service Fabric**

1. Prima **F5** para iniciar a depuração da aplicação.

    Visual Studio suporta a consola e tipos de projeto do ASP.NET para o .NET e .NET Core.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as funcionalidades do Service Fabric e contentores, siga esta ligação: [descrição geral do Service Fabric contentores](service-fabric-containers-overview.md).