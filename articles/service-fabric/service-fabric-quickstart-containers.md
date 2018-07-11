---
title: Criar uma aplicação de contentor do Windows no Service Fabric no Azure | Microsoft Docs
description: Neste guia de introdução, crie a sua primeira aplicação de contentor do Windows no Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/30/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 95877f8b81641dd70434fc167003cfcce07d9e84
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110815"
---
# <a name="quickstart-deploy-windows-containers-to-service-fabric"></a>Início rápido: Implementar contentores do Windows no Service Fabric

O Azure Service Fabric é uma plataforma de sistemas distribuídos par implementar e gerir microsserviços e contentores dimensionáveis e fiáveis.

Para executar uma aplicação existente num contentor do Windows num cluster do Service Fabric, não precisa de fazer quaisquer alterações à sua aplicação. Este guia de início rápido mostra como implementar uma imagem de contentor do Docker pré-concebida numa aplicação do Service Fabric. Quando tiver terminado, terá em funcionamento um contentor do Windows Server 2016 Nano Server e do IIS. Este guia de início rápido descreve a implementação de um contentor do Windows. Leia [este início rápido](service-fabric-quickstart-containers-linux.md) para implementar um contentor do Linux.

![Página Web do IIS predefinida][iis-default]

Neste início rápido, vai aprender a:

