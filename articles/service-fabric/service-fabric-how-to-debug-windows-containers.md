---
title: Depurar os contentores do Windows com o Service Fabric e o VS | Documentos da Microsoft
description: Saiba como depurar os contentores do Windows no Azure Service Fabric com o Visual Studio 2017.
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
ms.openlocfilehash: 437c38a8e674fcdf06e26a7191ceecef9d901470
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968325"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Como: depurar contentores Windows no Azure Service Fabric com o Visual Studio 2017

Com o Visual Studio 2017 atualização 7 (15.7), pode depurar aplicações de .NET em contentores como serviços do Service Fabric. Este artigo mostra-lhe como configurar o seu ambiente e, em seguida, depurar uma aplicação .NET num contentor em execução no cluster do Service Fabric local.

## <a name="prerequisites"></a>Pré-requisitos

* No Windows 10, siga este guia de introdução para [configurar o Windows 10 para executar contentores do Windows](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* No Windows Server 2016, siga este guia de introdução para [configurar o Windows 2016 para executar contentores do Windows](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Configurar o ambiente de Service Fabric local, seguindo [preparar o ambiente de desenvolvimento no Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Configurar o seu ambiente de desenvolvimento para depurar contentores

1. Certificar-se de que o Docker para o serviço do Windows está em execução antes de continuar com a próxima etapa.

1. Para oferecer suporte à resolução de DNS entre contentores, terá de configurar o cluster de desenvolvimento local, com o nome da máquina.
    1. Abra PowerShell como administrador
    1. Navegue para a pasta de configuração de Cluster de SDK, normalmente `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`
    1. Execute o script `DevClusterSetup.ps1` com o parâmetro `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > Pode utilizar o `-CreateOneNodeCluster` para configurar um cluster de um nó. A predefinição criará um cluster de cinco nós local.
    >

    Para saber mais sobre o serviço de DNS no Service Fabric, veja [serviço de DNS no Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Limitações conhecidas ao depurar contentores no Service Fabric

Segue-se uma lista de limitações conhecidas com depuração contentores no Service Fabric e resoluções possíveis:

* Utilizar localhost para ClusterFQDNorIP não suporta a resolução de DNS em contentores.
    * Resolução: Configurar o cluster local com o nome da máquina (consultar acima)
* A executar o Windows 10 numa máquina Virtual não irá obter resposta DNS para o contentor.
    * Resolução: Desative a descarga da soma de verificação UDP para IPv4 na NIC de máquinas virtuais
    * Tenha em atenção de que isso irá degradar o desempenho de rede na máquina.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Serviços no mesmo aplicativo através de DNS a resolver o nome de serviço não funciona no Windows 10, se a aplicação foi implementada com o Docker Compose
    * Resolução: Utilizar servicename.applicationname para resolver pontos finais de serviço
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Se utilizar o endereço IP para ClusterFQDNorIP, a alteração de IP primária no anfitrião irá interromper a funcionalidade DNS.
    * Resolução: Recrie o cluster utilizando o novo IP primário no anfitrião ou utilize o nome da máquina. Esta ação é propositada.
* Se o FQDN do cluster foi criado com não puder ser resolvido na rede, o DNS irá falhar.
    * Resolução: Recrie o cluster local com o IP primário do anfitrião. Esta ação é propositada.
* Ao depurar um contentor, registos do docker só estará disponíveis na janela de saída do Visual Studio, não através de APIs de recursos de infraestrutura do serviço, incluindo o Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Depurar um aplicativo .NET executado em contentores do docker no Service Fabric

1. Execute o Visual Studio como administrador.

1. Abrir uma aplicação .NET existente ou crie um novo.

1. Com o botão direito no projeto e selecione **adicionar -> suporte do Orchestrator de contentor -> Service Fabric**

1. Prima **F5** para iniciar a depuração da aplicação.

    Visual Studio oferece suporte a consola e tipos de projeto do ASP.NET para .NET e .NET Core.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as funcionalidades do Service Fabric e contentores, siga esta ligação: [descrição geral de contentores do Service Fabric](service-fabric-containers-overview.md).