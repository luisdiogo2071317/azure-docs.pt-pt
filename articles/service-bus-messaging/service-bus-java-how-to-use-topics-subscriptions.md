---
title: Como utilizar os tópicos do Service bus do Azure com Java | Documentos da Microsoft
description: Utilize tópicos do Service Bus e as subscrições no Azure.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/17/2017
ms.author: spelluru
ms.openlocfilehash: 0dc0ebd94abaa9dacd685034a46da1a7f204bfff
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700076"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-java"></a>Como utilizar os tópicos do Service Bus e as subscrições com Java

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Este guia descreve como utilizar tópicos do Service Bus e as subscrições. Os exemplos são escritos em Java e utilize o [Azure SDK para Java][Azure SDK for Java]. Os cenários abrangidos incluem **criar tópicos e subscrições**, **criar filtros de subscrição**, **enviar mensagens para um tópico**, **a receber mensagens de uma subscrição**, e **eliminar tópicos e subscrições**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>O que são os tópicos e as subscrições do Service Bus?
Os tópicos e as subscrições do Service Bus suportam um modelo de comunicação de mensagens de *publicação/subscrição*. Ao utilizar tópicos e subscrições, os componentes de uma aplicação distribuída não comunicam diretamente entre si; trocam antes mensagens através de um tópico, que funciona como um intermediário.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Ao contrário das filas do Service Bus, em que cada mensagem é processada por um único consumidor, os tópicos e as subscrições proporcionam uma forma de comunicação de “um-para-muitos”, utilizando um padrão de publicação/subscrição. É possível registar várias subscrições num tópico. Quando uma mensagem é enviada para um tópico, é depois disponibilizada para que cada subscrição a processe de forma independente.

Uma subscrição num tópico é semelhante a uma fila virtual que recebe cópias das mensagens que foram enviadas para o tópico. Opcionalmente, pode registar regras de filtro para um tópico numa base por subscrição, que permite filtrar/restringir que mensagens para um tópico são recebidas por que subscrições desse tópico.

Tópicos do Service Bus e as subscrições permitem-lhe dimensionar para processar um grande número de mensagens num grande número de utilizadores e aplicações.

## <a name="create-a-service-namespace"></a>Criar um espaço de nomes de serviço
Para começar a utilizar os tópicos do Service Bus e subscrições no Azure, primeiro tem de criar uma *espaço de nomes*, que fornece um contentor de âmbito para abordar os recursos de barramento de serviço na sua aplicação.

Para criar um espaço de nomes:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Configurar a sua aplicação para utilizar o Service Bus
Certifique-se de que instalou o [Azure SDK para Java] [ Azure SDK for Java] antes de criar este exemplo. Se estiver a utilizar Eclipse, pode instalar o [Azure Toolkit para Eclipse] [ Azure Toolkit for Eclipse] que inclui o Azure SDK para Java. Em seguida, pode adicionar os **bibliotecas do Microsoft Azure para Java** ao seu projeto:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Adicione o seguinte `import` declarações na parte superior do ficheiro Java:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Adicione as bibliotecas do Azure para Java ao seu caminho de compilação e incluí-lo em seu assembly de implantação do projeto.

## <a name="create-a-topic"></a>Criar um tópico
Operações de gestão para tópicos do Service Bus podem ser efetuadas pela **ServiceBusContract** classe. R **ServiceBusContract** objeto é construído com uma configuração apropriada que encapsula o token SAS com permissões para geri-lo, e o **ServiceBusContract** classe é o único ponto de comunicação com o Azure.

