---
title: Diagnosticar e resolver problemas relacionados com o SDK de Async Java do Azure Cosmos DB | Documentos da Microsoft
description: Utilizar funcionalidades como o registo do lado do cliente, e outras ferramentas de terceiros para identificar, diagnosticar e resolver problemas do Azure Cosmos DB.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: ee92a5dd474cdf4f32ed2c7327d732a2cfbbbf79
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632939"
---
# <a name="troubleshooting-issues-when-using-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Resolução de problemas ao utilizar o SDK do Java assíncrono com contas do Azure Cosmos DB SQL API
Este artigo aborda problemas, soluções alternativas, passos de diagnóstico e ferramentas comuns ao utilizar [ADK de Async Java](sql-api-sdk-async-java.md) com contas de API de SQL do Azure Cosmos DB.
SDK de Async Java fornece uma representação lógica do lado do cliente para acessar a API de SQL do Azure Cosmos DB. Este artigo descreve as ferramentas e abordagens para ajudá-lo caso se depare com quaisquer problemas.

Começar com esta lista:
    * Dê uma olhada a [problemas comuns e soluções alternativas] secção deste artigo.
    * É o nosso SDK [aberto no github](https://github.com/Azure/azure-cosmosdb-java) e temos [emite secção](https://github.com/Azure/azure-cosmosdb-java/issues) que iremos monitorizar ativamente. Verifique se encontrar algum problema semelhante com uma solução alternativa já o arquivou.
    * Revisão [sugestões de desempenho](performance-tips-async-java.md) e siga as práticas sugeridas.
    * Siga o resto deste artigo, se não encontrar uma solução, envie um [problema do GitHub](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Problemas comuns e soluções alternativas

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Problemas, de rede Netty ler falha de tempo limite, baixo débito, latência elevada

#### <a name="general-suggestions"></a>Sugestões gerais
* Certificar-se de que a aplicação está em execução na mesma região que a sua conta do Cosmos DB. 
* Verifique a utilização da CPU no anfitrião onde a aplicação está em execução. Se a utilização da CPU é 90% ou mais, considere executar a aplicação num anfitrião com configuração superior ou distribuir a carga em máquinas mais.

#### <a name="connection-throttling"></a>Limitação de conexão
Limitação de conexão pode acontecer devido a uma [Limite de ligação na máquina de anfitrião], ou [Exaustão de porta de SNAT (PAT) do Azure]:

##### <a name="connection-limit-on-host"></a>Limite de ligação na máquina de anfitrião
Alguns sistemas Linux (como 'Red Hat') tem um limite superior no número total de ficheiros abertos. Sockets no Linux são implementados como arquivos, para que este número limita o número total de ligações demasiado.
Execute o seguinte comando:

```bash
ulimit -a
```
O número de abrir os arquivos ("nofile") tem de ser suficientemente grande (no mínimo, como aspas como o tamanho do conjunto de ligação). Leia mais detalhes nas [sugestões de desempenho](performance-tips-async-java.md).

##### <a name="snat"></a>Exaustão de porta de SNAT (PAT) do Azure

Se a aplicação é implementada numa VM do Azure sem um endereço IP público, por predefinição [portas de SNAT de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) são utilizadas para estabelecer ligações para qualquer ponto de extremidade fora da sua VM. O número de ligações permitido da VM para o ponto de final do Cosmos DB é limitado pelos [configuração do Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

As portas de SNAT do Azure são utilizadas apenas quando a sua VM do Azure tem um endereço IP privado e um processo da VM tenta estabelecer uma ligação para um endereço IP público. Portanto, há duas soluções alternativas para evitar a limitação de SNAT do Azure:
    * Adicionar o ponto de final de serviço do Azure Cosmos DB para a sub-rede de VNET de VM do Azure, conforme explicado na [ponto final de serviço de VNET ativar](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). Quando o ponto final de serviço é ativado, os pedidos já não são enviados de um IP público ao cosmos DB em vez disso, a VNET e identidade do sub-rede é enviada. Esta alteração poderá resultar em quedas de firewall se só são permitidos IPs públicos. Se estiver a utilizar a firewall, quando ativar o ponto final de serviço, adicionar a sub-rede para através da firewall [ACLs de VNET](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
    * Atribua um IP público à VM do Azure.

#### <a name="http-proxy"></a>Proxy de HTTP

Se usar um HttpProxy, certificar-se de que sua HttpProxy pode suportar o número de ligações configuradas no SDK `ConnectionPolicy`.
Caso contrário, o que enfrenta problemas de ligação.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Padrão de codificação de inválido: a bloquear o thread de e/s de Netty

O SDK utiliza [Netty](https://netty.io/) biblioteca de e/s para comunicar com o serviço do Azure Cosmos DB. Temos a API de Async e podemos usar APIs de e/s de netty de sem bloqueio. Trabalho de e/s do SDK é executado em threads netty de e/s. O número de threads de e/s netty está configurado para ser o mesmo que o número de núcleos de CPU da máquina de aplicação. Os threads de e/s netty apenas se destinam a ser utilizado para o trabalho de e/s netty sem bloqueio. O SDK devolve o resultado da invocação de API em um dos threads de e/s netty ao código das aplicações. Se a aplicação depois de receber os resultados no netty thread realiza uma operação de longa duração no thread netty, que pode resultar em SDK não ter um número suficiente de threads de e/s para executar seu trabalho de e/s interno. Esse tipo de codificação de aplicação pode resultar em baixo débito, latência alta, e `io.netty.handler.timeout.ReadTimeoutException` falhas. A solução é o thread de alternar quando sabe que a operação irá demorar tempo.

   Por exemplo, o fragmento de código seguinte mostra que se realizar o trabalho de longa duração, o que demora mais do que alguns milissegundos, no netty thread, eventualmente, pode chegar a um Estado em que nenhum thread de e/s netty está presente para processar trabalho de e/s e assim obtém ReadTimeou tException:
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // time consuming work. For example:
                        // writing to a file, computationally heavy work, or just sleep
                        // basically anything which takes more than a few milliseconds
                        // doing such operation on the IO netty thread
                        // without a proper scheduler, will cause problems.
                        // The subscriber will get ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //will be io.netty.handler.timeout.ReadTimeoutException
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   A solução alternativa é alterar o thread no qual executar o trabalho de anotações do tempo. Defina uma instância singleton do agendador para a sua aplicação:
   ```java
// have a singleton instance of executor and scheduler
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Sempre que precisar de tempo e tirar trabalho (por exemplo, trabalho computacionalmente pesado, bloquear e/s), mude o thread para uma função de trabalho fornecida pelo seu `customScheduler` usando `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // switches the thread.
        .subscribe(
            // ...
        );
```
Ao utilizar `observeOn(customScheduler)`, o thread de e/s netty de versão e mude para o seu próprio thread personalizado fornecido pelo customScheduler. Essa modificação irá resolver o problema e não obterá `io.netty.handler.timeout.ReadTimeoutException` deixa de poder falha.

### <a name="connection-pool-exhausted-issue"></a>Conjunto de ligação esgotado o problema

`PoolExhaustedException` é uma falha do lado do cliente. Se obtiver, muitas vezes, esta falha, que é a indicação de que sua carga de trabalho de aplicação é maior do que o conjunto de ligações de SDK que possa servir. Pode ajudar a aumentar o tamanho do conjunto de ligação ou distribuir a carga em várias aplicações.

### <a name="request-rate-too-large"></a>Taxa de pedido demasiado grande
Esta falha é uma falha de servidor indicando que consumidos débito aprovisionado e deve ser repetida mais tarde. Se obtiver, muitas vezes, esta falha, considere aumentar o débito da coleção.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Falha ao ligar ao emulador do Azure Cosmos DB

Certificado HTTPS de emulador do cosmos DB é autoassinado. Para o SDK trabalhar com o emulador, deve importar o certificado de emulador para TrustStore de Java. Conforme explicado [aqui](local-emulator-export-ssl-certificates.md).


## <a name="enable-client-sice-logging"></a>Ativar o registo do SDK de cliente

As utilizações de SDK do Java Async SLF4j como a fachada de registo que suporta o início de sessão em arquiteturas de registo populares, como log4j e logback.

Por exemplo, se pretender utilizar log4j como a arquitetura de registo, adicione as seguintes bibliotecas no caminho da sua classe Java:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Também adicione uma configuração log4j:
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Revisão [sfl4j registo manual](https://www.slf4j.org/manual.html) para obter mais informações.

## <a name="netstats"></a>Estatísticas de rede do SO
Execute o comando netstat para ter uma ideia de quantos ligações estão na `Established` Estado, `CLOSE_WAIT` de estado, etc.

No Linux pode executar o seguinte comando:
```bash
netstat -nap
```
Filtre o resultado para apenas as ligações para o ponto final do Cosmos DB.

Aparentemente, o número de ligações para o ponto final do Cosmos DB no `Established` Estado deve não ser maior que o tamanho do conjunto de ligação configurada.

Se existirem várias ligações para o ponto final do Cosmos DB no `CLOSE_WAIT` de estado, para o exemplo mais do que 1000 ligações, essa é uma indicação de ligações são estabelecidas e destruídas rapidamente, que pode causar potencialmente problemas. Revisão [problemas comuns e soluções alternativas] secção para obter mais detalhes.

 <!--Anchors-->
[Problemas comuns e soluções alternativas]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Limite de ligação na máquina de anfitrião]: #connection-limit-on-host
[Exaustão de porta de SNAT (PAT) do Azure]: #snat


