---
title: Configurar um ambiente de desenvolvimento Windows para compilar aplicações do Service Fabric Mesh | Microsoft Docs
description: Configure o seu ambiente de desenvolvimento Windows para que possa criar uma aplicação do Service Fabric Mesh e implementá-la no Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 11/29/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: bec0b9a7e34f1577f80a99f5380795c479c04bc8
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890471"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Configurar o ambiente de desenvolvimento Windows para compilar aplicações do Service Fabric Mesh

Para compilar e executar aplicações do Azure Service Fabric Mesh no computador de desenvolvimento Windows, tem de instalar o runtime do Service Fabric Mesh, o SDK e as ferramentas.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Versões de sistema operativo com suporte

As seguintes versões do sistema operativo são suportadas para desenvolvimento:

* Windows 10 (Enterprise, Professional ou Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

Precisa do Visual Studio 2017 para implementar aplicações do Service Fabric Mesh. [Instale a versão 15.6.0][download-visual-studio] ou superior e ative as seguintes cargas de trabalho:

* Desenvolvimento ASP.NET e Web
* Desenvolvimento do Azure

## <a name="install-docker"></a>Instalar o Docker

#### <a name="windows-10"></a>Windows 10

Transfira e instale a versão mais recente do [Docker Community Edition para Windows][download-docker] para suportar as aplicações do Service Fabric em contentores utilizadas pelo Service Fabric Mesh.

Durante a instalação, selecione **Utilizar contentores do Windows em vez de contentores do Linux** quando lhe for pedido.

Se o Hyper-V não estiver ativado no seu computador, o instalador do Docker poderá ativá-lo. Clique em **OK** para fazer isso, se lhe for pedido.

#### <a name="windows-server-2016"></a>Windows Server 2016

Se não tiver a função do Hyper-V ativada, abra o PowerShell como administrador e execute o seguinte comando para ativar o Hyper-V e, em seguida, reinicie o computador. Para obter mais informações, veja [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Reinicie o computador.

Abra o PowerShell como administrador e execute os seguintes comandos para instalar o Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK e ferramentas

Instale o runtime, o SDK e as ferramentas do Service Fabric Mesh pela ordem seguinte.

1. Instale o [SDK do Service Fabric Mesh][download-sdkmesh] através do Instalador de Plataforma Web. Também será instalado o SDK e o runtime do Microsoft Azure Service Fabric.
2. Instale a [extensão Ferramentas do Service Fabric Mesh para Visual Studio (pré-visualização)][download-tools] a partir do Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Compilar um cluster

> [!IMPORTANT]
> O Docker **tem** estar em execução antes de poder compilar um cluster.
> Teste se o Docker está em execução ao abrir uma janela de terminal e ao executar `docker ps` para ver se ocorre um erro. Se a resposta não indicar um erro, o Docker está a ser executado e está pronto para compilar um cluster.

Se estiver a utilizar o Visual Studio, pode ignorar esta secção porque o Visual Studio vai criar um cluster local, caso não tenha um.

Para obter o melhor desempenho de depuração ao criar e executar aplicações do Service Fabric, recomendamos que crie um cluster de desenvolvimento de nó único local. Este cluster tem de estar em execução sempre que implementar ou depurar um projeto do Service Fabric Mesh.

Depois de instalar o tempo de execução, SDKs, ferramentas do Visual Studio, Docker e ter em execução do Docker, crie um cluster de desenvolvimento.

1. Feche a janela do PowerShell.
2. Abra uma nova janela do PowerShell elevada como administrador. Este passo é necessário para carregar os módulos do Service Fabric instalados recentemente.
3. Execute o seguinte comando do PowerShell para criar um cluster de desenvolvimento:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Para iniciar a ferramenta de gestor de clusters locais, execute o seguinte comando do PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Assim que a ferramenta de Gestor de clusters do serviço está em execução (aparece na Bandeja do sistema), faça duplo clique nele e clique em **iniciar o Cluster Local**.

![Figura 1 - iniciar o cluster local](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Está agora pronto para criar aplicações do Service Fabric Mesh!

## <a name="next-steps"></a>Passos Seguintes

Leia o tutorial [Criar uma aplicação do Azure Service Fabric](service-fabric-mesh-tutorial-create-dotnetcore.md).

Encontre respostas a [perguntas comuns](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/