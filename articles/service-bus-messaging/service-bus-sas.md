---
title: Controlo de acesso do Service Bus do Azure com assinaturas de acesso partilhado | Microsoft Docs
description: Descrição geral do controlo de acesso do Service Bus utilizando a descrição geral de assinaturas de acesso partilhado, detalhes sobre a autorização de SAS com o Service Bus do Azure.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: sethm;clemensv
ms.openlocfilehash: 21e9e0a20842e365e40b71ac96888e7cd2056e52
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Controlo de acesso do Service Bus com assinaturas de acesso partilhado

*Assinaturas de acesso partilhado* (SAS) são o mecanismo de principal de segurança de mensagens do Service Bus. Este artigo aborda SAS, como funcionam e como utilizá-los de forma agnóstico plataforma.

SAS guards acesso ao Service Bus com base nas regras de autorização. Os são configurados num espaço de nomes ou uma entidade de mensagens (reencaminhamento, fila ou um tópico). Uma regra de autorização tem um nome, está associada com direitos específicos e acarreta um par de chaves criptográficas. Utilize o nome da regra e a chave através do SDK do Service Bus ou no seu próprio código para gerar um token SAS. Um cliente, em seguida, pode passar o token para o Service Bus para provar a autorização para a operação pedida.

## <a name="overview-of-sas"></a>Descrição geral da SAS

Assinaturas de acesso partilhado são um mecanismo de autorização baseada em afirmações utilizando tokens simples. Através da SAS, chaves nunca são transmitidas em risco. As chaves são utilizadas para assinar criptograficamente informações que mais tarde podem ser verificadas pelo serviço. SAS podem ser utilizado semelhante a um esquema de nome de utilizador e palavra-passe em que o cliente estiver na posse imediata de um nome de regra de autorização e uma chave correspondente. SAS também podem ser utilizado semelhante a um modelo de segurança federada, onde o cliente recebe um token de acesso de tempo limitado e assinado de um serviço de token de segurança sem nunca entra possua a chave de assinatura.

