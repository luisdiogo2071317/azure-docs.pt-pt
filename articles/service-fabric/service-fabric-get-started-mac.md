---
title: Configurar o ambiente de desenvolvimento no Mac OS X para trabalhar com o Azure Service Fabric | Microsoft Docs
description: "Instale o runtime, o SDK e as ferramentas e crie um cluster de desenvolvimento local. Depois de concluir esta configuração, estará pronto para criar aplicações no Mac OS X."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/17/2017
ms.author: saysa
ms.openlocfilehash: 328b2778a68e32d95b666124bf7bba969a5f52a6
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Configurar o ambiente de desenvolvimento no Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Pode criar aplicações do Azure Service Fabric para serem executadas em clusters do Linux através do Mac OS X. Este documento descreve como configurar o seu Mac para desenvolvimento.

## <a name="prerequisites"></a>Pré-requisitos
O Azure Service Fabric não é executado de forma nativa no Mac OS X. Para executar um cluster do Service Fabric local, é fornecida uma imagem de contentor do Docker pré-configurada. Antes de começar, vai precisar do:

* De, pelo menos, 4 GB de RAM.
* A versão mais recente do [Docker](https://www.docker.com/).
* Aceda à [imagem de contentor onebox do Docker](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/) do Service Fabric.

>[!TIP]
>
>Para instalar o Docker no seu Mac, siga os passos na [documentação do Docker](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install). Depois de instalar, [verifique a instalação](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine).
>

## <a name="create-a-local-container-and-set-up-service-fabric"></a>Criar um contentor local e configurar o Service Fabric
Para configurar um contentor do Docker local e executar um cluster do Service Fabric no mesmo, realize os passos seguintes:

1. Solicitar a imagem de contentor do onebox do Service Fabric a partir do repositório de hub do Docker:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Atualize a configuração do daemon do Docker no seu anfitrião com as seguintes definições e reinicie-o: 

    ```json
    {
        "ipv6": true,
        "fixed-cidr-v6": "fd00::/64"
    }
    ```
    Pode atualizar estas definições diretamente no ficheiro daemon.json no caminho de instalação do Docker.
    
    >[!NOTE]
    >
    >A localização do ficheiro daemon.json pode variar de máquina para máquina. Por exemplo, ~/Library/Containers/com.docker.docker/Data/database/com.docker.driver.amd64-linux/etc/docker/daemon.json.
    >
    >A abordagem recomendada é modificar diretamente as definições de configuração do daemon no Docker. Selecione o **ícone de Docker**e, em seguida, selecione **Preferências** > **Daemon** > **Avançadas**.
    >

3. Inicie uma instância do contentor do onebox do Service Fabric e utilize a imagem solicitada no primeiro passo:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Forneça um nome para a instância do contentor para poder processá-la de forma mais legível. 
    >
    >Se a aplicação estiver a escutar em determinadas portas, as portas têm de ser especificada através da utilização de etiquetas `-p` adicionais. Por exemplo, se a sua aplicação estiver em escuta na porta 8080, adicione a seguinte etiqueta `-p`:
    >
    >`run docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox servicefabricoss/service-fabric-onebox`
    >

4. Inicie sessão no contentor do Docker no modo SSH interativo:

    ```bash
    docker exec -it sfonebox bash
    ```

5. Execute o script de configuração para obter as dependências necessárias e inicie o cluster no contentor:

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

6. Após a conclusão do passo 5, navegue até à `http://localhost:19080` a partir do seu Mac. Deverá ver o explorador do Service Fabric.

## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>Configurar a CLI do Service Fabric (sfctl) no seu Mac

Siga as instruções em [Service Fabric CLI](service-fabric-cli.md#cli-mac) para instalar a CLI do Service Fabric (`sfctl`) no seu Mac.
Os comandos da CLI suportam a interação com as entidades do Service Fabric, incluindo clusters, aplicações e serviços.

## <a name="create-your-application-on-your-mac-by-using-yeoman"></a>Criar uma aplicação no seu Mac com o Yeoman

O Service Fabric fornece ferramentas estruturais que o ajudam a criar uma aplicação do Service Fabric a partir do terminal, utilizando o gerador de modelos Yeoman. Utilize os seguintes passos para confirmar que o gerador de modelos Yeoman do Service Fabric está a funcionar na sua máquina:

1. O Node.js e o Node Package Manager (NPM) têm de estar instalados no seu Mac. O software pode ser instalado utilizando o [HomeBrew](https://brew.sh/), da seguinte forma:

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Instalar o gerador de modelos [Yeoman](http://yeoman.io/) no seu computador a partir do NPM:

    ```bash
    npm install -g yo
    ```
3. Instale o gerador Yeoman que prefira seguindo os passos na [documentação](service-fabric-get-started-linux.md) da introdução. Para criar aplicações do Service Fabric utilizando o Yeoman, siga estes passos:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Para criar uma aplicação Java do Service Fabric no Mac, o JDK versão 1.8 e o Gradle devem estar instalados na máquina anfitriã. O software pode ser instalado utilizando o [HomeBrew](https://brew.sh/), da seguinte forma: 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-your-application-on-your-mac-from-the-terminal"></a>Implementar a aplicação no seu Mac a partir do terminal

Depois de criar e compilar a aplicação do Service Fabric, pode implementá-la com a [CLI do Service Fabric](service-fabric-cli.md#cli-mac):

1. Ligue ao cluster do Service Fabric que está em execução dentro da instância de contentor no seu Mac:

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. A partir do diretório do seu projeto, execute o script de instalação:

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>Configurar o desenvolvimento em .NET Core 2.0

Instale o [.NET Core 2.0 SDK for Mac](https://www.microsoft.com/net/core#macos) para começar a [criar aplicações do Service Fabric em C#](service-fabric-create-your-first-linux-application-with-csharp.md). Os pacotes para aplicações do Service Fabric do .NET Core 2.0 são alojados no NuGet.org, que está atualmente em pré-visualização.

## <a name="install-the-service-fabric-plug-in-for-eclipse-neon-on-your-mac"></a>Instalar o plug-in do Service Fabric para Eclipse Neon no seu Mac

Fornece um plug-in do Azure Service Fabric para o Eclipse Neon para o IDE Java. O plug-in simplifica o processo de criação e implementação de serviços Java. Para instalar ou atualizar o plug-in do Service Fabric para Eclipse para a versão mais recente, siga [estes passos](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon). Os outros passos na [documentação do Service Fabric para o Eclipse](service-fabric-get-started-eclipse.md) também são aplicáveis: criar uma aplicação, adicionar um serviço a uma aplicação, desinstalar uma aplicação e assim sucessivamente.

O último passo consiste em instanciar o contentor com um caminho que é partilhado com o anfitrião. O plug-in requer este tipo de instanciação para trabalhar com o contentor do Docker no seu Mac. Por exemplo:

```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```

Os atributos são definidos do seguinte modo:
* `/Users/sayantan/work/workspaces/mySFWorkspace` é o caminho completamente qualificado da área de trabalho no seu Mac.
* `/tmp/mySFWorkspace` é o caminho que se encontra dentro do contentor para onde a área de trabalho deve ser mapeada.

>[!NOTE]
> 
>Se tiver um nome/caminho diferente para a sua área de trabalho, atualize estes valores no comando `docker run`.
> 
>Se iniciar o contentor com um nome que não seja `sfonebox`, atualize o valor de nome no ficheiro testclient.sh na sua aplicação Java de atores do Service Fabric.
>

## <a name="next-steps"></a>Passos seguintes
<!-- Links -->
* [Criar e implementar a sua primeira aplicação Java do Service Fabric no Linux com o Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Criar e implementar a sua primeira aplicação Java do Service Fabric no Linux com o plug-in do Service Fabric para Eclipse](service-fabric-get-started-eclipse.md)
* [Create a Service Fabric cluster in the Azure portal (Criar um cluster do Service Fabric no portal do Azure)](service-fabric-cluster-creation-via-portal.md)
* [Create a Service Fabric cluster by using Azure Resource Manager (Criar um cluster do Service Fabric com o Azure Resource Manager)](service-fabric-cluster-creation-via-arm.md)
* [Understand the Service Fabric application model (Compreender o modelo de aplicações do Service Fabric)](service-fabric-application-model.md)
* [Utilizar a CLI do Service Fabric para gerir as aplicações](service-fabric-application-lifecycle-sfctl.md)
* [Prepare a Linux development environment on Windows](service-fabric-local-linux-cluster-windows.md) (Preparar um ambiente de desenvolvimento do Linux no Windows)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
