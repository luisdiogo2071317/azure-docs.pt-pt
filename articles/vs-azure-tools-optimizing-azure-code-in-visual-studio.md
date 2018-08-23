---
title: Otimizar o código do Azure no Visual Studio | Documentos da Microsoft
description: Saiba mais sobre o código como o Azure otimização ferramentas no Visual Studio ajudam a tornar seu código mais robusto e com melhor desempenho.
services: visual-studio-online
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/11/2016
ms.author: cawa
ms.openlocfilehash: 0497ac628d7882a0b722796493c10c0d8b04e759
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42443673"
---
# <a name="optimizing-your-azure-code"></a>Otimizar o código do Azure
Quando estiver a programar aplicações que utilizam o Microsoft Azure, existem algumas práticas de codificação, que deve seguir para ajudar a evitar problemas de escalabilidade da aplicação, o comportamento e desempenho num ambiente de cloud. A Microsoft fornece uma ferramenta de análise de código do Azure que reconhece e identifica vários desses problemas comumente encontrados e ajuda-o a resolvê-los. Pode baixar a ferramenta no Visual Studio por meio do NuGet.

## <a name="azure-code-analysis-rules"></a>Regras de análise de código do Azure
A ferramenta de análise de código do Azure utiliza as seguintes regras para sinalizar automaticamente seu código do Azure quando encontra problemas conhecidos que afetam o desempenho. Detetou problemas aparecem como um avisos ou erros de compilador. Correções de código ou sugestões para resolver o aviso ou erro, muitas vezes, são fornecidos por meio de um ícone de lâmpada.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Evite utilizar o modo de estado de sessão (em processo) predefinido
### <a name="id"></a>ID
AP0000

### <a name="description"></a>Descrição
Se utilizar o modo de estado de sessão (em processo) predefinido para aplicações na cloud, pode perder o estado da sessão.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Por predefinição, o modo de estado de sessão especificado no ficheiro Web. config está em processo. Além disso, se nenhuma entrada especificada no ficheiro de configuração, o modo de estado da sessão é predefinida como em processo. O modo de dentro do processo armazena o estado da sessão na memória no servidor web. Quando uma instância é reiniciada ou uma nova instância é utilizada para balanceamento de carga ou suporte de ativação pós-falha, não é guardado o estado da sessão armazenado na memória no servidor web. Esta situação impede a aplicação de ser escalável na cloud.

