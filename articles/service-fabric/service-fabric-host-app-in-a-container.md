---
title: Implementar uma aplicação .NET num contentor para o Azure Service Fabric | Microsoft Docs
description: Ensina como empacotar uma aplicação .NET no Visual Studio num Contentor do Docker. Esta nova aplicação "contentor" é então implementada num cluster do Service Fabric.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 11bb34939e5fa3699973051664e85f9ef2147ff7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Implementar uma aplicação .NET num contentor do Windows para o Azure Service Fabric

Este tutorial mostra como implementar uma aplicação ASP.NET existente num contentor do Windows no Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um projeto do Docker no Visual Studio
> * Colocar uma aplicação existente num contentor
> * Configurar integração contínua com o Visual Studio e VSTS

## <a name="prerequisites"></a>Pré-requisitos

1. Instale o [Docker CE para Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description), para que possam executar contentores no Windows 10.
2. Familiarize-se com o [Início rápido dos Contentores do Windows 10][link-container-quickstart].
3. Transfira a aplicação de exemplo [Fabrikam Fiber CallCenter][link-fabrikam-github].
4. Instale o [Azure PowerShell][link-azure-powershell-install]
5. Instale a [Extensão de Ferramentas de Entrega Contínua para o Visual Studio 2017][link-visualstudio-cd-extension]
6. Crie uma [subscrição do Azure][link-azure-subscription] e uma [conta do Visual Studio Team Services][link-vsts-account]. 
7. [Criar um cluster no Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md)

## <a name="create-a-cluster-on-azure"></a>Criar um cluster no Azure
As aplicações do Service Fabric são executadas num cluster, um conjunto de máquinas virtuais ou físicas ligadas à rede. [Configure um cluster do Service Fabric em execução no Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md) antes de criar e implementar a sua aplicação. Ao criar o cluster, escolha uma SKU que suporte contentores em execução (como o Windows Server 2016 Datacenter com Contentores).

## <a name="containerize-the-application"></a>Colocar a aplicação num contentor

Agora que tem um cluster do Service Fabric em execução no Azure, está pronto para criar e implementar uma aplicação que se encontre num contentor. Para começar a executar a nossa aplicação num contentor, precisamos de adicionar **Suporte do Docker** ao projeto no Visual Studio. Quando adiciona **suporte Docker** à aplicação, acontecem duas coisas. Primeiro, é adicionado _Dockerfile_ ao projeto. Este novo ficheiro descreve como a imagem de contentor tem de ser criada. Segundo, é adicionado um novo projeto _docker-compose_ à solução. O novo projeto contém alguns ficheiros docker-compose. Os ficheiros docker-compose podem servir para descrever como o contentor é executado.

Obtenha mais informações sobre como trabalhar com [Ferramentas de Contentor do Visual Studio][link-visualstudio-container-tools].

### <a name="add-docker-support"></a>Adicionar suporte do Docker

Abra o ficheiro [FabrikamFiber.CallCenter.sln][link-fabrikam-github] no Visual Studio.

Clique com o botão direito do rato em **FabrikamFiber.Web** projeto > **Adicionar** > **Suporte do Docker**.

### <a name="add-support-for-sql"></a>Adicionar suporte para SQL

Esta aplicação utiliza o SQL como o fornecedor de dados, para que seja preciso um servidor SQL para executar a aplicação. Referencie a uma imagem de contentor do SQL Server no nosso ficheiro docker-compose.override.yml.

No Visual Studio, abra **Explorador de Soluções**, encontre **docker-compose** e abra o ficheiro **docker-compose.override.yml**.

Navegue para o nó `services:`, adicione um nó denominado `db:`, que define a entrada do SQL Server para o contentor.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Pode utilizar qualquer SQL Server que preferir para depuração local, desde que seja acessível a partir do seu anfitrião. No entanto, o **localdb** não suporta comunicação `container -> host`.

>[!WARNING]
>Executar o SQL Server num contentor não suporta dados persistentes. Quando o contentor para, os dados são apagados. Não utilize esta configuração para produção.

