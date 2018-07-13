---
title: Como utilizar os tópicos do Service Bus (Ruby) | Documentos da Microsoft
description: Saiba como utilizar os tópicos do Service Bus e subscrições no Azure. Exemplos de código estão escritos para aplicações Ruby.
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38712970"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Como utilizar tópicos do Service Bus e as subscrições com Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Este artigo descreve como utilizar tópicos do Service Bus e as subscrições de aplicações Ruby. Os cenários abrangidos incluem **criação de tópicos e subscrições, criar filtros de subscrição, envio de mensagens** para um tópico **receber mensagens de uma subscrição**, e **a eliminar tópicos e subscrições**. Para obter mais informações sobre tópicos e subscrições, veja a [passos seguintes](#next-steps) secção.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Criar um tópico
O **Azure::ServiceBusService** objeto permite-lhe trabalhar com tópicos. O código seguinte cria um **Azure::ServiceBusService** objeto. Para criar um tópico, utilize o `create_topic()` método. O exemplo seguinte cria um tópico ou imprime os erros, se existir alguma.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Também é possível passar um **Azure::ServiceBus::Topic** objeto com opções adicionais, que permitem-lhe substituir as predefinições de tópico, como o tempo de tamanho da fila ao vivo ou máximo da mensagem. O exemplo seguinte mostra a definir o tamanho máximo da fila para 5 GB e tempo de duração para 1 minuto:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Criar subscrições
Subscrições de tópicos também são criadas com o **Azure::ServiceBusService** objeto. As assinaturas são nomeadas e podem ter um filtro opcional que restringe o conjunto de mensagens entregues à fila virtual da subscrição.

As subscrições são persistentes e continuarão a existir até a eles ou o tópico estão associados com, são eliminados. Se a sua aplicação contém lógica para criar uma subscrição, deve primeiro verificar se a subscrição já existe ao utilizar o método de getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Criar uma subscrição com o filtro (MatchAll) predefinido
O filtro **MatchAll** é o filtro predefinido utilizado se não for especificado qualquer filtro na criação de uma nova subscrição. Quando o **MatchAll** filtro é utilizado, todas as mensagens publicadas para o tópico são colocadas na fila virtual da subscrição. O exemplo seguinte cria uma subscrição com o nome "mensagens de todos os" e utiliza a predefinição **MatchAll** filtro.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Criar subscrições com filtros
Também pode definir filtros que permitem-lhe especificar que as mensagens enviadas para um tópico devem aparecer no espaço de uma subscrição específica.

O tipo mais flexível de filtro suportado pelas subscrições é a **Azure::ServiceBus::SqlFilter**, que implementa um subconjunto de SQL92. Os filtros do SQL operam nas propriedades das mensagens publicadas para o tópico. Para obter mais detalhes sobre as expressões que podem ser utilizadas com um filtro SQL, reveja os [SqlFilter](service-bus-messaging-sql-filter.md) sintaxe.

Pode adicionar filtros a uma subscrição, utilizando o `create_rule()` método da **Azure::ServiceBusService** objeto. Este método permite-lhe adicionar novos filtros a uma subscrição existente.

Uma vez que o filtro predefinido é aplicado automaticamente a todas as novas subscrições, primeiro tem de remover o filtro predefinido, ou o **MatchAll** irá substituir todos os filtros que pode especificar. Pode remover a regra predefinida, utilizando o `delete_rule()` método no **Azure::ServiceBusService** objeto.

O exemplo seguinte cria uma subscrição com o nome "mensagens de alta", com uma **Azure::ServiceBus::SqlFilter** que seleciona apenas as mensagens possuem `message_number` propriedade superior a 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Da mesma forma, o exemplo seguinte cria uma subscrição com o nome `low-messages` com um **Azure::ServiceBus::SqlFilter** que seleciona apenas as mensagens com um `message_number` propriedade menor ou igual a 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Quando uma mensagem é agora enviada `test-topic`, é sempre ser entregue aos destinatários para a `all-messages` subscrição de tópico e entregue seletivamente aos destinatários a `high-messages` e `low-messages` (dependendo de subscrições de tópicos Quando o conteúdo de mensagem).

## <a name="send-messages-to-a-topic"></a>Enviar mensagens para um tópico
Para enviar uma mensagem para um tópico do Service Bus, a aplicação tem de utilizar o `send_topic_message()` método no **Azure::ServiceBusService** objeto. As mensagens enviadas para tópicos do Service Bus são instâncias do **Azure::ServiceBus::BrokeredMessage** objetos. **Azure::ServiceBus::BrokeredMessage** objetos têm um conjunto de propriedades padrão (tais como `label` e `time_to_live`), um dicionário utilizado para reter propriedades personalizadas de específicas da aplicação e um corpo de dados de cadeia de caracteres. Uma aplicação pode definir o corpo da mensagem, passando um valor de cadeia de caracteres para o `send_topic_message()` método e quaisquer necessárias propriedades padrão serão preenchidas pelo valores predefinidos.