Estado de sessão do ASP.NET oferece suporte a várias opções de armazenamento diferente para os dados de estado de sessão: InProc, StateServer, SQLServer, personalizado e desativado. É recomendado que utilize modo personalizado para alojar dados num arquivo de estado de sessão externo, como [fornecedor de estado de sessão do Azure para Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Solução
É uma solução recomendada armazenar o estado da sessão num serviço de cache gerida. Aprenda a usar [fornecedor de estado de sessão do Azure para Redis](http://go.microsoft.com/fwlink/?LinkId=401521) para armazenar seu estado de sessão. Também pode armazenar o estado da sessão em outros lugares para garantir que seu aplicativo é escalável na cloud. Para saber mais sobre as soluções alternativas, leia [modos de estado de sessão](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Método de execução não deve ser async
### <a name="id"></a>ID
AP1000

### <a name="description"></a>Descrição
Criar métodos assíncronos (como [await](https://msdn.microsoft.com/library/hh156528.aspx)) fora do [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método e, em seguida, chamar os métodos de async partir [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Declarar o [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método como async faz com que a função de trabalho introduzir um ciclo de reinício.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Chamar os métodos async dentro de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método faz com que o tempo de execução do serviço cloud reciclar a função de trabalho. Quando uma função de trabalho é iniciado, todas as execução do programa se ocorre dentro do [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método. Sair da [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método faz com que a função de trabalho reiniciar. Quando o tempo de execução de função de trabalho atinge o método assíncrono, ele expede todas as operações após o método assíncrono e, em seguida, retorna. Isso faz com que a função de trabalho de saída a [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método e reinício. Na próxima iteração de execução, a função de trabalho atinge o método async novamente e é reiniciado, fazendo com que a função de trabalho reciclar novamente também.

### <a name="solution"></a>Solução
Colocar todas as operações de async fora dos [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método. Em seguida, chamar o método de async refatorizá-los de dentro do [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) método, por exemplo, RunAsync () .wait. A ferramenta de análise de código do Azure pode ajudá-lo a corrigir este problema.

O fragmento de código seguinte demonstra a correção de código para este problema:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Utilizar a autenticação da assinatura de acesso partilhado do Service Bus
### <a name="id"></a>ID
AP2000

### <a name="description"></a>Descrição
Utilize a assinatura de acesso partilhado (SAS) para autenticação. Serviço de controlo de acesso (ACS) está a ser preterido para a autenticação de barramento de serviço.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Para maior segurança, Azure Active Directory está a substituir a autenticação ACS com a autenticação de SAS. Ver [do Azure Active Directory é o futuro do ACS](https://cloudblogs.microsoft.com/enterprisemobility/2013/06/22/azure-active-directory-is-the-future-of-acs/) para obter informações sobre o plano de transição.

### <a name="solution"></a>Solução
Utilize a autenticação de SAS nas suas aplicações. O exemplo seguinte mostra como utilizar um token de SAS para aceder a um espaço de nomes do service bus ou uma entidade.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Consulte os seguintes tópicos para obter mais informações.

* Para uma descrição geral, consulte [autenticação de assinatura de acesso partilhado com o Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)
* [Como utilizar a autenticação da assinatura de acesso partilhado com o Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)
* Para um projeto de exemplo, consulte [autenticação a utilizar acesso assinatura partilhado (SAS) com subscrições do Service Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Considere utilizar o método OnMessage para evitar "receber loop"
### <a name="id"></a>ID
AP2002

### <a name="description"></a>Descrição
Para evitar um "loop, receber" chamar o **OnMessage** método é uma solução melhor para o recebimento de mensagens que chamar os **Receive** método. No entanto, se tiver de utilizar o **Receive** método e especificar um tempo de espera do servidor não predefinido, certifique-se do tempo de espera do servidor é mais do que um minuto.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Ao chamar **OnMessage**, o cliente inicia uma bomba de mensagens internas constantemente consulta a fila ou subscrição. Essa bomba de mensagens contém um loop infinito que emite uma chamada para receber mensagens. Se a chamada exceder o tempo limite, ele emite uma chamada de novo. O intervalo de tempo limite é determinado pelo valor do [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) propriedade da [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)que está a ser utilizado.

A vantagem de usar **OnMessage** em comparação com **Receive** é que os utilizadores não têm manualmente consultar de mensagens, lidar com exceções, processar várias mensagens em paralelo e concluir as mensagens.

Se chamar **Receive** sem utilizar o valor predefinido, certifique-se a *ServerWaitTime* valor é mais do que um minuto. A definição *ServerWaitTime* para mais de um minuto impede que o servidor exceder o tempo limite antes da mensagem é recebida totalmente.

### <a name="solution"></a>Solução
Consulte os seguintes exemplos de código para utilizações recomendadas. Para obter mais detalhes, consulte [QueueClient.OnMessage método (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)e [QueueClient.Receive método (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Para melhorar o desempenho da infraestrutura de mensagens do Azure, veja o padrão de design [manual básico de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).

Segue-se um exemplo de uso **OnMessage** para receber mensagens.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Segue-se um exemplo de uso **Receive** com o servidor predefinido de tempo de espera.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Segue-se um exemplo de uso **Receive** com um servidor de não-padrão de tempo de espera.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Considere a utilização de métodos assíncronos do Service Bus
### <a name="id"></a>ID
AP2003

### <a name="description"></a>Descrição
Utilize os métodos assíncronos do Service Bus para melhorar o desempenho com as mensagens mediadas.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Usar métodos assíncronos possibilita a simultaneidade de programa do aplicativo porque cada chamada em execução não bloqueia o thread principal. Quando utilizar o Service Bus métodos de mensagens, executar uma operação (enviar, receber, eliminar, etc.) demora algum tempo. Desta vez inclui o processamento da operação pelo serviço do Service Bus, além da latência do pedido e resposta. Para aumentar o número de operações por hora, operações devem executar em simultâneo. Para obter mais informações, consulte a [melhores práticas para desempenho melhorias usando o barramento de mensagens mediadas do Service](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Solução
Ver [QueueClient classe (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) para obter informações sobre como utilizar o método assíncrono recomendado.

Para melhorar o desempenho da infraestrutura de mensagens do Azure, veja o padrão de design [manual básico de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Considere a criação de partições de filas do Service Bus e tópicos
### <a name="id"></a>ID
AP2004

### <a name="description"></a>Descrição
Filas do Service Bus de partição e tópicos para um melhor desempenho com mensagens do Service Bus.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Criação de partições de filas do Service Bus e tópicos aumenta a disponibilidade de débito e o serviço de desempenho, uma vez que o débito total de uma fila particionada ou um tópico já não é limitado pelo desempenho de um mediador de mensagem única ou arquivo de mensagens. Além disso, uma indisponibilidade temporária de um arquivo de mensagens não faz uma particionada fila ou tópico indisponível. Para obter mais informações, consulte [entidades de mensagens de criação de partições](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Solução
O fragmento de código seguinte mostra como criar partições entidades de mensagens.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Para obter mais informações, consulte [particionados de filas do Service Bus e tópicos | Blogue do Microsoft Azure](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) e verifique a [fila de partições do Microsoft Azure Service Bus](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) exemplo.

## <a name="do-not-set-sharedaccessstarttime"></a>Não defina SharedAccessStartTime
### <a name="id"></a>ID
AP3001

### <a name="description"></a>Descrição
Deve evitar utilizar SharedAccessStartTimeset para a hora atual para iniciar imediatamente a política de acesso partilhado. Apenas terá de definir esta propriedade se deseja iniciar a política de acesso partilhado numa altura posterior.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Sincronização do relógio faz com que uma ligeira diferença entre os datacenters. Por exemplo, que logicamente parece definir a hora de início de uma política SAS de armazenamento como a hora atual usando DateTime. Now ou um método semelhante fará com que a política SAS entrar em vigor imediatamente. No entanto, as diferenças de horário ligeira entre centros de dados podem causar problemas com isso, uma vez que algumas vezes de centro de dados podem ser um pouco posteriores à hora de início, enquanto outros à frente-lo. Como resultado, a política SAS pode expirar rapidamente (ou até mesmo imediatamente) se o tempo de vida de política estiver definido demasiado curto.

Para obter mais orientações sobre como utilizar a assinatura de acesso partilhado no armazenamento do Azure, consulte [apresentando tabela SAS (assinatura de acesso partilhado), SAS da fila e de atualização de SAS do Blob - blogue da equipa do armazenamento de Azure de Microsoft - Home page - Site Blogs do MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Solução
Remova a instrução que define a hora de início da política de acesso partilhado. A ferramenta de análise de código do Azure fornece uma correção para este problema. Para obter mais informações sobre o gerenciamento de segurança, consulte o padrão de design [padrão de chave Valet](https://msdn.microsoft.com/library/dn568102.aspx).

O fragmento de código seguinte demonstra a correção de código para este problema.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Partilhado a política de acesso a hora de expiração tem de ser mais de cinco minutos
### <a name="id"></a>ID
AP3002

### <a name="description"></a>Descrição
Pode haver tanto quanto de cinco minutos diferença no relógios entre os datacenters em diferentes localizações devido a uma condição conhecido como "distorção." Para impedir que a SAS o token de política de expirar definido anteriormente que o planejado, o tempo de expiração para ser mais de cinco minutos.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Sincronizar os Datacenters em diferentes localizações em todo o mundo por um sinal de relógio. Uma vez que demora algum tempo para o sinal de relógio para viajar para diferentes localizações, pode haver um desvio de tempo entre os datacenters em localizações geográficas diferentes apesar de tudo o que está a ser sincronizado supostamente. Essa diferença de tempo pode afetar o acesso partilhado início tempo e de expiração do intervalo da política. Por conseguinte, para garantir a política de acesso partilhado entra em vigor imediatamente, a não especifica a hora de início. Além disso, certifique-se de que a hora de expiração é mais de 5 minutos para impedir que o tempo limite antecipada.

Para obter mais informações sobre como utilizar a assinatura de acesso partilhado no armazenamento do Azure, consulte [apresentando tabela SAS (assinatura de acesso partilhado), SAS da fila e de atualização de SAS do Blob - blogue da equipa do armazenamento de Azure de Microsoft - Home page - Site Blogs do MSDN](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Solução
Para obter mais informações sobre o gerenciamento de segurança, consulte o padrão de design [padrão de chave Valet](https://msdn.microsoft.com/library/dn568102.aspx).

Segue-se um exemplo de não especificar uma hora de início de política de acesso partilhado.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Segue-se um exemplo de especificar uma hora de início de política de acesso partilhado com uma duração de expiração de política maior do que cinco minutos.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Para obter mais informações, consulte [criar e utilizar uma assinatura de acesso partilhado](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Usar o CloudConfigurationManager
### <a name="id"></a>ID
AP4000

### <a name="description"></a>Descrição
Utilizar o [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) para projetos de classe, como o Web site do Azure e serviços móveis do Azure não será preciso introduzir problemas de tempo de execução. Como melhor prática, no entanto, é uma boa idéia usar na nuvem[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) como uma forma unificada de gerenciamento de configurações para todas as aplicações na Cloud do Azure.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
CloudConfigurationManager lê o arquivo de configuração adequado para o ambiente de aplicativo.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Solução
Refatorar o código para usar o [classe CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Uma solução de código para este problema é fornecida pela ferramenta de análise de código do Azure.

O fragmento de código seguinte demonstra a correção de código para este problema. Substituir

`var settings = ConfigurationManager.AppSettings["mySettings"];`

com

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Eis um exemplo de como armazenar a definição de configuração num arquivo App. config ou Web. config. Adicione as definições para a secção appSettings do ficheiro de configuração. Segue-se o ficheiro Web. config no exemplo de código anterior.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Evitar o uso de cadeias de ligação codificados
### <a name="id"></a>ID
AP4001

### <a name="description"></a>Descrição
Se usar cadeias de ligação codificados e terá de atualizá-los mais tarde, terá de fazer alterações ao seu código-fonte e recompilar o aplicativo. No entanto, se armazenar cadeias de ligação num ficheiro de configuração, pode alterá-los mais tarde ao simplesmente atualizar o ficheiro de configuração.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Embutir no código cadeias de ligação é uma prática inadequada porque introduz problemas quando as cadeias de ligação tem de ser alterado rapidamente. Além disso, se o projeto tem de ser verificado controle de origem, as cadeias de ligação hard-coded introduzem vulnerabilidades de segurança, uma vez que as cadeias de caracteres podem ser visualizadas no código-fonte.

### <a name="solution"></a>Solução
Store cadeias de ligação em arquivos de configuração ou ambientes do Azure.

* Para aplicativos autônomos, utilize o App. config para armazenar definições de cadeia de ligação.
* Para aplicações web alojadas no IIS, utilize o Web. config para armazenar as cadeias de ligação.
* Para aplicativos do ASP.NET vNext, utilize configuration.json para armazenar as cadeias de ligação.

Para obter informações sobre como usar arquivos de configurações, como Web. config ou App. config, consulte [diretrizes de configuração do ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Para obter informações sobre como o Azure funcionam das variáveis de ambiente, consulte [Web Sites do Azure: como cadeias de caracteres para a aplicação e trabalhar de seqüências de caracteres de conexão](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Para obter informações sobre o armazenamento de cadeia de ligação no controlo de origem, consulte [evitam colocar informações confidenciais, como cadeias de ligação nos ficheiros que estão armazenados no repositório de código fonte](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Utilize o ficheiro de configuração de diagnósticos
### <a name="id"></a>ID
AP5000

### <a name="description"></a>Descrição
Em vez de configurar as definições de diagnóstico no seu código, como ao utilizar a API de programação de Microsoft.WindowsAzure.Diagnostics, deve configurar as definições de diagnóstico no arquivo diagnostics.wadcfg. (Ou, diagnostics.wadcfgx se utilizar o Azure SDK 2.5). Ao fazer isso, pode alterar as definições de diagnóstico sem precisar recompilar o seu código.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Antes de que foi possível configurar o Azure SDK 2.5 (que usa o diagnóstico do Azure 1.3), o diagnóstico do Azure (WAD), utilizando vários métodos diferentes: adicioná-lo para o blob de configuração no armazenamento, utilizando código imperativo, configuração declarativa ou a predefinição configuração. No entanto, na forma preferida de configurar os diagnósticos é usar um arquivo de configuração XML (diagnostics.wadcfg ou diagnositcs.wadcfgx para SDK 2.5 e versões posteriores) no projeto de aplicativo. Nesta abordagem, o ficheiro de diagnostics.wadcfg completamente define a configuração e pode ser atualizado e reimplantado à vontade. Mistura a utilização do ficheiro de configuração diagnostics.wadcfg com os métodos programáticos de definição de configurações utilizando a [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)ou [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes podem gerar confusão. Ver [Initialize ou alterar a configuração do diagnóstico do Azure](https://msdn.microsoft.com/library/azure/hh411537.aspx) para obter mais informações.

A partir do 1.3 WAD (incluído com o Azure SDK 2.5), já não é possível usar o código para configurar os diagnósticos. Como resultado, apenas pode fornecer a configuração ao aplicar ou atualizar a extensão de diagnóstico.

### <a name="solution"></a>Solução
Utilize o estruturador de configuração de diagnósticos para mover as definições de diagnóstico para o ficheiro de configuração de diagnósticos (diagnositcs.wadcfg ou diagnositcs.wadcfgx para SDK 2.5 e posterior). Também é recomendável que instale [do Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) e utilizar a funcionalidade de diagnóstico mais recente.

1. No menu de atalho para a função que pretende configurar, escolha Propriedades e, em seguida, selecione a guia de configuração.
2. Na **diagnóstico** secção, certifique-se de que o **ativar diagnósticos** caixa de verificação está selecionada.
3. Escolha o **configurar** botão.

   ![Aceder a opção de ativar o diagnóstico](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Ver [configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) para obter mais informações.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Evitar a declarar os objetos de DbContext como estática
### <a name="id"></a>ID
AP6000

### <a name="description"></a>Descrição
Para poupar memória, evite a declarar os objetos de DBContext como estática.

Partilhe ideias e seus comentários na [comentários de análise de código do Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razão
Objetos de DBContext armazenar os resultados da consulta de cada chamada. Objetos de DBContext estáticos não são eliminados depois até o domínio do aplicativo é descarregado. Por conseguinte, um objeto DBContext estático pode consumir grandes quantidades de memória.

### <a name="solution"></a>Solução
Declarar DBContext como uma variável local ou o campo de instância não estático, utilizá-lo para uma tarefa e, em seguida, permitir que o mesmo ser descartados após o uso.

O classe controller do MVC de exemplo seguinte mostra como usar o objeto DBContext.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre Otimização e resolução de problemas de aplicações do Azure, veja [resolver problemas de uma aplicação web no serviço de aplicações do Azure com o Visual Studio](app-service/web-sites-dotnet-troubleshoot-visual-studio.md).
