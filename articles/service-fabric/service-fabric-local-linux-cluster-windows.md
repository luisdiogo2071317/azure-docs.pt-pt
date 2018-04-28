---
title: Configurar o cluster do Azure Service Fabric Linux no Windows | Microsoft Docs
description: Este artigo abrange como configurar clusters de Service Fabric Linux em execução em máquinas de desenvolvimento do Windows. Isto é particularmente útil para cruzada desenvolvimento de plataforma.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: 89c1cf36c3b92376dedb1cb29d190c4c6d8f619b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Configurar um cluster de Linux Service Fabric no seu computador de programador do Windows

Este documento abrange como configurar uma infraestrutura de serviço local do Linux em máquinas de desenvolvimento do Windows. Configurar um cluster local do Linux é útil para testar aplicações direcionadas para os clusters do Linux rapidamente, mas são desenvolvidas num computador Windows.

## <a name="prerequisites"></a>Pré-requisitos
Clusters de Service Fabric baseados em Linux não executam nativamente no Windows. Para executar um cluster de Service Fabric local, é fornecida uma imagem de contentor do Docker previamente configurada. Antes de começar, vai precisar do:

* De, pelo menos, 4 GB de RAM
* Da versão mais recente do [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)

>[!TIP]
> * Pode seguir os passos mencionados o Docker oficial [documentação](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) para instalar o Docker no seu Windows. 
> * Depois de concluída a instalação, execute os passos mencionados [aqui](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) para confirmar que foi instalada corretamente.


## <a name="create-a-local-container-and-setup-service-fabric"></a>Criar um contentor local e configurar o Service Fabric
Para configurar um contentor de Docker local e tiver um cluster do service fabric em execução no mesmo, execute os seguintes passos no PowerShell:

1. Extrair a imagem do repositório de hubs do Docker:

    ```powershell
    docker pull microsoft/service-fabric-onebox
    ```

2. Atualize a configuração do daemon do Docker no seu anfitrião com o seguinte e reinicie-o: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    A forma advised atualizar é - vá para o ícone de Docker > Definições > Daemon > Avançadas e atualizá-lo não existe. Em seguida, reinicie o daemon de Docker para que as alterações entrem em vigor. 

3. Iniciar uma instância de contentor do Service Fabric One-box com a imagem:

    ```powershell
    docker run -itd -p 19080:19080 --name sfonebox microsoft/service-fabric-onebox
    ```
    >[!TIP]
    > * Especificar um nome para a instância do contentor permite-lhe processá-la de forma mais legível. 
    > * Se a aplicação estiver a escutar em determinadas portas, tem de ser especificada com etiquetas -p adicionais. Por exemplo, se a sua aplicação em escuta na porta 8080, execução docker run 8080:8080 de -p de 19080:19080 -p - itd – nome sfonebox microsoft/service-infraestrutura-onebox

4. Inicie sessão no contentor do Docker no modo ssh interativo:

    ```powershell
    docker exec -it sfonebox bash
    ```

5. Execute o script de configuração, o qual vai obter as dependências necessárias e, depois, iniciar o cluster no contentor.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

6. Passo 5 esteja concluída com êxito, pode aceder à ``http://localhost:19080`` do seu Windows e seria capaz de ver o Explorador do Service Fabric. Neste momento, pode ligar a este cluster utilizar quaisquer ferramentas a partir do seu computador de programador do Windows e implementar aplicações direcionadas para os clusters do Linux Service Fabric. 

    > [!NOTE]
    > O plug-in do Eclipse não é atualmente suportado no Windows. 

## <a name="next-steps"></a>Passos Seguintes
* Introdução ao [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Veja outros [exemplos de Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png