Se a autenticação SAS no Service Bus com o nome [as regras de autorização de acesso partilhado](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ter direitos de acesso e um par de chaves criptográficas primária e secundária associados. As chaves são valores de 256 bits em Base64 representação. Pode configurar regras ao nível do espaço de nomes, no Service Bus [reencaminha](service-bus-fundamentals-hybrid-solutions.md#relays), [filas](service-bus-fundamentals-hybrid-solutions.md#queues), e [tópicos](service-bus-fundamentals-hybrid-solutions.md#topics).

O [assinatura de acesso partilhado](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token contém o nome da regra de autorização escolhido, o URI do recurso que deverá ser acedido, uma expiração instantânea, e uma assinatura de criptografia HMAC SHA256 calculado sobre estes campos a utilizar o principal ou a chave criptográfica secundária da regra de autorização que escolheu.

## <a name="shared-access-authorization-policies"></a>Políticas de autorização de acesso partilhado

Cada espaço de nomes do Service Bus e cada entidade de barramento de serviço tem uma política de autorização de acesso partilhado constituída por regras. A política ao nível do espaço de nomes se aplica a todas as entidades dentro do espaço de nomes, independentemente da respetiva configuração de política individuais.

Para cada regra de política de autorização que opte por utilizar três informações: **nome**, **âmbito**, e **direitos**. O **nome** apenas que; é um nome exclusivo nesse âmbito. O âmbito é suficientemente fácil: é o URI do recurso em questão. Para um espaço de nomes do Service Bus, o âmbito é o nome de domínio completamente qualificado (FQDN), tal como `https://<yournamespace>.servicebus.windows.net/`.

Os direitos conferidos pela regra de política podem ser uma combinação de:

* 'Envio' - Confers o direito para enviar mensagens para a entidade
* 'Escutar' - Confers o direito de escutar (reencaminhamento) ou receber (fila, subscrições) e todos os relacionados com o processamento de mensagens
* "Gerir" - Confers o direito de gerir a topologia do espaço de nomes, incluindo criar e eliminar entidades

O direito de "Gerir" inclui os direitos 'Envio' e 'Receção'.

Uma política de espaço de nomes ou entidade pode conter até 12 regras de autorização de acesso partilhado, fornecendo espaço para três conjuntos de regras, cada que abrangem os direitos básicos e a combinação de envio e escuta. Este sublinhados de indicação do limite que armazenam a política SAS destina-se não seja um utilizador ou o arquivo de contas de serviço. Se a aplicação tem de conceder acesso ao Service Bus com base no utilizador ou de identidades do serviço, deve implementar um serviço de token de segurança que emite tokens de SAS após uma verificação de acesso e autenticação.

Uma regra de autorização é atribuída um *chave primária* e um *chave secundária*. Estes são chaves criptograficamente seguras. Não perca-los ou fuga-los - estejam sempre disponíveis no [portal do Azure][Azure portal]. Pode utilizar qualquer uma das chaves geradas e pode voltar a gerá-los em qualquer altura. Se voltar a gerar ou alterar uma chave na política, todos os anteriormente tokens emitiram com base nessa chave tornar-se de forma instantânea inválido. No entanto, as ligações em curso criadas com base nesses tokens irão continuar a funcionar até que o token expira.

Quando cria um espaço de nomes do Service Bus, uma regra de política com o nome **RootManageSharedAccessKey** é criado automaticamente para o espaço de nomes. Esta política tem de gerir permissões para todo o espaço de nomes. É recomendado que trate esta regra, como um administrativo **raiz** conta e não utilizá-lo na sua aplicação. Pode criar regras de política adicionais no **configurar** separador para o espaço de nomes no portal, através do Powershell ou a CLI do Azure.

## <a name="configuration-for-shared-access-signature-authentication"></a>Configuração de autenticação da assinatura de acesso partilhado

Pode configurar o [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regra em espaços de nomes do Service Bus, filas ou tópicos. Configurar um [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) um barramento de serviço subscrição atualmente não é suportada, mas pode utilizar regras configuradas num espaço de nomes ou tópico para proteger o acesso às subscrições. Para um exemplo de trabalho que ilustra a este procedimento, consulte o [autenticação a utilizar acesso assinatura partilhado (SAS) com as subscrições do Service Bus](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) exemplo.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Este figura a *manageRuleNS*, *sendRuleNS*, e *listenRuleNS* regras de autorização são aplicadas a fila Q1 e tópico T1, enquanto *listenRuleQ* e *sendRuleQ* aplicam-se apenas a fila Q1 e *sendRuleT* aplica-se apenas ao tópico T1.

## <a name="generate-a-shared-access-signature-token"></a>Gerar um token de assinatura de acesso partilhado

Qualquer cliente que tenha acesso ao nome de um nome de regra de autorização e das respetivas chaves de assinatura podem gerar um token SAS. O token é gerado por uma cadeia no seguinte formato de composição:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`** -De expiração do token instantânea. Número inteiro ao refletir segundos desde a época `00:00:00 UTC` no de 1970 1 de Janeiro (época UNIX) quando o token expira.
* **`skn`** -Nome da regra de autorização.
* **`sr`** -URI do recurso que está a ser acedido.
* **`sig`** -A assinatura.

O `signature-string` o hash SHA-256 é calculado através do URI do recurso (**âmbito** conforme descrito na secção anterior) e a representação de cadeia da expiração do token instantânea, separados por CRLF.

O cálculo de hash semelhante para o seguinte código pseudo e devolve um valor de hash de 256 bits/32-byte.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

O token contém os valores não protegido por hash para que o destinatário pode recalcular o hash com os mesmos parâmetros, verificar se o emissor está na posse de uma chave de assinatura válida. 

O URI do recurso é o URI do recurso de barramento de serviço ao qual foi reclamado acesso completo. Por exemplo, `http://<namespace>.servicebus.windows.net/<entityPath>` ou `sb://<namespace>.servicebus.windows.net/<entityPath>`; ou seja, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. O URI tem de ser [codificado por cento](https://msdn.microsoft.com/library/4fkewx0t.aspx).

A regra de autorização de acesso partilhado utilizada para a assinatura tem de ser configurada na entidade especificada por este URI ou por um dos respetivos pais hierárquicas. Por exemplo, `http://contoso.servicebus.windows.net/contosoTopics/T1` ou `http://contoso.servicebus.windows.net` no exemplo anterior.

Um token SAS é válido para o prefixo de todos os recursos a `<resourceURI>` utilizado o `signature-string`.

## <a name="regenerating-keys"></a>Voltar a gerar chaves

É recomendado que periodicamente regenerar as chaves utilizadas no [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objeto. Os blocos de chaves primários e secundários existem, de modo a que pode rodar chaves gradualmente. Se a sua aplicação geralmente utiliza a chave primária, pode copiar a chave primária para a ranhura de chave secundária e apenas, em seguida, voltar a gerar a chave primária. O novo valor de chave primário, em seguida, pode ser configurado para as aplicações de cliente, continuaram acesso utilizando a chave primária antiga na ranhura de secundária. Depois de todos os clientes são atualizados, pode voltar a gerar a chave secundária, finalmente, extinguir a chave primária antiga.

Se saiba ou suspeite que uma chave fica comprometida e tem de revogar as chaves, pode voltar a gerar ambas as [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) e [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) de um [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), substituindo-los com chaves novas. Este procedimento invalida todos os tokens assinados com as chaves antigas.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Autenticação da assinatura de acesso partilhada com o Service Bus

Os cenários descritos forma incluem a configuração das regras de autorização, a geração de autorização de cliente e os tokens SAS.

Para um completo amostra de trabalho de uma aplicação de Service Bus que ilustra a autorização de SAS de configuração e utilizações, consulte [autenticação da assinatura de acesso partilhado com o Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Um exemplo relacionado que ilustra a utilização de regras de autorização de SAS configuradas nos espaços de nomes ou tópicos para proteger as subscrições do Service Bus está disponível aqui: [autenticação a utilizar acesso assinatura partilhado (SAS) com as subscrições do Service Bus](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Regras de autorização de acesso partilhado de acesso numa entidade

Com bibliotecas do Service Bus .NET Framework, pode aceder a um [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objeto configurado numa fila do Service Bus ou um tópico através de [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) coleção no correspondente [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) ou [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

O código seguinte mostra como adicionar regras de autorização de uma fila.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Utilize a autorização de assinatura de acesso partilhado

As aplicações que utilizam o SDK .NET do Azure com a bibliotecas .NET de barramento de serviço podem utilizar a autorização de SAS através do [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) classe. O código seguinte ilustra a utilização do fornecedor de tokens para enviar mensagens para uma fila do Service Bus. Alternativa à utilização mostrada aqui, que também pode passar um token emitido anteriormente para o método de fábrica do fornecedor de tokens.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Também pode utilizar o fornecedor de tokens diretamente para emitir tokens para passar para outros clientes. 

Cadeias de ligação podem incluir um nome de regra (*SharedAccessKeyName*) e a chave de regra (*SharedAccessKey*) ou um token emitido anteriormente (*SharedAccessSignature*). Quando os estão presentes na cadeia de ligação transmitida a qualquer construtor ou método de fábrica aceitar uma cadeia de ligação, o fornecedor de tokens SAS é automaticamente criado e preenchido.

Tenha em atenção que, para utilizar a autorização de SAS com reencaminhamentos do Service Bus, pode utilizar as chaves SAS configuradas no espaço de nomes de barramento de serviço. Se criar explicitamente um reencaminhamento no espaço de nomes ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) com um [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) de objeto, pode definir as regras SAS apenas para esse reencaminhamento. Para utilizar a autorização de SAS com subscrições do Service Bus, pode utilizar as chaves SAS configuradas num espaço de nomes de barramento de serviço ou um tópico.

## <a name="use-the-shared-access-signature-at-http-level"></a>Utilize a assinatura de acesso partilhado (a nível HTTP)

Agora que sabe como criar assinaturas de acesso partilhado para todas as entidades de Service Bus, está pronto para executar um HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Lembre-se de que isto funciona para todos os fins. Pode criar SAS para uma fila, tópico ou de subscrição.

Se receber um remetente ou cliente um token SAS, não tem a chave diretamente e não é possível reverter o hash para obtê-lo. Como tal, tem controlo sobre o que pode aceder e como período de tempo. Um mais importante lembrar-se é que se alterar a chave primária na política, as assinaturas de acesso partilhado criado a partir do mesmo são também invalidadas.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Utilize a assinatura de acesso partilhado (nível AMQP)

Na secção anterior, vimos como utilizar o token SAS com um pedido POST de HTTP para enviar dados para o Service Bus. Como souber, pode aceder ao Service Bus com o Advanced mensagem colocação protocolo (AMQP) que é o protocolo preferencial a utilizar por motivos de desempenho, em vários cenários. A utilização de token SAS com AMQP é descrita no documento [AMQP Claim-Based segurança versão 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) que está no trabalho rascunho desde 2013 mas bem suportados pelo Azure hoje.

Antes de começar a enviar dados para o Service Bus, o publicador tem de enviar o token SAS dentro de uma mensagem AMQP para um nó AMQP bem definido com o nome **$cbs** (pode vê-lo como uma fila "especial" utilizada pelo serviço de adquirir e validar os tokens de SAS). O publicador tem de especificar o **ReplyTo** campo dentro da mensagem AMQP; este é o nó no qual o serviço responde ao publicador com o resultado da validação do token (um padrão de pedido/resposta simples entre o publicador e o serviço). Este nó de resposta é criado "no momento," falando sobre "criação dinâmica de nó remoto", conforme descrito pela especificação AMQP 1.0. Após verificar que o token SAS é válido, o fabricante pode avançar e começar a enviar dados para o serviço.

Os passos seguintes mostram como enviar o token SAS com a utilização do protocolo AMQP o [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) biblioteca. Isto é útil se não é possível utilizar o SDK do Service Bus oficial (por exemplo, num WinRT, .net Compact Framework, o .net Micro Framework e Mono) desenvolver no C\#. Obviamente, esta biblioteca é útil para ajudar a compreender como afirmações com base na segurança funciona ao nível do AMQP, conforme mostrado como funciona a nível de HTTP (com um pedido POST de HTTP e o token SAS enviados dentro do cabeçalho "Authorization"). Se não precisar desses conhecimentos profundo sobre AMQP, pode utilizar o SDK oficial do Service Bus com o .net aplicações Framework, que irão fazer por si.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

O `PutCbsToken()` método recebe o *ligação* (instância de classe de ligação AMQP conforme fornecida pelo [biblioteca AMQP .NET Lite](https://github.com/Azure/amqpnetlite)) que representa a ligação de TCP para o serviço e o *sasToken* token de parâmetro que é a SAS para enviar. 

> [!NOTE]
> É importante que a ligação é criada com **mecanismo de autenticação SASL definido como ANONYMOUS** (e não o predefinido simples com o nome de utilizador e palavra-passe utilizada quando não precisa de enviar o token SAS).
> 
> 

Em seguida, o publicador cria duas ligações AMQP para enviar o token SAS e receber a resposta (o resultado da validação de token) do serviço.

A mensagem AMQP contém um conjunto de propriedades e informações mais do que uma simple mensagem. O token SAS é o corpo da mensagem (utilizando o respetivo construtor). O **"ReplyTo"** propriedade está definida como o nome do nó para receber o resultado da validação da ligação de recetor (pode alterar o nome se pretender, e será criado dinamicamente pelo serviço). As últimas três propriedades da aplicação/personalizada são utilizadas pelo serviço para indicar que tipo de operação tem de executar. Tal como descrito pela especificação de rascunho CBS, têm de ser o **nome da operação** ("put-token"), o **tipo de token** (neste caso, um `servicebus.windows.net:sastoken`) e o **"nome" a audiência** ao qual o token aplica-se (a entidade completa).

Depois de enviar o token SAS na ligação de remetente, o publicador tem de ler a resposta da ligação do recetor. A resposta é uma simple mensagem AMQP com uma propriedade de aplicação com o nome **"código de estado"** que pode conter os mesmos valores como um código de estado HTTP.

## <a name="rights-required-for-service-bus-operations"></a>Direitos necessários para operações de Service Bus

A tabela seguinte mostra os direitos de acesso necessários para várias operações de recursos do Service Bus.

| Operação | Afirmação necessária | Âmbito de afirmação |
| --- | --- | --- |
| **Namespace** | | |
| Configurar regra de autorização de um espaço de nomes |Gerir |Qualquer endereço de espaço de nomes |
| **Registo do serviço** | | |
| Enumerar privada políticas |Gerir |Qualquer endereço de espaço de nomes |
| Começar a escutar num espaço de nomes |Escutar |Qualquer endereço de espaço de nomes |
| Enviar mensagens para um serviço de escuta um espaço de nomes |Enviar |Qualquer endereço de espaço de nomes |
| **fila** | | |
| Criar uma fila |Gerir |Qualquer endereço de espaço de nomes |
| Eliminar uma fila |Gerir |Qualquer endereço de fila válida |
| Enumerar as filas |Gerir |$ Recursos/filas |
| Obter a descrição da fila |Gerir |Qualquer endereço de fila válida |
| Configurar a regra de autorização para uma fila |Gerir |Qualquer endereço de fila válida |
| Enviar em para a fila |Enviar |Qualquer endereço de fila válida |
| Receber mensagens a partir de uma fila |Escutar |Qualquer endereço de fila válida |
| Abandonar ou concluir mensagens depois de receber a mensagem no modo de bloqueio peek |Escutar |Qualquer endereço de fila válida |
| Diferir uma mensagem para obtenção posterior |Escutar |Qualquer endereço de fila válida |
| Uma mensagem de mensagens não entregues |Escutar |Qualquer endereço de fila válida |
| Obter o Estado associado a uma sessão de fila de mensagens |Escutar |Qualquer endereço de fila válida |
| Definir o Estado associado uma sessão de fila de mensagens |Escutar |Qualquer endereço de fila válida |
| Agendar uma mensagem para entrega posterior; Por exemplo, [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Escutar | Qualquer endereço de fila válida
| **Tópico** | | |
| Criar um tópico |Gerir |Qualquer endereço de espaço de nomes |
| Eliminar um tópico |Gerir |Qualquer endereço de tópico válido |
| Enumerar tópicos |Gerir |$ Recursos/tópicos |
| Obter a descrição de tópico |Gerir |Qualquer endereço de tópico válido |
| Configurar a regra de autorização para um tópico |Gerir |Qualquer endereço de tópico válido |
| Enviar para o tópico |Enviar |Qualquer endereço de tópico válido |
| **Subscrição** | | |
| Criar uma subscrição |Gerir |Qualquer endereço de espaço de nomes |
| Eliminar subscrição |Gerir |../myTopic/subscriptions/mySubscription |
| Enumerar subscrições |Gerir |../myTopic/subscrições |
| Obter a descrição da subscrição |Gerir |../myTopic/subscriptions/mySubscription |
| Abandonar ou concluir mensagens depois de receber a mensagem no modo de bloqueio peek |Escutar |../myTopic/subscriptions/mySubscription |
| Diferir uma mensagem para obtenção posterior |Escutar |../myTopic/subscriptions/mySubscription |
| Uma mensagem de mensagens não entregues |Escutar |../myTopic/subscriptions/mySubscription |
| Obter o Estado associado a uma sessão de tópico |Escutar |../myTopic/subscriptions/mySubscription |
| Definir o Estado associado uma sessão de tópico |Escutar |../myTopic/subscriptions/mySubscription |
| **Regras** | | |
| Criar uma regra |Gerir |../myTopic/subscriptions/mySubscription |
| Eliminar uma regra |Gerir |../myTopic/subscriptions/mySubscription |
| Enumerar regras |Gerir ou escutar |../myTopic/subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre mensagens do Service Bus, consulte os seguintes tópicos.

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Como utilizar as filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com