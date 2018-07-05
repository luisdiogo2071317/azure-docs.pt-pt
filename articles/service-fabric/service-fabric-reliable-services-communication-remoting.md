---
title: Comunicação remota do serviço usando c# no Service Fabric | Documentos da Microsoft
description: Comunicação remota do Service Fabric permite que clientes e serviços comunicar com serviços de c#, usando uma chamada de procedimento remoto.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: 7afa50484c3ebf258bbdd2b7f16c9cd051710d28
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437897"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>Comunicação remota do serviço em c# com o Reliable Services
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java em Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Para os serviços que não ficam amarrados a um protocolo de comunicação específico ou a pilha, como WebAPI, Windows Communication Foundation (WCF) ou outras pessoas, a estrutura de Reliable Services fornece um mecanismo de comunicação remota de forma rápida e fácil configurar chamadas de procedimento remoto para serviços. Este artigo aborda como configurar a chamadas de procedimento remoto para os serviços escritos com o c#.

## <a name="set-up-remoting-on-a-service"></a>Configurar remotamente num serviço
Configurar a comunicação remota para um serviço é feita em duas etapas simples:

1. Crie uma interface para o seu serviço implementar. Esta interface define os métodos disponíveis para uma chamada de procedimento remoto no seu serviço. Tem de ser os métodos que retornam task métodos assíncronos. Tem de implementar a interface `Microsoft.ServiceFabric.Services.Remoting.IService` para sinalizar que o serviço tem uma interface de comunicação remota.
2. Utilize um serviço de escuta de comunicação remota no seu serviço. RemotingListener é um `ICommunicationListener` implementação que fornece capacidades de comunicação remota. O `Microsoft.ServiceFabric.Services.Remoting.Runtime` espaço de nomes contém um método de extensão,`CreateServiceRemotingListener` para serviços com e sem monitoração de estado que podem ser utilizados para criar um serviço de escuta de comunicação remota utilizando o protocolo de transporte de comunicação remota do padrão.

>[!NOTE]
>O `Remoting` espaço de nomes está disponível como um pacote de NuGet separado chamado `Microsoft.ServiceFabric.Services.Remoting`

Por exemplo, o serviço sem estado seguinte expõe um único método para obter "Hello World" através de uma chamada de procedimento remoto.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task<string> HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context => this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> Os argumentos e os tipos de retorno na interface de serviço podem ser qualquer tipo personalizado, simple ou complexo, mas têm de ser serializáveis, o .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Chamar os métodos de serviço remoto
Chamar métodos num serviço utilizando a pilha de comunicação remota é feito usando um proxy local para o serviço através do `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` classe. O `ServiceProxy` método cria um proxy local com a mesma interface, que implementa o serviço. Com que o proxy, pode chamar métodos na interface remotamente.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

A estrutura de comunicação remota propaga exceções geradas pelo serviço ao cliente. Como resultado, quando utilizar `ServiceProxy`, o cliente é responsável por processar as exceções geradas pelo serviço.

