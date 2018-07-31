---
title: Configurar um ambiente de desenvolvimento Windows para aplicações do Service Fabric Mesh | Microsoft Docs
description: Configure o seu ambiente de desenvolvimento Windows para que possa criar uma aplicação do Service Fabric Mesh e implementá-la no Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/20/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 589bef1894a3bee1e6974a0ea2516200fae2891f
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185548"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Configurar o ambiente de desenvolvimento Windows para compilar aplicações do Service Fabric

Para compilar e executar aplicações do Azure Service Fabric no computador de desenvolvimento Windows, tem de instalar o runtime, o SDK e as ferramentas do Service Fabric.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Versões de sistema operativo com suporte

As seguintes versões do sistema operativo são suportadas para desenvolvimento:

* Windows 10 (Enterprise, Professional ou Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

É necessário o Visual Studio 2017 para implementar aplicações do Service Fabric. [Instale a versão 15.6.0][download-visual-studio] ou superior e ative as seguintes cargas de trabalho:

- Desenvolvimento ASP.NET e Web
- Desenvolvimento do Azure
 
## <a name="windows-10---install-docker"></a>Windows 10 - Instalar o Docker

Transfira e instale a versão mais recente do [Docker Community Edition para Windows][download-docker] para suportar as aplicações do Service Fabric em contentores utilizadas pelo Service Fabric Mesh.

Durante a instalação, selecione **Utilizar contentores do Windows em vez de contentores do Linux** quando lhe for pedido. Se o Hyper-V não estiver ativado no seu computador, a instalação do Docker poderá ativá-lo. Clique em **OK** para fazer isso, se lhe for pedido.

## <a name="windows-server-2016---install-hyper-v-and-docker"></a>Windows Server 2016 - Instalar o Hyper-V e o Docker

**Instalar o Hyper-V**

Primeiro, abra o PowerShell como administrador e execute o seguinte comando para instalar o Hyper-V e reiniciar o computador. Para obter mais informações, veja [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

**Instalar o Docker**

Abra o PowerShell como administrador e execute os seguintes comandos para instalar o Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Reinicie o computador.

## <a name="sdk-and-tools"></a>SDK e ferramentas

Instale o runtime, o SDK e as ferramentas do Service Fabric Mesh pela ordem seguinte.

1. Instale o [SDK do Service Fabric Mesh][download-sdkmesh] através do Instalador de Plataforma Web. Também será instalado o SDK e o runtime do Microsoft Azure Service Fabric.
2. Instale a [extensão Ferramentas do Service Fabric para Visual Studio (pré-visualização)][download-tools] a partir do Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Compilar um cluster

Se estiver a utilizar o Visual Studio, pode ignorar esta secção porque o Visual Studio vai criar um cluster local, caso não tenha um.

Para obter o melhor desempenho de depuração ao criar e executar aplicações do Service Fabric, recomendamos que crie um cluster de desenvolvimento de nó único local. Este cluster tem de estar em execução sempre que implementar ou depurar um projeto do Service Fabric Mesh.

O Docker **tem** estar em execução antes de poder compilar um cluster. Teste se o Docker está em execução ao abrir uma janela de terminal e ao executar `docker ps` para ver se ocorre um erro. Se a resposta não indicar um erro, o Docker está a ser executado e está pronto para compilar um cluster.

Depois de instalar o runtime, os SDKs e as ferramentas do Visual Studio, crie um cluster de desenvolvimento.

1. Feche a janela do PowerShell.
2. Abra uma nova janela do PowerShell elevada como administrador. Este passo é necessário para carregar os módulos do Service Fabric instalados recentemente.
3. Execute o seguinte comando do PowerShell para criar um cluster de desenvolvimento:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster -UseMachineName
    ```

4. Para iniciar a ferramenta de gestor de clusters locais, execute o seguinte comando do PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```

Está agora pronto para criar aplicações do Service Fabric Mesh!

## <a name="next-steps"></a>Passos seguintes

Leia o tutorial [Criar uma aplicação do Azure Service Fabric](service-fabric-mesh-tutorial-create-dotnetcore.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/