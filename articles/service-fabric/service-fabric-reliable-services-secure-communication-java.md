---
title: Proteger as comunicações de comunicação remota do serviço com o Java no Azure Service Fabric | Microsoft Docs
description: Saiba como proteger a comunicação de sistema de interação remota com base do serviço para os serviços de fiáveis do Java que estão a executar um cluster do Service Fabric do Azure.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1843720b9700e66af8ee84766cf7d63ac62e6283
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749917"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Proteger as comunicações de comunicação remota do serviço no serviço de Java
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java em Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

A segurança é um dos aspetos mais importantes de comunicação. A estrutura da aplicação Reliable Services fornece alguns pilhas de comunicação prebuilt e ferramentas que pode utilizar para melhorar a segurança. Este artigo descreve como melhorar a segurança quando estiver a utilizar a comunicação remota do serviço no serviço de Java. Baseia-se no existente [exemplo](service-fabric-reliable-services-communication-remoting-java.md) que explica como configurar a gestão remota para serviços fiáveis escrita em Java. 

Para ajudar a proteger um serviço quando estiver a utilizar a comunicação remota do serviço com os serviços de Java, siga estes passos:

1. Criar uma interface, `HelloWorldStateless`, que define os métodos que estarão disponíveis para uma chamada de procedimento remoto do seu serviço. O serviço irá utilizar `FabricTransportServiceRemotingListener`, que está declarada no `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` pacote. Este é um `CommunicationListener` implementação que oferece funções de sistema de interação remota.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Adicione as definições de escuta e credenciais de segurança.

    Certifique-se de que o certificado que pretende utilizar para ajudar a proteger a comunicação de serviço é instalado em todos os nós do cluster. Existem duas formas que pode fornecer as definições de escuta e credenciais de segurança:

   1. Fornecê-los utilizando um [o pacote de configuração](service-fabric-application-and-service-manifests.md):

       Adicionar um nomeado `TransportSettings` secção no ficheiro settings.xml.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       Neste caso, o `createServiceInstanceListeners` método terá este aspeto:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Se adicionar um `TransportSettings` secção no ficheiro settings.xml sem um prefixo `FabricTransportListenerSettings` carregará todas as definições desta secção por predefinição.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Neste caso, o `CreateServiceInstanceListeners` método terá este aspeto:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Quando chamar métodos num serviço protegido através da pilha do sistema de interação remota, em vez de utilizar o `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` classe para criar um proxy de serviço, utilize `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Se o código de cliente está a ser executado como parte de um serviço, pode carregar `FabricTransportSettings` do ficheiro settings.xml. Crie uma secção de TransportSettings que é semelhante ao código do serviço, conforme mostrado anteriormente. Efetue as seguintes alterações ao código de cliente:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