## <a name="service-proxy-lifetime"></a>O tempo de vida do serviço Proxy
Criação de ServiceProxy é uma operação simples, para que possa criar tantos conforme necessário. Instâncias de Proxy de serviço podem ser reutilizadas, desde que eles são necessários. Se uma chamada de procedimento remoto lança uma exceção, ainda pode reutilizar a mesma instância de proxy. Cada ServiceProxy contenha um cliente de comunicação utilizado para enviar mensagens durante a transmissão. Ao invocar chamadas remotas, são executadas verificações de internas para determinar se o cliente de comunicação é válido. Com base nos resultados dessas verificações, o cliente de comunicação é recriado se for necessário. Por conseguinte, se ocorrer uma exceção, não precisa de recriar `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>Tempo de vida de ServiceProxyFactory
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) é uma fábrica que cria instâncias de proxy para interfaces de comunicação remota diferente. Se usar a api `ServiceProxy.Create` para a criação de proxy, em seguida, o framework cria um singleton ServiceProxy.
É útil criar um manualmente quando precisa substituir [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) propriedades.
A criação do Factory é uma operação dispendiosa. ServiceProxyFactory mantém um cache interna do cliente de comunicação.
Prática recomendada é colocar em cache ServiceProxyFactory durante mais tempo.

## <a name="remoting-exception-handling"></a>Manipulação de exceção de comunicação remota
Todas as exceções remotas geradas pela API do serviço são enviadas ao cliente como AggregateException. RemoteExceptions deve ser DataContract serializável; Se não forem, o proxy de API emite [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) com o erro de serialização no mesmo.

ServiceProxy processa todas as exceções de ativação pós-falha para a partição de serviço que é criado para. Ele resolve novamente os pontos de extremidade, se existirem exceções de ativação pós-falha (não transitórias exceções) e repete a chamada com o ponto final correto. O número de tentativas de exceções de ativação pós-falha é indefinido.
Se ocorrerem exceções transitórias, o proxy repete a chamada.

Padrão repetição parâmetros são fornecem pela [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

Pode configurar estes valores, passando o objeto OperationRetrySettings para ServiceProxyFactory construtor.

## <a name="how-to-use-the-remoting-v2-stack"></a>Como utilizar a pilha de gestão remota V2

A partir da versão de pacote NuGet Remoting 2.8, tem a opção para utilizar a pilha de gestão remota V2. A pilha de gestão remota V2 apresenta um desempenho melhor e fornece recursos como a serialização personalizada e da API mais conectável.
Código de modelo continua a utilizar a pilha de V1 de comunicação remota.
Gestão remota V2 não é compatível com V1 (anterior comunicação remota pilha), por isso, siga as instruções abaixo no [como atualizar do V1 para V2](#how-to-upgrade-from-remoting-v1-to-remoting-v2) sem afetar a disponibilidade do serviço.

As seguintes abordagens estão disponíveis para permitir a pilha de V2.

### <a name="using-an-assembly-attribute-to-use-the-v2-stack"></a>Usando um atributo de assembly para usar a pilha de V2

Estes passos alterar código de modelo para usar a pilha de V2 usando um atributo de assembly.

1. Alterar o recurso de ponto final da `"ServiceEndpoint"` para `"ServiceEndpointV2"` no manifesto do serviço.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Utilize o `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners` método de extensão para criar serviços de escuta de comunicação remota (igual para a V1 e V2).

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3. O Assembly que contém as interfaces de comunicação remota com um `FabricTransportServiceRemotingProvider` atributo.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```

Sem alterações de código são necessárias no projeto cliente.
Crie o assembly de cliente com o assembly de interface para se certificar de que o atributo de assembly mostrado acima é usado.

### <a name="using-explicit-v2-classes-to-use-the-v2-stack"></a>Usando classes de V2 explícitas para usar a pilha de V2

Como alternativa à utilização de um atributo de assembly, a pilha de V2 também pode ser ativada através da utilização de classes de V2 explícitas.

Estes passos alterar código de modelo para usar a pilha de V2 usando classes de V2 explícitas.

1. Alterar o recurso de ponto final da `"ServiceEndpoint"` para `"ServiceEndpointV2"` no manifesto do serviço.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Utilize o [FabricTransportServiceRemotingListener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) partir o `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime` espaço de nomes.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                return new FabricTransportServiceRemotingListener(c, this);

            })
        };
    }
  ```

3. Utilize o [FabricTransportServiceRemotingClientFactory ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) partir o `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` espaço de nomes para a criação de clientes.

  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>Como atualizar do V1 de comunicação remota para gestão remota V2.
Para atualizar a partir do V1 para V2, as atualizações de 2 passos são necessárias. Passos seguintes para ser executado na sequência listada.

1. Atualize o serviço de V1 para V2 do serviço, utilizando o atributo de CompactListener.
Esta alteração torna-se de que o serviço está a escutar no V1 e V2 serviço de escuta.

    a) adicione um recurso de ponto final com o nome como "ServiceEndpointV2" no manifesto do serviço.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b) utilize o método de extensão para criar o serviço de escuta de comunicação remota seguinte.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) adicione o atributo de Assembly em Interfaces de comunicação remota para utilizar CompatListener e cliente V2.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.CompatListener, RemotingClient = RemotingClient.V2Client)]

      ```