Navegue para o nó `fabrikamfiber.web:` e adicione um nó subordinado denominado `depends_on:`. Isto garante que o serviço `db` (o contentor do SQL Server) é iniciado antes da nossa aplicação Web (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Atualizar a configuração da Web

Volte para o projeto **FabrikamFiber.Web**, atualize a cadeia de ligação no ficheiro **web.config**, para que encaminhe para o SQL Server no contentor.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Se pretender utilizar um SQL Server diferente ao criar uma versão de compilação da aplicação Web, adicione outra cadeia de ligação ao seu ficheiro web.release.config.

### <a name="test-your-container"></a>Testar o seu contentor

Prima **F5** para executar e depurar a aplicação no seu contentor.

O Edge abre a página definida de início da sua aplicação com o endereço IP do contentor na rede de NAT interna (normalmente 172.x.x.x). Para saber mais sobre a depuração de aplicações nos contentores com o Visual Studio 2017, veja [este artigo][link-debug-container].

![exemplo do fabrikam num contentor][image-web-preview]

O contentor está agora pronto para ser criado e empacotado numa aplicação do Service Fabric. Assim que tiver a imagem de contentor incorporada no seu computador, pode enviá-la para qualquer registo de contentor e pedir a qualquer anfitrião para executar.

## <a name="get-the-application-ready-for-the-cloud"></a>Preparar a aplicação para a cloud

Para preparar a aplicação para ser executada no Service Fabric no Azure, temos de concluir dois passos:

1. Expor a porta onde queremos alcançar a nossa aplicação Web no cluster do Service Fabric.
2. Providencie um base de dados SQL pronta para produção para a nossa aplicação.

### <a name="expose-the-port-for-the-app"></a>Expor a porta para a aplicação
O cluster do Service Fabric configurado, tem a porta *80* aberta por predefinição no Balanceador de Carga do Azure, que equilibra o tráfego de entrada para o cluster. Podemos expor o nosso contentor nesta porta através do nosso ficheiro docker-compose.yml.

No Visual Studio, abra **Explorador de Soluções**, encontre **docker-compose** e abra o ficheiro **docker-compose.yml**.

Modifique o nó `fabrikamfiber.web:`, adicione um nó subordinado denominado `ports:`.

Adicione uma entrada de cadeia `- "80:80"`. O ficheiro docker-compose.yml deve ter o seguinte aspeto:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Utilizar uma base de dados SQL de produção
Ao executar na produção, precisamos que os nossos dados fiquem na nossa base de dados. Não existe atualmente nenhuma forma de garantir que os dados fiquem num contentor, por isso não pode armazenar dados de produção no SQL Server num contentor.

Recomendamos que utilize uma Base de Dados SQL do Azure. Para configurar e executar um SQL Server gerido no Azure, leia o artigo [Inícios Rápidos de Base de Dados SQL do Azure][link-azure-sql].

>[!NOTE]
>Não se esqueça de alterar as cadeias de ligação no SQL server no ficheiro **web.release.config** no projeto **FabrikamFiber.Web**.
>
>Esta aplicação falha se nenhuma base de dados SQL estiver acessível. Pode optar por prosseguir e implementar a aplicação sem o SQL server.

## <a name="deploy-with-visual-studio-team-services"></a>Implementar com o Visual Studio Team Services

Para configurar a implementação com o Visual Studio Team Services, tem de instalar a [Extensão de Ferramentas de Entrega Contínua para o Visual Studio 2017][link-visualstudio-cd-extension]. Esta extensão facilita a implementação no Azure ao configurar o Visual Studio Team Services e implementar a sua aplicação no cluster do Service Fabric.

Para começar, o código tem de estar alojado no controlo de origem. O resto desta secção assume que o **git** está a ser utilizado.

### <a name="set-up-a-vsts-repo"></a>Configurar um repositório VSTS
No canto inferior direito do Visual Studio, clique em **Adicionar ao Controlo de Origem** > **Git** (ou qualquer opção que preferir).

![prima o botão do controlo de origem][image-source-control]

No painel _Team Explorer_, prima **Publicar Repositório do Git**.

Selecione o nome do repositório VSTS e prima **Repositório**.

![publicar repositório no VSTS][image-publish-repo]

Agora que o seu código está sincronizado com um repositório de origem VSTS, pode configurar a integração e a entrega contínuas.

### <a name="setup-continuous-delivery"></a>Configurar entrega contínua

No _Explorador de Soluções_, clique com o botão direito do rato em **solução** > **Configurar Entrega Contínua**.

Selecione a subscrição do Azure.

Defina o **Tipo de Anfitrião** para **Cluster do Service Fabric**.

Defina o **Anfitrião de Destino** para o cluster dos recursos de infraestrutura que criou na secção anterior.

Escolha um **Registo de Contentor** para publicar o seu contentor.

>[!TIP]
>Utilize o botão **Editar** para criar um registo de contentor.

Prima **OK**.

![configurar integração contínua dos recursos de infraestrutura][image-setup-ci]
   
   Assim que a configuração estiver concluída, o contentor é implementado no Service Fabric. Sempre que emitir atualizações para o repositório, é executada uma nova compilação e versão.
   
   >[!NOTE]
   >Demora aproximadamente 15 minutos a compilar imagens de contentor.
   >A primeira implementação para o cluster do Service Fabric faz com que as imagens de contentor do Windows Server Core base sejam transferidas. A transferência demora mais 5-10 minutos a concluir.

Navegue para a aplicação Fabrikam Call Center com o URL do seu cluster: por exemplo, *http://mycluster.westeurope.cloudapp.azure.com*

Agora que implementou a solução do Fabrikam Call Center e colocou num contentor, pode abrir o [portal do Azure][link-azure-portal] e ver a aplicação em execução no Service Fabric. Para experimentar a aplicação, abra um browser e avance para o URL do seu cluster do Service Fabric.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um projeto do Docker no Visual Studio
> * Colocar uma aplicação existente num contentor
> * Configurar integração contínua com o Visual Studio e VSTS

Na parte seguinte do tutorial, aprenda a configurar a [monitorização para o contentor](service-fabric-tutorial-monitoring-wincontainers.md).

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