O **ServiceBusService** classe fornece métodos para criar, enumerar e eliminar tópicos. A exemplo a seguir mostra como um **ServiceBusService** objeto pode ser usado para criar um tópico com o nome `TestTopic`, com um namespace chamado `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Existem métodos na **TopicInfo** que permitem que as propriedades do tópico seja definido (por exemplo: definir o padrão time-to-live (TTL) valor a ser aplicado às mensagens enviadas para o tópico). O exemplo seguinte mostra como criar um tópico com o nome `TestTopic` com um tamanho máximo de 5 GB:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Pode utilizar o **listTopics** método no **ServiceBusContract** objetos para verificar se um tópico com um nome especificado já existe num espaço de nomes do serviço.

## <a name="create-subscriptions"></a>Criar subscrições
Subscrições de tópicos também são criadas com o **ServiceBusService** classe. As subscrições têm um nome e podem ter um filtro opcional que restringe o conjunto de mensagens transmitidas para a fila virtual da subscrição.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Criar uma subscrição com o filtro (MatchAll) predefinido
Se não for especificado nenhum filtro aquando da criação de uma nova subscrição, será utilizado o filtro predefinido **MatchAll**. Quando o **MatchAll** filtro é utilizado, todas as mensagens publicadas para o tópico são colocadas na fila virtual da subscrição. O exemplo seguinte cria uma subscrição com o nome "AllMessages" e utiliza o filtro **MatchAll** predefinido.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Criar subscrições com filtros
Também pode criar filtros que permitem ao âmbito de que as mensagens enviadas para um tópico devem aparecer no espaço de uma subscrição de tópico específico.

O tipo mais flexível de filtro suportado pelas subscrições é a [SqlFilter][SqlFilter], que implementa um subconjunto de SQL92. Os filtros do SQL operam nas propriedades das mensagens publicadas para o tópico. Para obter mais detalhes sobre as expressões que podem ser utilizadas com um filtro SQL, reveja os [Sqlexpression] [ SqlFilter.SqlExpression] sintaxe.

O exemplo seguinte cria uma subscrição com o nome `HighMessages` com um [SqlFilter] [ SqlFilter] objeto que seleciona apenas as mensagens personalizada **MessageNumber** propriedade é superior a 3:

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Da mesma forma, o exemplo seguinte cria uma subscrição com o nome `LowMessages` com um [SqlFilter] [ SqlFilter] objeto que seleciona apenas as mensagens que têm um **MessageNumber** propriedade menor ou igual a 3:

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Quando uma mensagem é agora enviada `TestTopic`, será sempre entregue aos destinatários a `AllMessages` subscrição e entregue seletivamente aos destinatários a `HighMessages` e `LowMessages` subscrições (consoante a conteúdo da mensagem).

## <a name="send-messages-to-a-topic"></a>Enviar mensagens para um tópico
Para enviar uma mensagem para um tópico do Service Bus, a aplicação obtém um **ServiceBusContract** objeto. O código a seguir demonstra como enviar uma mensagem o `TestTopic` tópico que criou anteriormente no `HowToSample` espaço de nomes:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

As mensagens enviadas para tópicos do Service Bus são instâncias do [BrokeredMessage] [ BrokeredMessage] classe. [BrokeredMessage][BrokeredMessage]* objetos têm um conjunto de métodos padrão (como **setLabel** e **TimeToLive**), um dicionário que é utilizado para armazenar personalizado propriedades específicas da aplicação e um corpo de dados arbitrários da aplicação. Uma aplicação pode definir o corpo da mensagem, passando qualquer objeto serializável para o construtor do [BrokeredMessage][BrokeredMessage]e o adequado **DataContractSerializer** , em seguida, é utilizado para serializar o objeto. Em alternativa, uma **java.io.InputStream** pode ser fornecido.

O exemplo seguinte demonstra como enviar cinco mensagens de teste para o `TestTopic` **MessageSender** obteve no trecho de código anterior.
Observe como o **MessageNumber** valor da propriedade de cada mensagem varia com a iteração do loop (este valor determina quais as subscrições que recebem a mesma):

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Os tópicos do Service Bus suportam um tamanho da mensagem máximo de 256 KB no [escalão Padrão](service-bus-premium-messaging.md) e de 1 MB no [escalão Premium](service-bus-premium-messaging.md). O cabeçalho, que inclui as propriedades da aplicação padrão e personalizadas, pode ter um tamanho máximo de 64 KB. Não existe nenhum limite no número de mensagens contidas num tópico, mas existe um limite do tamanho total das mensagens contidas num tópico. O tamanho do tópico é definido no momento de criação, com um limite superior de 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Como receber mensagens de uma subscrição
Para receber mensagens de uma subscrição, utilize um **ServiceBusContract** objeto. As mensagens recebidas podem funcionar em dois modos diferentes: **ReceiveAndDelete** e **PeekLock** (predefinição).

Ao utilizar o **ReceiveAndDelete** modo, receber é uma operação única - ou seja, quando o Service Bus recebe um pedido de leitura para uma mensagem, marca a mensagem como consumida e devolve a mesma à aplicação. **ReceiveAndDelete** modo é o modelo mais simples e funciona melhor para cenários em que uma aplicação pode tolerar o não processamento de uma mensagem se ocorrer uma falha. Por exemplo, considere um cenário em que o consumidor emite o pedido de receção e, em seguida, falha antes de processá-lo. Porque o Service Bus marcou a mensagem como consumida, em seguida, quando a aplicação reinicia e começa a consumir novamente mensagens, ele tem estado em falta a mensagem consumida antes da falha de sistema.

Na **PeekLock** modo, receber torna-se uma operação de duas etapas que possibilita o suporte de aplicações que não toleram mensagens em falta. Quando o Service Bus recebe um pedido, localiza a mensagem seguinte a ser consumida, bloqueia-a para impedir a respetiva receção por outros consumidores e, em seguida, devolve a mesma à aplicação. Depois da aplicação concluir o processamento da mensagem (ou armazena-lo de forma fiável para processamento futuro), ele conclui a segunda etapa do processo de receção ao chamar **eliminar** à mensagem recebida. Quando o Service Bus vê a **eliminar** chamada, ela marca a mensagem como consumida e remove-o do tópico.

O exemplo seguinte demonstra como as mensagens podem ser recebidas e processados usando **PeekLock** (o modo predefinido). O exemplo executa um loop e processa as mensagens no `HighMessages` subscrição e, em seguida, sai quando não existirem mais mensagens (em alternativa, pode ser configurada de espera para novas mensagens).

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Como processar falhas da aplicação e mensagens ilegíveis
O Service Bus fornece funcionalidades para ajudar a recuperar corretamente de erros na sua aplicação ou problemas no processamento de uma mensagem. Se uma aplicação recetora não é possível processar a mensagem por algum motivo, então pode chamar o **unlockMessage** método à mensagem recebida (em vez do **deleteMessage** método). Isso faz com que o Service Bus desbloqueie a mensagem dentro do tópico e torná-la disponível para ser novamente recebida, pela mesma aplicação de consumo ou por outra aplicação de consumo.

Há também um tempo limite associado à mensagem bloqueada no tópico e, se a aplicação conseguir processar a mensagem antes do tempo limite de bloqueio expira (por exemplo, se a falha da aplicação), o Service Bus desbloqueia automaticamente a mensagem e torna disponível para ser recebida novamente.

No caso de falha da aplicação após o processamento da mensagem, mas antes a **deleteMessage** solicitação é emitida, em seguida, a mensagem é reenviada para a aplicação quando esta reiniciar. Este processo é, muitas vezes, denominado **, pelo menos, uma vez processamento**; ou seja, cada mensagem é processada pelo menos uma vez, mas em determinadas situações a mesma mensagem poderá ser reenviada. Se o cenário não conseguir tolerar o processamento duplicado, os programadores da aplicação devem acrescentar uma lógica adicional à aplicação para processar a entrega da mensagem duplicada. Isto é, frequentemente, conseguido através da **getMessageId** método da mensagem, que permanece constante nas tentativas de entrega.

## <a name="delete-topics-and-subscriptions"></a>Eliminar tópicos e subscrições
A principal maneira de eliminar tópicos e subscrições está a utilizar um **ServiceBusContract** objeto. A eliminação de um tópico elimina também quaisquer subscrições registadas com o tópico. As subscrições também podem ser eliminadas de modo independente.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Próximos Passos
Agora que aprendeu as noções básicas de filas do Service Bus, veja [filas do Service Bus, tópicos e subscrições] [ Service Bus queues, topics, and subscriptions] para obter mais informações.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