2. Atualize o cliente de V1 para V2 cliente utilizando o atributo de cliente da V2.
Este passo torna-se de que o cliente está a utilizar a pilha de V2.
Nenhuma alteração no projeto/serviço de cliente é necessária. A criação de projetos de cliente com o assembly da interface atualizada é suficiente.

3. Este passo é opcional. Utilize o atributo V2Listener e, em seguida, atualize o serviço de V2.
Este passo certifica-se de que o serviço está escutando apenas no serviço de escuta do V2.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
```

## <a name="how-to-use-custom-serialization-with-remoting-v2"></a>Como utilizar a serialização personalizada com a gestão remota V2.
Exemplo seguinte utiliza a serialização Json com a gestão remota V2.
1. Implementar a interface de IServiceRemotingMessageSerializationProvider para fornecer implementação para a serialização personalizada.
    Segue-se em como a implementação é parecida com o trecho de código.

 ```csharp
    public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
    {
        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> requestBodyTypes)
        {
            return new ServiceRemotingRequestJsonMessageBodySerializer(serviceInterfaceType, requestBodyTypes);
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> responseBodyTypes)
        {
            return new ServiceRemotingResponseJsonMessageBodySerializer(serviceInterfaceType, responseBodyTypes);
        }

        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
            return new JsonMessageFactory();
        }
    }

    class JsonMessageFactory : IServiceRemotingMessageBodyFactory
    {
        public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName,
            int numberOfParameters)
        {
            return new JsonRemotingRequestBody();
        }

        public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName)
        {
            return new JsonRemotingResponseBody();
        }
    }

    class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
    {
        public ServiceRemotingRequestJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
        {
            if (serviceRemotingRequestMessageBody == null)
            {
                return null;
            }

            var writeStream = new MemoryStream();
            var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream));

            var serializer = JsonSerializer.Create(new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);

            jsonWriter.Flush();
            var segment = new ArraySegment<byte>(writeStream.ToArray());
            var segments = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(segments);
        }

        public IServiceRemotingRequestMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (JsonReader reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingRequestBody>(reader);
            }
        }
    }

    class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
    {
        public ServiceRemotingResponseJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
        {
            var json = JsonConvert.SerializeObject(responseMessageBody, new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            var bytes = Encoding.UTF8.GetBytes(json);
            var segment = new ArraySegment<byte>(bytes);
            var list = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(list);
        }

        public IServiceRemotingResponseMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (var reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingResponseBody>(reader);
            }
        }
    }

    internal class JsonRemotingResponseBody : IServiceRemotingResponseMessageBody
    {
        public object Value;

        public void Set(object response)
        {
            this.Value = response;
        }

        public object Get(Type paramType)
        {
            return this.Value;
        }
    }

    class JsonRemotingRequestBody : IServiceRemotingRequestMessageBody
    {
        public readonly Dictionary<string, object> parameters = new Dictionary<string, object>();        

        public void SetParameter(int position, string parameName, object parameter)
        {
            this.parameters[parameName] = parameter;
        }

        public object GetParameter(int position, string parameName, Type paramType)
        {
            return this.parameters[parameName];
        }
    }
 ```

2.    Substitua o provedor de serialização padrão com JsonSerializationProvider de serviço de escuta de comunicação remota.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
   {
       return new[]
       {
           new ServiceInstanceListener((c) =>
           {
               return new FabricTransportServiceRemotingListener(c, this,
                   new ServiceRemotingJsonSerializationProvider());
           })
       };
   }
  ```

3.    Substitua o provedor de serialização padrão com JsonSerializationProvider de fábrica do cliente de comunicação remota.

```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
            {
                return new FabricTransportServiceRemotingClientFactory(
                    serializationProvider: new ServiceRemotingJsonSerializationProvider());
            });
  ```

## <a name="next-steps"></a>Passos Seguintes
* [Web API com o OWIN no Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Comunicação do WCF com o Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Proteger a comunicação de Reliable Services](service-fabric-reliable-services-secure-communication.md)

