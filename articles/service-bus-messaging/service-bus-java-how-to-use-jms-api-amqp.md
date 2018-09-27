---
title: Como utilizar o AMQP 1.0 com a API de barramento de serviço de Java | Documentos da Microsoft
description: Como utilizar o Java Message Service (JMS) com o Azure Service Bus e o Advanced Message Queuing Protodol (AMQP) 1.0.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2018
ms.author: spelluru
ms.openlocfilehash: a3274053e772cbdf120be15a385c84d5ae37d610
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392657"
---
# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Como utilizar o Java Message Service (JMS) API com do Service Bus e AMQP 1.0
O Advanced Message colocação Protocol (AMQP) 1.0 é um protocolo de mensagens eficiente, fiável e ao nível da transmissão que pode utilizar para criar aplicativos de mensagens robustos e para várias plataformas.

Suporte de AMQP 1.0 no Service Bus significa que pode utilizar a colocação em fila e de publicação/subscrição funcionalidades de mensagens mediadas entre uma variedade de plataformas através de um protocolo binário eficiente. Além disso, pode criar aplicativos compostos por componentes criados usando uma combinação de linguagens, arquiteturas e sistemas operativos.

Este artigo explica como utilizar o Service Bus (filas e tópicos de publicação/subscrição) de recursos de mensagens das aplicações Java com o popular Java Message Service (JMS) API standard. Há uma [artigo complementar](service-bus-amqp-dotnet.md) que explica como fazer o mesmo com a API de .NET do Service Bus. Pode utilizar estes guias de dois em conjunto para saber mais sobre mensagens de várias plataformas com AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Introdução ao Service Bus
Este guia assume que já tem um espaço de nomes do Service Bus que contém uma fila com o nome **queue1**. Se não o fizer, em seguida, pode [criar o espaço de nomes e a fila](service-bus-create-namespace-portal.md) utilizando o [portal do Azure](https://portal.azure.com). Para obter mais informações sobre como criar espaços de nomes do Service Bus e filas, veja [introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Filas e tópicos particionados também suportam AMQP. Para obter mais informações, consulte [entidades de mensagens Particionadas](service-bus-partitioning.md) e [suporte de AMQP 1.0 para o Service Bus filas e tópicos particionados](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>Baixar a biblioteca de cliente AMQP 1.0 JMS
Para obter informações sobre onde pode transferir a versão mais recente da biblioteca de cliente Apache Qpid JMS AMQP 1.0, visite [ https://qpid.apache.org/download.html ](https://qpid.apache.org/download.html).

Tem de adicionar os seguintes ficheiros JAR quatro de arquivo de distribuição Apache Qpid JMS AMQP 1.0 para o caminho da classe de Java quando criar e executar aplicativos de JMS com o Service Bus:

* geronimo jms\_1.1\_spec 1.0.jar
* qpid-amqp-1-0-Client-[Version].JAR
* qpid-amqp-1-0-Client-jms-[Version].JAR
* qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Programar aplicações Java
### <a name="java-naming-and-directory-interface-jndi"></a>A atribuição de nomes de Java e a Interface de diretório (JNDI)
JMS usa a nomenclatura de Java e a Interface de diretório (JNDI) para criar uma separação entre nomes de lógicos e físico. Dois tipos de objetos JMS são resolvidos usando JNDI: ConnectionFactory e de destino. JNDI utiliza um modelo de provedor no qual pode conectar os serviços de diretório diferente para processar tarefas de resolução de nome. Formato do Apache Qpid JMS AMQP 1.0 biblioteca é fornecida com um simples propriedades fornecedor JNDI baseados em ficheiros, que é configurado usando um arquivo de propriedades dos seguintes procedimentos:

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Configurar o ConnectionFactory
A entrada usada para definir um **ConnectionFactory** no arquivo de propriedades Qpid do fornecedor JNDI é o seguinte formato:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Em que **[jndi_name]** e **[ConnectionURL]** possuir o significado seguinte:

* **[jndi_name]** : O nome lógico do ConnectionFactory. Este é o nome que será resolvido no aplicativo Java utilizando o método JNDI IntialContext.lookup().
* **[ConnectionURL]** : Um URL que fornece a biblioteca JMS com as informações necessárias para o mediador AMQP.

O formato do **ConnectionURL** é o seguinte:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Em que **[espaço de nomes]**, **[SASPolicyName]** e **[SASPolicyKey]** possuir o significado seguinte:

* **[espaço de nomes]** : O barramento de serviço espaço de nomes.
* **[SASPolicyName]** : Nome da política de assinatura de acesso partilhado a fila.
* **[SASPolicyKey]** : Chave de política de assinatura de acesso partilhado a fila.

> [!NOTE]
> Deve codificar o URL a palavra-passe manualmente. Um utilitário de codificação do URL útil está disponível em [ http://www.w3schools.com/tags/ref_urlencode.asp ](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Configurar destinos
A entrada usada para definir um destino no fornecedor JNDI do arquivo de propriedades de Qpid é no seguinte formato:

```
queue.[jndi_name] = [physical_name]
```

ou

```
topic.[jndi_name] = [physical_name]
```

Onde **[jndi\_name]** e **[física\_nome]** possuir o significado seguinte:

* **[jndi_name]** : O nome lógico do destino. Este é o nome que será resolvido no aplicativo Java utilizando o método JNDI IntialContext.lookup().
* **[physical_name]** : O nome da entidade do Service Bus para a qual a aplicação envia ou recebe mensagens.

> [!NOTE]
> Se receber de uma subscrição de tópico do Service Bus, o nome físico especificado no JNDI deve ser o nome do tópico. O nome da subscrição é fornecido quando a assinatura durável é criada no código da aplicação de JMS. O [guia para programadores 1.0 AMQP Bus Service](service-bus-amqp-dotnet.md) fornece mais detalhes sobre como trabalhar com tópicos do Service Bus do JMS.
> 
> 

### <a name="write-the-jms-application"></a>Escreva a aplicação de JMS
Não há especial APIs ou opções necessárias ao utilizar JMS com o Service Bus. No entanto, existem algumas restrições que serão abordadas mais tarde. Como com qualquer aplicativo JMS, é a primeira coisa que necessário, configuração do ambiente JNDI, ser capaz de resolver uma **ConnectionFactory** e destinos.

#### <a name="configure-the-jndi-initialcontext"></a>Configurar o InitialContext JNDI
O ambiente de JNDI está configurado, passando uma tabela de hash de informações de configuração para o construtor da classe javax.naming.InitialContext. Os dois necessário elementos da tabela de hash são o nome da classe de fábrica de contexto inicial e o URL do fornecedor. O código seguinte mostra como configurar o ambiente de JNDI para utilizar o arquivo de propriedades baseado em provedor JNDI com um arquivo de propriedades com o nome de Qpid **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Um aplicativo JMS simples com uma fila do Service Bus
O programa de exemplo seguintes envia JMS TextMessages uma fila do Service Bus com o nome lógico JNDI da fila e recebe as mensagens de volta.

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-the-application"></a>Executar a aplicação
Executando o aplicativo produz a saída do formulário:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plataformas de mensagens entre JMS e .NET
Este guia mostrou como enviar e receber mensagens para e do Service Bus utilizando JMS. No entanto, um dos principais benefícios do AMQP 1.0 é que ele permite que os aplicativos criados a partir de componentes escritos em idiomas diferentes, com mensagens trocadas de forma fiável e em total fidelidade.

Usando o exemplo de aplicativo de JMS descrito acima e um aplicativo .NET semelhante obtidas a partir de um artigo complementar, [usando o Service Bus do .NET com o AMQP 1.0](service-bus-amqp-dotnet.md), podem trocar mensagens entre .NET e Java. Leia este artigo para obter mais informações sobre os detalhes de mensagens usando o Service Bus e AMQP 1.0 para várias plataformas.

### <a name="jms-to-net"></a>JMS para .NET
Para demonstrar JMS para .NET de mensagens:

* Inicie a aplicação de exemplo .NET sem nenhum argumento da linha de comandos.
* Inicie o aplicativo de exemplo de Java com o argumento da linha de comandos "sendonly". Neste modo, o aplicativo não irá receber mensagens da fila, só irá enviar.
* Prima **Enter** algumas vezes na consola de aplicação de Java, que fará com que mensagens sejam enviadas.
* Estas mensagens são recebidas pelo aplicativo .NET.

#### <a name="output-from-jms-application"></a>Resultado de aplicação JMS
```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Resultado da aplicação .NET
```
> SimpleSenderReceiver.exe    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET para JMS
Para demonstrar o .NET para JMS de mensagens:

* Inicie a aplicação de exemplo do .NET com o argumento da linha de comandos "sendonly". Neste modo, o aplicativo não irá receber mensagens da fila, só irá enviar.
* Inicie a aplicação de exemplo de Java sem nenhum argumento da linha de comandos.
* Prima **Enter** algumas vezes na consola de aplicação do .NET, que fará com que mensagens sejam enviadas.
* Estas mensagens são recebidas pela aplicação Java.

#### <a name="output-from-net-application"></a>Resultado da aplicação .NET
```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Resultado de aplicação JMS
```
> java SimpleSenderReceiver    
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Funcionalidades não suportadas e restrições
As seguintes restrições existem ao utilizar JMS através do AMQP 1.0 com o Service Bus, ou seja:

* Apenas um **MessageProducer** ou **MessageConsumer** permitidas por **sessão**. Se precisar de criar vários **MessageProducers** ou **MessageConsumers** num aplicativo, criar um dedicado **sessão** para cada uma delas.
* Subscrições de tópicos volátil não são atualmente suportadas.
* **MessageSelectors** não são atualmente suportadas.
* Destinos temporários; Por exemplo, **TemporaryQueue**, **TemporaryTopic** não são atualmente suportadas, juntamente com o **QueueRequestor** e **TopicRequestor**APIs que as utilizam.
* Sessões confiáveis e transações distribuídas não são suportadas.

## <a name="summary"></a>Resumo
Este guia de procedimentos mostrou como utilizar mediadas do Service Bus funcionalidades de mensagens (filas e tópicos de publicação/subscrição) do Java com a API de JMS populares e AMQP 1.0.

Também pode utilizar o Service Bus AMQP 1.0 de outras linguagens, incluindo .NET, C, Python e PHP. Componentes criados com essas linguagens diferentes podem trocar mensagens de forma fiável e em fidelidade total usando o AMQP 1.0 suporte no Service Bus.

## <a name="next-steps"></a>Passos Seguintes
* [Suporte de AMQP 1.0 no Azure Service Bus](service-bus-amqp-overview.md)
* [Como utilizar o AMQP 1.0 com a API .NET do Service Bus](service-bus-dotnet-advanced-message-queuing.md)
* [AMQP 1.0 no Service Bus guia para programadores](service-bus-amqp-dotnet.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Centro de Programadores do Java](https://azure.microsoft.com/develop/java/)