O exemplo seguinte demonstra como enviar cinco mensagens de teste para `test-topic`. Tenha em atenção que o `message_number` valor de propriedade personalizada de cada mensagem varia com a iteração do ciclo (Isto determina qual assinatura o recebe ele):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Os tópicos do Service Bus suportam um tamanho da mensagem máximo de 256 KB no [escalão Padrão](service-bus-premium-messaging.md) e de 1 MB no [escalão Premium](service-bus-premium-messaging.md). O cabeçalho, que inclui as propriedades da aplicação padrão e personalizadas, pode ter um tamanho máximo de 64 KB. Não existe qualquer limite no número de mensagens contidas num tópico, contudo, existe um limite do tamanho total das mensagens contidas num tópico. O tamanho do tópico é definido no momento de criação, com um limite superior de 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Receber mensagens de uma subscrição
As mensagens são recebidas a partir de uma subscrição com o `receive_subscription_message()` método no **Azure::ServiceBusService** objeto. Por predefinição, as mensagens são read(peak) e bloqueado sem eliminá-lo a partir da subscrição. Pode ler e eliminar a mensagem da subscrição, definindo a `peek_lock` a opção de **false**.

O comportamento padrão torna a leitura e a eliminação de uma operação de duas etapas, que também torna possível oferecer suporte a aplicativos que não toleram mensagens em falta. Quando o Service Bus recebe um pedido, localiza a mensagem seguinte a ser consumida, bloqueia-a para impedir a respetiva receção por outros consumidores e, em seguida, devolve a mesma à aplicação. Depois da aplicação concluir o processamento da mensagem (ou armazena-lo de forma fiável para processamento futuro), ele conclui a segunda etapa do processo de receção ao chamar `delete_subscription_message()` método e fornecer a mensagem a ser eliminada como um parâmetro. O `delete_subscription_message()` método irá marcar a mensagem como consumida e removê-lo a partir da subscrição.

Se o `:peek_lock` parâmetro estiver definido como **false**, ler e eliminar a mensagem se torna o modelo mais simples e funciona melhor para cenários em que uma aplicação pode tolerar o não processamento de uma mensagem em caso de falha. Para compreender isto, considere um cenário em que o consumidor emite o pedido de receção e, em seguida, o sistema falha antes do respetivo processamento. Uma vez que o Service Bus terá marcado a mensagem como consumida, em seguida, quando a aplicação reinicia e começa a consumir novamente mensagens, terá perdido a mensagem consumida antes da falha de sistema.

O exemplo seguinte demonstra como as mensagens podem ser recebidas e processados usando `receive_subscription_message()`. O exemplo recebe e elimina uma mensagem do `low-messages` subscrição utilizando `:peek_lock` definido como **false**, em seguida, receber outra mensagem do `high-messages` e, em seguida, elimina a mensagem com `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Como processar falhas da aplicação e mensagens ilegíveis
O Service Bus fornece funcionalidades para ajudar a recuperar corretamente de erros na sua aplicação ou problemas no processamento de uma mensagem. Se uma aplicação recetora não é possível processar a mensagem por algum motivo, então pode chamar o `unlock_subscription_message()` método no **Azure::ServiceBusService** objeto. Tal faz com que o Service Bus desbloqueie a mensagem na subscrição e torna a mesma disponível para ser novamente recebida, quer pela mesma aplicação de consumo quer por outra aplicação de consumo.

Há também um tempo limite associado à mensagem bloqueada na subscrição e, se a aplicação conseguir processar a mensagem antes do tempo limite de bloqueio expira (por exemplo, se a falha da aplicação), o Service Bus desbloqueie a mensagem automaticamente e disponibilizá-lo ser recebida novamente.

No caso de falha da aplicação após o processamento da mensagem, mas antes a `delete_subscription_message()` método é chamado, em seguida, a mensagem é reenviada para a aplicação quando esta reiniciar. Isto é frequentemente chamado *, pelo menos, uma vez processamento*; ou seja, cada mensagem será processada pelo menos uma vez, mas em determinadas situações a mesma mensagem poderá ser reenviada. Se o cenário não conseguir tolerar o processamento duplicado, os programadores da aplicação devem acrescentar uma lógica adicional à aplicação para processar a entrega da mensagem duplicada. Essa lógica é, frequentemente, conseguida utilizando o `message_id` propriedade da mensagem, que permanecerá constante nas tentativas de entrega.

## <a name="delete-topics-and-subscriptions"></a>Eliminar tópicos e subscrições
Tópicos e subscrições são persistentes e tem de ser explicitamente eliminada através da [portal do Azure] [ Azure portal] ou programaticamente. O exemplo a seguir mostra como eliminar o tópico com o nome `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

A eliminação de um tópico elimina também quaisquer subscrições registadas com o tópico. As subscrições também podem ser eliminadas de modo independente. O código a seguir mostra como eliminar a subscrição com o nome `high-messages` partir o `test-topic` tópico:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu as noções básicas de tópicos do Service Bus, siga estas ligações para saber mais.

* Ver [filas, tópicos e subscrições](service-bus-queues-topics-subscriptions.md).
* Referência da API para [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Visite o [Azure SDK para Ruby](https://github.com/Azure/azure-sdk-for-ruby) repositório no GitHub.

[Azure portal]: https://portal.azure.com