* Empacotar um contentor de imagens do Docker
* Configurar a comunicação
* Criar e empacotar a aplicação do Service Fabric
* Implementar a aplicação de contentor no Azure

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure (pode criar uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)).
* Um computador de programação com:
  * Visual Studio 2015 ou Visual Studio 2017.
  * [SDK e ferramentas do Service Fabric](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Empacotar um contentor de imagens do Docker com o Visual Studio

O SDK e as ferramentas do Service Fabric fornecem um modelo de serviço para o ajudar a implementar um contentor num cluster do Service Fabric.

Inicie o Visual Studio como “Administrador”.  Selecione **Ficheiro** > **Novo** > **Projeto**.

Selecione **Aplicação do Service Fabric**, dê-lhe o nome "MyFirstContainer" e clique em **OK**.

Selecione **Contentor** nos modelos de **Contentores e Aplicações Alojados**.

Em **Nome da Imagem**, introduza "microsoft/iis:nanoserver", a [imagem de base do Windows Server Nano Server e do IIS](https://hub.docker.com/r/microsoft/iis/).

Configure o mapeamento de portas, da porta para o anfitrião do contentor, de modo que os pedidos recebidos para o serviço na porta 80 sejam mapeados para a porta 80 no contentor.  Defina a **Porta de Contentor** para "80" e defina a **Porta do Anfitrião** para "80".  

Dê ao serviço o nome "MyContainerService" e clique em **OK**.

<<<<<<< Origem atualizada ![Nova caixa de diálogo de serviço][new-service]
=======
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Configurar a comunicação e o mapeamento de portas de porta para anfitrião do contentor

O serviço de precisa de um ponto final para comunicação.  Neste início rápido, o serviço em contentores escuta na porta 80.  No Explorador de Soluções, abra *MyFirstContainer/ApplicationPackageRoot/MyContainerServicePkg/ServiceManifest.xml*.  Atualize o `Endpoint` existente no ficheiro ServiceManifest.xml e adicione o protocolo, a porta e o esquema uri:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
   </Endpoints>
</Resources>
```

Fornecer o `UriScheme` regista automaticamente o ponto final do contentor no serviço de Nomes do Service Fabric para deteção. Um ficheiro de exemplo completo do ServiceManifest.xml está disponível no final deste artigo.

Configure o mapeamento de portas, da porta para o anfitrião do contentor, de modo que os pedidos recebidos para o serviço na porta 80 sejam mapeados para a porta 80 no contentor.  No Explorador de Soluções, abra *MyFirstContainer/ApplicationPackageRoot/ApplicationManifest.xml* e especifique uma política `PortBinding` em `ContainerHostPolicies`.  Neste início rápido, `ContainerPort` é 80 e `EndpointRef` é "MyContainerServiceTypeEndpoint" (o ponto final definido no manifesto do serviço).

```xml
<ServiceManifestImport>
...
  <ConfigOverrides />
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Um ficheiro de exemplo completo do ApplicationManifest.xml está disponível no final deste artigo.
>>>>>>> Alterações armazenadas

## <a name="specify-the-os-build-for-your-container-image"></a>Especificar a compilação do SO para a imagem do contentor
Os contentores criados com uma versão específica do Windows Server poderão não funcionar num anfitrião com uma versão diferente do Windows Server. Por exemplo, os contentores criados com o Windows Server versão 1709 não funcionam nos sistemas anfitriões que executam a versão 2016 do Windows Server. Para obter mais informações, veja [Compatibilidade do sistema operativo do contentor do Windows Server e do sistema operativo do sistema anfitrião ](service-fabric-get-started-containers.md#windows-server-container-os-and-host-os-compatibility). 

Com a versão 6.1 do runtime do Service Fabric e com versões mais recentes, pode especificar várias imagens de sistema operativo por contentor e etiquetar cada uma com a versão do sistema operativo para a qual deve ser implementada. Este procedimento ajuda-o a verificar se a aplicação funcionará em sistemas anfitriões com versões diferentes do sistema operativo Windows. Para saber mais, veja [Indicar imagens de contentor específicas da compilação de SO](service-fabric-get-started-containers.md#specify-os-build-specific-container-images). 

A Microsoft publica imagens diferentes para as versões do IIS criadas em diferentes versões do Windows Server. Para verificar se o Service Fabric implementa um contentor compatível com a versão do Windows Server que em execução nos nós do cluster onde implementa a aplicação, adicione as seguintes linhas ao ficheiro *ApplicationManifest.xml*. A versão de compilação do Windows Server 2016 é 14393 e a da versão 1709 do Windows Server é 16299. 

```xml
    <ContainerHostPolicies CodePackageRef="Code"> 
      <ImageOverrides> 
        ...
          <Image Name="microsoft/iis:nanoserverDefault" /> 
          <Image Name= "microsoft/iis:nanoserver" Os="14393" /> 
          <Image Name="microsoft/iis:windowsservercore-1709" Os="16299" /> 
      </ImageOverrides> 
    </ContainerHostPolicies> 
```

O manifesto de serviço continua a especificar apenas uma imagem para o Nano Server, `microsoft/iis:nanoserver`. 

## <a name="create-a-cluster"></a>Criar um cluster

Para implementar a aplicação num cluster no Azure, pode aderir a um cluster de terceiros. Os party clusters são clusters do Service Fabric gratuitos, limitados temporalmente, alojados no Azure e executados pela equipa do Service Fabric, nos quais qualquer pessoa pode implementar aplicações e saber mais sobre a plataforma.  O cluster utiliza um certificado autoassinado para o nó "nó para nó", bem como a segurança de "cliente para nó". Os party clusters suportam contentores. Se optar por configurar e utilizar o seu próprio cluster, o cluster tem de estar em execução numa SKU que suporte contentores (por exemplo, o Windows Server 2016 Datacenter com Contentores).

Inicie sessão e [adira a um cluster do Windows](http://aka.ms/tryservicefabric). Transfira o certificado PFX para o seu computador ao clicar na ligação **PFX**. Clique na ligação **Como ligar a um cluster de terceiro seguro?** e copie a palavra-passe do certificado. O certificado, a palavra-passe do certificado e o valor **Ponto final da ligação** são utilizados nos passos seguintes.

![PFX e ponto final de ligação](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

> [!Note]
> Há um número limitado de clusters de terceiros por hora. Se aparecer um erro ao tentar inscrever-se num cluster de terceiros, pode ter aguardar algum tempo e tentar novamente ou pode seguir estes passos no tutorial [Implementar uma aplicação .NET](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) para criar um cluster Service Fabric na subscrição do Azure e implementar a aplicação ao mesmo. O cluster criado através do Visual Studio suporta contentores. Depois de ter implementado e verificado a aplicação no seu cluster, pode avançar diretamente para [Concluir a aplicação do Service Fabric de exemplo e manifestos do serviço](#complete-example-service-fabric-application-and-service-manifests) neste início rápido.
>

Num computador Windows, instale o PFX no arquivo de certificados *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
<<<<<<< Updated upstream
``` 
=======
```

Remember the thumbprint for the following step.
>>>>>>> Stashed changes

## Deploy the application to Azure using Visual Studio

Now that the application is ready, you can deploy it to a cluster directly from Visual Studio.

Right-click **MyFirstContainer** in the Solution Explorer and choose **Publish**. The Publish dialog appears.

<<<<<<< Updated upstream
Copy the **Connection Endpoint** from the Party cluster page into the **Connection Endpoint** field. For example, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. 
=======
Copy the **Connection Endpoint** from the Party cluster page into the **Connection Endpoint** field. For example, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Click **Advanced Connection Parameters** and verify the connection parameter information.  *FindValue* and *ServerCertThumbprint* values must match the thumbprint of the certificate installed in the previous step.

![Publish Dialog](./media/service-fabric-quickstart-containers/publish-app.png)
>>>>>>> Stashed changes

Click **Publish**.

Each application in the cluster must have a unique name.  Party clusters are a public, shared environment however and there may be a conflict with an existing application.  If there is a name conflict, rename the Visual Studio project and deploy again.

Open a browser and navigate to the **Connection endpoint** specified in the Party cluster page. You can optionally prepend the scheme identifier, `http://`, and append the port, `:80`, to the URL. For example, http://zwin7fh14scd.westus.cloudapp.azure.com:80. You should see the IIS default web page:
![IIS default web page][iis-default]

<<<<<<< Updated upstream
=======
## Complete example Service Fabric application and service manifests

Here are the complete service and application manifests used in this quickstart.

### ServiceManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>microsoft/iis:nanoserver</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

>>>>>>> Alterações armazenadas
## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a:

* Empacotar um contentor de imagens do Docker
* Configurar a comunicação
* Criar e empacotar a aplicação do Service Fabric
* Implementar a aplicação de contentor no Azure

Para saber mais sobre como trabalhar com contentores do Windows no Service Fabric, avance para o tutorial para aplicações de contentor do Windows.

> [!div class="nextstepaction"]
> [Criar uma aplicação contentora do Windows](./service-fabric-host-app-in-a-container.md)

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
[new-service]: ./media/service-fabric-quickstart-containers/NewService.png
