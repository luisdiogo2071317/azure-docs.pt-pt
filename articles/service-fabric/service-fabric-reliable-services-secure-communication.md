---
title: Proteger as comunicações de comunicação remota do serviço com c# no Azure Service Fabric | Microsoft Docs
description: Saiba como proteger a comunicação do serviço de sistema de interação remota com base em c# fiável serviços que estejam a executar um cluster do Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: d185be26633178d8b3f147453b4c48eb77d7e425
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753528"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>Proteger as comunicações de comunicação remota do serviço no serviço do c#
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java em Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

A segurança é um dos aspetos mais importantes de comunicação. A estrutura da aplicação Reliable Services fornece alguns pilhas de comunicação prebuilt e ferramentas que pode utilizar para melhorar a segurança. Este artigo descreve como melhorar a segurança quando estiver a utilizar a comunicação remota do serviço no serviço do c#. Baseia-se no existente [exemplo](service-fabric-reliable-services-communication-remoting.md) que explica como configurar a gestão remota para serviços fiáveis escrito em c#. 

Para ajudar a proteger um serviço quando estiver a utilizar a comunicação remota do serviço com os serviços de c#, siga estes passos:

1. Criar uma interface, `IHelloWorldStateful`, que define os métodos que estarão disponíveis para uma chamada de procedimento remoto do seu serviço. O serviço irá utilizar `FabricTransportServiceRemotingListener`, que está declarada no `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` espaço de nomes. Este é um `ICommunicationListener` implementação que oferece funções de sistema de interação remota.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. Adicione as definições de escuta e credenciais de segurança.

    Certifique-se de que o certificado que pretende utilizar para ajudar a proteger a comunicação de serviço é instalado em todos os nós do cluster. Existem duas formas que pode fornecer as definições de escuta e credenciais de segurança:

   1. Fornecê-los diretamente com o código do serviço:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. Fornecê-los utilizando um [o pacote de configuração](service-fabric-application-and-service-manifests.md):

       Adicionar um nomeado `TransportSettings` secção no ficheiro settings.xml.

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       Neste caso, o `CreateServiceReplicaListeners` método terá este aspeto:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        Se adicionar um `TransportSettings` secção no ficheiro settings.xml, `FabricTransportRemotingListenerSettings ` carregará todas as definições desta secção por predefinição.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Neste caso, o `CreateServiceReplicaListeners` método terá este aspeto:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. Quando chamar métodos num serviço protegido através da pilha do sistema de interação remota, em vez de utilizar o `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` classe para criar um proxy de serviço, utilize `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Transmita `FabricTransportRemotingSettings`, que contém `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Se o código de cliente está a ser executado como parte de um serviço, pode carregar `FabricTransportRemotingSettings` do ficheiro settings.xml. Crie uma secção de HelloWorldClientTransportSettings que é semelhante ao código do serviço, conforme mostrado anteriormente. Efetue as seguintes alterações ao código de cliente:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Se o cliente não está em execução como parte de um serviço, pode criar um ficheiro de client_name.settings.xml na mesma localização onde está o client_name.exe. Em seguida, crie uma secção TransportSettings nesse ficheiro.

    Semelhante ao serviço, se adicionar um `TransportSettings` secção cliente settings.xml/client_name.settings.xml, `FabricTransportRemotingSettings` carrega todas as definições desta secção por predefinição.

    Nesse caso, o código anterior ainda mais é simplificado:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

Como passo seguinte, leia [Web API com OWIN no Reliable Services](service-fabric-reliable-services-communication-webapi.md).
