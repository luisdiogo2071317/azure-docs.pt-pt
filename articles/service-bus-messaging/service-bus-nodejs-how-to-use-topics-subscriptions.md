---
title: Como utilizar os tópicos de Service Bus do Azure e as subscrições com o Node.js | Microsoft Docs
description: Saiba como utilizar o Service Bus tópicos e subscrições no Azure a partir de uma aplicação Node.js.
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: d3a7ebd135f705a6a3ea91feb4e037a9ed6d0c79
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641310"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a>Como tópicos do barramento de serviço de utilização e as subscrições com o Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Este guia descreve como utilizar tópicos do Service Bus e as subscrições de aplicações Node.js. Os cenários abrangidos incluem:

- Criação de tópicos e subscrições 
- Criar filtros de subscrição 
- Enviar mensagens para um tópico 
- Receber mensagens de uma subscrição
- Eliminar tópicos e subscrições 

Para obter mais informações sobre os tópicos e subscrições, consulte [passos](#next-steps) secção.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Criar uma aplicação Node.js
Crie uma aplicação Node.js em branco. Para obter instruções sobre como criar uma aplicação Node.js, consulte [criar e implementar uma aplicação Node.js para um Web Site do Azure], [serviço em nuvem de Node.js] [ Node.js Cloud Service] utilizando o Windows PowerShell ou do Web Site com o WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Configurar a sua aplicação para utilizar o Service Bus
Para utilizar o Service Bus, transfira o pacote do Azure do Node.js. Este pacote inclui um conjunto de bibliotecas que comunicam com os serviços de REST do Service Bus.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Utilize o Gestor de pacote de nó (NPM) para obter o pacote
1. Abra uma interface de linha de comandos, tal como **PowerShell** (Windows), **Terminal** (Mac), ou **Bash** (Unix).
2. Navegue para a pasta onde criou o seu exemplo de aplicação.
3. Tipo **npm instalar azure** na janela de comando, que deve resultar no seguinte resultado:

   ```
       azure@0.7.5 node_modules\azure
   ├── dateformat@1.0.2-1.2.3
   ├── xmlbuilder@0.4.2
   ├── node-uuid@1.2.0
   ├── mime@1.2.9
   ├── underscore@1.4.4
   ├── validator@1.1.1
   ├── tunnel@0.0.2
   ├── wns@0.5.3
   ├── xml2js@0.2.7 (sax@0.5.2)
   └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
   ```
3. Pode executar manualmente o **ls** comando para verificar se um **nó\_módulos** pasta foi criada. Dentro dessa pasta localizar o **azure** pacote, que contém as bibliotecas que precisa de aceder a tópicos do Service Bus.

### <a name="import-the-module"></a>Importe o módulo
Utilizar o bloco de notas ou noutro editor de texto, adicione o seguinte na parte superior do **server.js** ficheiro da aplicação:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Configurar uma ligação de barramento de serviço
O módulo do Azure lê a variável de ambiente `AZURE_SERVICEBUS_CONNECTION_STRING` da cadeia de ligação que obteve no passo anterior, "obter as credenciais". Se esta variável de ambiente não for definida, tem de especificar as informações de conta ao chamar `createServiceBusService`.

Para obter um exemplo de definir as variáveis de ambiente para um serviço em nuvem do Azure, consulte [Node.js o serviço em nuvem com o armazenamento][Node.js Cloud Service with Storage].



## <a name="create-a-topic"></a>Criar um tópico
O **ServiceBusService** objeto permite-lhe funcionar com tópicos. O código seguinte cria um **ServiceBusService** objeto. Adicione-a junto ao topo do **server.js** ficheiro, após a declaração a importar o módulo do azure:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Se chamar `createTopicIfNotExists` no **ServiceBusService** objeto, o tópico especificado é devolvido (caso exista), ou não foi criado um novo tópico com o nome especificado. O seguinte código utiliza `createTopicIfNotExists` para criar ou ligue para o tópico com o nome `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

O `createServiceBusService` método também suporta opções adicionais, que permitem-lhe substituir as definições predefinidas da tópico como hora TTL da mensagem ou o tamanho máximo do tópico. 

O exemplo a seguir define o tamanho máximo de tópico 5 GB com um TTL de um minuto:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtros
Operações de filtragem opcionais podem ser aplicadas a operações executadas utilizando **ServiceBusService**. Operações de filtragem pode incluir registo automaticamente repetir, etc. Os filtros são objetos que implementam um método com a assinatura:

```javascript
function handle (requestOptions, next)
```

Depois de efetuar pré-processamentos nas opções de pedido, chama o método `next`e passa uma chamada de retorno com a assinatura seguinte:

```javascript
function (returnObject, finalCallback, next)
```

Esta chamada de retorno e após o processamento de `returnObject` (a resposta do pedido para o servidor), a chamada de retorno tem invocar seguinte (se existir) para continuar a processar outros filtros ou invocar `finalCallback` para terminar a invocação de serviço.

Dois filtros que implementa lógica de repetição são incluídos com o Azure SDK para Node.js, **ExponentialRetryPolicyFilter** e **LinearRetryPolicyFilter**. O seguinte cria um **ServiceBusService** objeto que utiliza o **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Criar subscrições
Subscrições de tópico também são criadas com o **ServiceBusService** objeto. As subscrições são denominadas e podem ter um filtro opcional que restringe o conjunto de mensagens entregues nos fila virtual da subscrição.

> [!NOTE]
> As subscrições sejam persistentes até que ambos estes ou tópico estão associados com, são eliminados. Se a sua aplicação contém lógica para criar uma subscrição, este deve primeiro verificar se a subscrição existe utilizando o `getSubscription` método.
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Criar uma subscrição com o filtro (MatchAll) predefinido
O **MatchAll** filtro é o filtro predefinido utilizado quando é criada uma subscrição. Quando utiliza o filtro **MatchAll**, todas as mensagens publicadas para o tópico são colocadas na fila virtual da subscrição. O exemplo seguinte cria uma subscrição designada AllMessages e utiliza a predefinição **MatchAll** filtro.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Criar subscrições com filtros
Também pode criar filtros que permitem-lhe âmbito que as mensagens enviadas para um tópico deve aparecer dentro de uma subscrição de tópico específica.

O tipo mais flexível do filtro suportado pelas subscrições é a **SqlFilter**, que implementa um subconjunto de SQL92. Os filtros do SQL operam nas propriedades das mensagens publicadas para o tópico. Para obter mais detalhes sobre as expressões que podem ser utilizadas com um filtro SQL, consulte o [Sqlexpression] [ SqlFilter.SqlExpression] sintaxe.

Os filtros podem ser adicionados a uma subscrição utilizando o `createRule` método o **ServiceBusService** objeto. Este método permite-lhe adicionar novos filtros para uma subscrição existente.

> [!NOTE]
> Porque o filtro predefinido é aplicado automaticamente para todas as novas subscrições, primeiro tem de remover o filtro predefinido ou o **MatchAll** substituirão quaisquer outros filtros pode especificar. Pode remover a regra predefinida, utilizando o `deleteRule` método o **ServiceBusService** objeto.
>
>

O exemplo seguinte cria uma subscrição designada `HighMessages` com um **SqlFilter** que seleciona apenas as mensagens com um personalizado `messagenumber` propriedade superior a 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Da mesma forma, o exemplo seguinte cria uma subscrição designada `LowMessages` com um **SqlFilter** que seleciona apenas as mensagens com um `messagenumber` propriedade inferior ou igual a 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Quando agora é enviada uma mensagem para `MyTopic`, irá ser entregue para o `AllMessages` subscrição do tópico e entregue seletivamente subscrever o `HighMessages` e `LowMessages` subscrições (consoante o conteúdo da mensagem).

## <a name="how-to-send-messages-to-a-topic"></a>Como enviar mensagens para um tópico
Para enviar uma mensagem para um tópico de barramento de serviço, a aplicação tem de utilizar o `sendTopicMessage` método o **ServiceBusService** objeto.
As mensagens enviadas para tópicos do Service Bus são **BrokeredMessage** objetos.
**BrokeredMessage** objetos têm um conjunto de propriedades padrão (tais como `Label` e `TimeToLive`), um dicionário utilizado para reter propriedades personalizadas de específicas da aplicação e um corpo de dados de cadeia. Uma aplicação pode definir o corpo da mensagem através da transmissão de um valor de cadeia para o `sendTopicMessage` e necessárias propriedades padrão são preenchidas por valores predefinidos.

O exemplo seguinte mostra como enviar cinco mensagens de teste para `MyTopic`. O `messagenumber` varia de acordo com o valor da propriedade de cada mensagem no iteração do ciclo (Isto determina quais as subscrições receberem):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Os tópicos do Service Bus suportam um tamanho da mensagem máximo de 256 KB no [escalão Padrão](service-bus-premium-messaging.md) e de 1 MB no [escalão Premium](service-bus-premium-messaging.md). O cabeçalho, que inclui as propriedades da aplicação padrão e personalizadas, pode ter um tamanho máximo de 64 KB. Não há nenhum limite no número de mensagens contidas num tópico, mas não existe um limite de tamanho total das mensagens contidas num tópico. O tamanho do tópico é definido no momento de criação, com um limite superior de 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Receber mensagens de uma subscrição
As mensagens são recebidas a partir de uma subscrição utilizando o `receiveSubscriptionMessage` método no **ServiceBusService** objeto. Por predefinição, as mensagens são eliminadas da subscrição, que são de leitura. No entanto, pode definir o parâmetro opcional `isPeekLock` para **verdadeiro** (peek) de ler e bloquear a mensagem sem eliminá-lo da subscrição.

O comportamento predefinido de ler e eliminar a mensagem como parte da operação de receção é o modelo mais simples e funciona melhor para cenários em que uma aplicação pode tolerar o não processamento de uma mensagem no caso de falha. Para compreender este comportamento, considere um cenário em que o consumidor emite o pedido de receção e, em seguida, falhas antes do processamento-lo. Porque o Service Bus terá marcado a mensagem como consumida, em seguida, quando a aplicação reinicia e começa a consumir novamente mensagens, terá perdido a mensagem consumida antes da falha.

Se o `isPeekLock` parâmetro está definido como **verdadeiro**, a receção torna-se uma operação de duas etapas que possibilita o suporte de aplicações que não toleram mensagens em falta. Quando o Service Bus recebe um pedido, localiza a mensagem seguinte para consumir, bloqueia-a para impedir a receção outros consumidores e devolve a mesma à aplicação.
Após a aplicação processa a mensagem (ou armazena a mesma forma fiável para processamento futuro), conclui a segunda etapa do processo de receção ao chamar **deleteMessage** método e passa a mensagem para eliminar como parâmetro. O **deleteMessage** método marca a mensagem como consumida e remove a mesma da subscrição.

O exemplo seguinte demonstra como podem ser recebidas mensagens e processados utilizando `receiveSubscriptionMessage`. O exemplo recebe pela primeira vez e elimina uma mensagem da subscrição 'LowMessages' e, em seguida, recebe uma mensagem da utilização de subscrição de 'HighMessages' `isPeekLock` definido como verdadeiro. Em seguida, elimina a mensagem utilizando `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Como processar falhas da aplicação e mensagens ilegíveis
O Service Bus fornece funcionalidades para ajudar a recuperar corretamente de erros na sua aplicação ou problemas no processamento de uma mensagem. Se uma aplicação recetora não é possível processar a mensagem por algum motivo, em seguida, pode chamar o `unlockMessage` método no **ServiceBusService** objeto. Este método faz com que o Service Bus desbloqueie a mensagem dentro da subscrição e disponibilizá-lo ser recebida novamente. Neste caso, pela mesma aplicação de consumo ou por outra aplicação de consumo.

Há também um tempo limite associado à mensagem bloqueada na subscrição. Se a aplicação não conseguir processar a mensagem antes do tempo limite de bloqueio expira (por exemplo, no caso de falha da aplicação), o Service Bus desbloqueia automaticamente a mensagem e torna a mesma disponível para ser recebida novamente.

No caso de falha da aplicação após o processamento da mensagem, mas antes o `deleteMessage` método é denominado, a mensagem é reenviada para a aplicação quando esta reiniciar. Este comportamento é frequentemente designado *, pelo menos, uma vez processamento*. Ou seja, cada mensagem é processada pelo menos uma vez, mas em determinadas situações a mesma mensagem poderá ser reenviada. Se o cenário não conseguir tolerar o processamento duplicado, deve adicionar lógica à sua aplicação para processar a entrega da mensagem duplicada. Pode utilizar o **MessageId** propriedade da mensagem, que permanece constante nas tentativas de entrega.

## <a name="delete-topics-and-subscriptions"></a>Eliminar tópicos e subscrições
Os tópicos e subscrições sejam persistentes e tem de ser explicitamente eliminado quer através de [portal do Azure] [ Azure portal] ou através de programação.
O exemplo seguinte demonstra como eliminar o tópico com o nome `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

A eliminação de um tópico elimina também quaisquer subscrições registadas com o tópico. As subscrições também podem ser eliminadas de modo independente. O exemplo seguinte mostra como eliminar uma subscrição designada `HighMessages` do `MyTopic` tópico:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Próximos Passos
Agora que aprendeu as noções básicas de tópicos do Service Bus, siga estas ligações para saber mais.

* Consulte [filas, tópicos e subscrições][Queues, topics, and subscriptions].
* Referência da API para [SqlFilter][SqlFilter].
* Visite o [Azure SDK para o nó] [ Azure SDK for Node] repositório no GitHub.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Criar e implementar uma aplicação Node.js para um Web Site do Azure]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

