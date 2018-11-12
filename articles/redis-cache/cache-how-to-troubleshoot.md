---
title: Como resolver problemas de Cache de Redis do Azure | Documentos da Microsoft
description: Saiba como resolver problemas comuns com a Cache de Redis do Azure.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: ff2076d678b16f4de421a2634d751d26956a400d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228866"
---
# <a name="how-to-troubleshoot-azure-redis-cache"></a>Como resolver problemas de Cache de Redis do Azure
Este artigo fornece orientações para resolução de problemas seguintes categorias de problemas de Cache de Redis do Azure.

* [Resolução de problemas do lado do cliente](#client-side-troubleshooting) – Esta seção fornece diretrizes sobre identificando e resolvendo problemas causados pela aplicação a ligar à Cache de Redis do Azure.
* [Resolução de problemas do lado do servidor](#server-side-troubleshooting) – Esta seção fornece diretrizes sobre a identificar e resolver problemas causados no lado do servidor de Cache de Redis do Azure.
* [Exceções de tempo limite do stackexchange. redis](#stackexchangeredis-timeout-exceptions) -esta secção fornece informações sobre solução de problemas quando utilizar o cliente stackexchange. redis.

> [!NOTE]
> Vários dos passos de resolução de problemas neste guia incluem instruções para executar comandos da Redis e monitorize várias métricas de desempenho. Para obter mais informações e instruções, consulte os artigos da [informações adicionais](#additional-information) secção.
> 
> 

## <a name="client-side-troubleshooting"></a>Resolução de problemas do lado do cliente
Esta secção aborda a resolução de problemas que ocorrem devido a uma condição na aplicação de cliente.

* [Pressão de memória no cliente](#memory-pressure-on-the-client)
* [Fluxo de tráfego](#burst-of-traffic)
* [Cliente de elevada utilização da CPU](#high-client-cpu-usage)
* [Largura de banda do lado do cliente excedida](#client-side-bandwidth-exceeded)
* [Tamanho grande de solicitação/resposta](#large-requestresponse-size)
* [O que aconteceu aos meus dados no Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Pressão de memória no cliente
#### <a name="problem"></a>Problema
A pressão de memória na máquina cliente leva a todos os tipos de problemas de desempenho que podem atrasar o processamento de dados que lhe foi enviados pela instância do Redis sem demora. Quando chega a pressão de memória, o sistema normalmente tem aos dados da página de memória física, a memória virtual, o que é efetuada no disco. Isso *a falha de página* faz com que o sistema para significativamente mais lentos.

#### <a name="measurement"></a>Medida
1. Monitorizar a utilização de memória na máquina para certificar-se de que não excede a memória disponível. 
2. Monitor de `Page Faults/Sec` contador de desempenho. A maioria dos sistemas tem algumas falhas de página, mesmo durante o funcionamento normal, portanto, atenção para picos no contador de desempenho de falhas nesta página, que correspondem aos tempos limite.

#### <a name="resolution"></a>Resolução
Atualizar o cliente para um cliente maior tamanho VM com mais memória ou se aprofundar nos seus padrões de utilização de memória para reduzir o consumo de memória.

### <a name="burst-of-traffic"></a>Fluxo de tráfego
#### <a name="problem"></a>Problema
Aumentos repentinos de tráfego combinado com pobre `ThreadPool` definições podem resultar em atrasos no processamento de dados já enviados pelo servidor Redis, mas ainda não são consumidos no lado do cliente.

#### <a name="measurement"></a>Medida
Monitor de como sua `ThreadPool` estatísticas mudam ao longo do tempo usando código [assim](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Também pode ver o `TimeoutException` mensagem do stackexchange. redis. Segue-se um exemplo:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

A mensagem anterior, há vários problemas que são interessantes:

1. Tenha em atenção que no `IOCP` secção e a `WORKER` secção tem um `Busy` valor maior que o `Min` valor. Essa diferença significa que seu `ThreadPool` definições precisam de ajuste.
2. Também pode ver `in: 64221`. Este valor indica que 64,211 bytes foram recebidos na camada de soquete do kernel, mas ainda não tiver sido lido pela aplicação (por exemplo, stackexchange. redis). Essa diferença normalmente significa que seu aplicativo não está lendo dados de rede tão rapidamente quanto o servidor é enviar para.

#### <a name="resolution"></a>Resolução
Configurar o seu [definições de ThreadPool](https://gist.github.com/JonCole/e65411214030f0d823cb) para se certificar de que seu pool de threads verticalmente rapidamente em períodos de rajada de cenários.

### <a name="high-client-cpu-usage"></a>Cliente de elevada utilização da CPU
#### <a name="problem"></a>Problema
Elevada utilização da CPU no cliente é uma indicação de que o sistema não é possível acompanhar o trabalho que foi pedido para executar. Esta situação significa que o cliente pode falhar ao processar uma resposta de Redis de forma oportuna, mesmo que o Redis enviada a resposta rapidamente.

#### <a name="measurement"></a>Medida
Monitorize a utilização de CPU ampla de sistema através do Portal do Azure ou através do contador de desempenho associados. Tenha cuidado para não monitorizar *processo* CPU porque um único processo pode ter baixa utilização da CPU ao mesmo tempo que todo o sistema CPU pode ser elevada. Assista a picos de utilização da CPU que correspondem aos tempos limite. Como resultado de CPU elevada, poderá também ver alto `in: XXX` valores na `TimeoutException` mensagens de erro, conforme descrito no [fluxo de tráfego](#burst-of-traffic) secção.

> [!NOTE]
> Stackexchange. redis 1.1.603 e mais tarde inclui a `local-cpu` métrica no `TimeoutException` mensagens de erro. Certifique-se estiver a utilizar a versão mais recente do [pacote NuGet do stackexchange. redis](https://www.nuget.org/packages/StackExchange.Redis/). Existem erros constantemente a ser corrigidos no código para tornar mais robusto para tempos limite, de modo é importante ter a versão mais recente.
> 
> 

#### <a name="resolution"></a>Resolução
Atualize para um tamanho maior de VM com mais capacidade de CPU ou investigar o que está a causar picos de CPU. 

### <a name="client-side-bandwidth-exceeded"></a>Largura de banda do lado do cliente excedida
#### <a name="problem"></a>Problema
Consoante a arquitetura de computadores cliente, podem ter limitações sobre quanta largura de banda de rede tem disponíveis. Se o cliente excede a largura de banda disponível ao sobrecarregar a capacidade de rede, em seguida, dados não foi processados no lado do cliente tão rapidamente quanto o servidor está a enviar. Esta situação pode levar a tempos limite.

#### <a name="measurement"></a>Medida
Monitorizar como alterar a sua utilização de largura de banda ao longo do tempo usando código [assim](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Esse código pode não ser executada com êxito em alguns ambientes com permissões restritas (como web sites do Azure).

#### <a name="resolution"></a>Resolução
Tamanho da VM de cliente de aumentar ou reduzir o consumo de largura de banda de rede.

### <a name="large-requestresponse-size"></a>Tamanho grande de solicitação/resposta
#### <a name="problem"></a>Problema
Uma solicitação/resposta grandes pode provocar tempos limite. Por exemplo, suponha que seu valor de tempo limite configurado no cliente é de 1 segundo. A aplicação solicite duas chaves (por exemplo, "A" e "B") ao mesmo tempo (usando a mesma conexão de rede física). Maioria dos clientes suporta "Pipelining" de pedidos, de forma a que ambos os pedidos "A" e "B" são enviados na conexão para o servidor um após o outro sem aguardar que as respostas. O servidor envia as respostas de volta na mesma ordem. Se a resposta "A" for muito grande, pode comer a maior parte do tempo limite para as solicitações subseqüentes. 

O exemplo seguinte demonstra este cenário. Neste cenário, pedido 'A' e 'B' são enviados rapidamente, o servidor começa a enviar respostas 'A' e 'B' rapidamente, mas devido a tempos de transferência de dados, 'B' ficar preso atrás do pedido e expire, apesar do servidor respondeu rapidamente.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Medida
Este pedido/resposta é difícil de medir. Basicamente, precisa instrumentar o seu código de cliente para controlar os pedidos e respostas grandes. 

#### <a name="resolution"></a>Resolução
1. Redis está otimizado para um grande número de valores de pequenos, em vez de alguns valores grandes. A solução preferencial é dividir os dados para os valores menores relacionados. Consulte o [o que é o intervalo de tamanho do valor ideal para redis? 100 KB é demasiado grande? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) publicar para obter detalhes sobre por que valores menores são recomendadas.
2. Aumente o tamanho da VM (para o cliente e servidor de Cache de Redis), para obter capacidades de largura de banda superior, reduzindo os tempos de transferência de dados de respostas maior. Obter mais largura de banda no apenas o servidor ou apenas no cliente pode não ser suficiente. Medir a utilização de largura de banda e compará-lo com as capacidades do tamanho da VM já existe.
3. Aumentar o número de `ConnectionMultiplexer` os objetos utilize e round robin pedidos através de ligações diferentes.

### <a name="what-happened-to-my-data-in-redis"></a>O que aconteceu aos meus dados no Redis?
#### <a name="problem"></a>Problema
Eu esperava para determinados dados estejam em minha instância de Cache de Redis do Azure, mas ele não parece estar ali.

#### <a name="resolution"></a>Resolução
Ver [o que aconteceu aos meus dados no Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) para encontrar possíveis causas e resoluções.

## <a name="server-side-troubleshooting"></a>Resolução de problemas do lado do servidor
Esta secção aborda a resolução de problemas que ocorrem devido a uma condição no servidor de cache.

* [Pressão de memória no servidor](#memory-pressure-on-the-server)
* [Elevada utilização da CPU / carregamento do servidor](#high-cpu-usage-server-load)
* [Largura de banda do lado servidor excedida](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Pressão de memória no servidor
#### <a name="problem"></a>Problema
A pressão de memória no lado do servidor nos leva a todos os tipos de problemas de desempenho que podem atrasar o processamento de pedidos. Quando chega a pressão de memória, o sistema normalmente tem aos dados da página de memória física, a memória virtual, o que é efetuada no disco. Isso *a falha de página* faz com que o sistema para significativamente mais lentos. Existem várias causas possíveis deste pressão de memória: 

1. Tiver preenchido o cache para a capacidade total com os dados. 
2. Redis está a ver fragmentação de memória elevada - causada frequentemente por armazenamento de objetos grandes (Redis está otimizado para um objetos pequenos - consulte o [o que é o intervalo de tamanho do valor ideal para redis? 100 KB é demasiado grande? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) publicar para obter detalhes). 

#### <a name="measurement"></a>Medida
Redis expõe duas métricas que podem ajudar a identificar este problema. A primeira é `used_memory` e o outro é `used_memory_rss`. [Estas métricas](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) estão disponíveis no Portal do Azure ou através do [Redis informações](http://redis.io/commands/info) comando.

#### <a name="resolution"></a>Resolução
Existem várias alterações possíveis que pode fazer para ajudar a manter a utilização de memória em bom estado de funcionamento:

1. [Configurar uma política de memória](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) e definir as horas de expiração em suas chaves. Esta configuração pode não ser suficiente se tiver a fragmentação.
2. [Configura o valor maxmemory reservados](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) que é grande o suficiente para compensar a fragmentação de memória.
3. Divida os seus objetos em cache grandes em menores objetos relacionados.
4. [Dimensionamento](cache-how-to-scale.md) para um tamanho maior de cache.
5. Se estiver a utilizar um [cache premium com o cluster de Redis ativados](cache-how-to-premium-clustering.md), pode [aumentar o número de partições horizontais](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Elevada utilização da CPU / carregamento do servidor
#### <a name="problem"></a>Problema
Elevada utilização da CPU pode significar que o lado do cliente poderão não conseguir processar uma resposta de Redis de forma oportuna, mesmo que o Redis enviada a resposta rapidamente.

#### <a name="measurement"></a>Medida
Monitorize a utilização de CPU ampla de sistema através do Portal do Azure ou através do contador de desempenho associados. Tenha cuidado para não monitorizar *processo* CPU porque um único processo pode ter baixa utilização da CPU ao mesmo tempo que todo o sistema CPU pode ser elevada. Assista a picos de utilização da CPU que correspondem aos tempos limite.

#### <a name="resolution"></a>Resolução
* Reveja quaisquer conselhos e alertas mencionado na [Assistente de Cache de Redis](cache-configure.md#redis-cache-advisor).
* Reveja também as outras recomendações neste tópico, e [melhores práticas para de Redis do Azure](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f) para ver se tem empregado todas as opções para otimizar ainda mais a sua cache e o seu cliente. 
* Reveja a [desempenho de Cache de Redis do Azure](cache-faq.md#azure-redis-cache-performance) gráficos e veja se é quase os limites superiores do seu escalão atual. Se necessário, [dimensionamento](cache-how-to-scale.md) para um escalão de cache maior com mais capacidade de CPU. Se já estiver a utilizar o escalão Premium, pode querer [aumentar horizontalmente com clustering](cache-how-to-premium-clustering.md)


### <a name="server-side-bandwidth-exceeded"></a>Largura de banda do lado servidor excedida
#### <a name="problem"></a>Problema
Dependendo do tamanho das instâncias de cache, podem ter limitações sobre quanta largura de banda de rede tem disponíveis. Se o servidor excede a largura de banda disponível, dados não são enviados para o cliente o mais rapidamente. Esta situação pode levar a tempos limite.

#### <a name="measurement"></a>Medida
Pode monitorizar o `Cache Read` métrica, o que é a quantidade de dados de leitura da cache em Megabytes por segundo (MB/s) durante o intervalo de relatório especificado. Este valor corresponde à largura de banda de rede utilizada por esta cache. Se pretender configurar alertas para limites de largura de banda de rede do lado do servidor, pode criá-los usando `Cache Read` contador. Comparar as leituras com os valores na [esta tabela](cache-faq.md#cache-performance) para os limites de largura de banda observados para a cache de várias camadas e tamanhos de preços.

#### <a name="resolution"></a>Resolução
Se for consistentemente perto observada largura de banda máxima para o tamanho da cache e o escalão de preço, considere [dimensionamento](cache-how-to-scale.md) para um escalão de preço ou tamanho que tem maior largura de banda de rede, usando os valores na [esta tabela](cache-faq.md#cache-performance)como um guia.

## <a name="stackexchangeredis-timeout-exceptions"></a>Exceções de tempo limite do stackexchange. redis
Stackexchange. redis utiliza uma configuração de definição denominada `synctimeout` para operações síncronas, que têm um valor padrão de 1000 ms. Se uma chamada síncrona não concluir no período de tempo estipulado, o cliente stackexchange. redis emite um erro de tempo limite semelhante ao seguinte exemplo:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Esta mensagem de erro contém métricas que podem ajudar a indicar-lhe a causa e possíveis resolução do problema. A tabela seguinte contém detalhes sobre as métricas de mensagem de erro.

| Métrica de mensagem de erro | Detalhes |
| --- | --- |
| portamento da instalação |No intervalo de tempo passado: 0 comandos foram emitidos |
| Gestor |O Gestor de socket está a efetuar `socket.select`, que significa que pede o sistema operacional para indicar um soquete que tem algo a fazer; basicamente: o leitor não está lendo ativamente da rede porque ele não pensa que há algo a ver |
| fila |Existem 73 total de operações em curso |
| qu |6 das operações em curso estão na fila de mensagens e ainda não foram escritos para a rede de saída |
| Se considerássemos |67 das operações em curso foram enviados para o servidor, mas uma resposta ainda não está disponível. A resposta pode ser `Not yet sent by the server` ou `sent by the server but not yet processed by the client.` |
| QC |0 das operações em curso viu respostas, mas ainda não foi marcada como concluída devido a aguardando o loop de conclusão |
| wR |Existe um escritor Active Directory (ou seja, que não estão a ser ignorados os 6 pedidos não enviados) bytes/activewriters |
| pol. |Não há nenhum leitores Active Directory e zero bytes estão disponíveis para ser continue a ler os bytes/activereaders NIC |

### <a name="steps-to-investigate"></a>Passos para investigar
1. Como melhor prática, certifique-se, estiver a utilizar o seguinte padrão para ligar ao utilizar o cliente stackexchange. redis.

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    Para obter mais informações, consulte [ligar à cache a utilizar o stackexchange. redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Certifique-se de que a sua Cache de Redis do Azure e a aplicação de cliente estão na mesma região no Azure. Por exemplo, poderá ser tirando tempos limite quando a cache está na região E.U.A. leste, mas o cliente está na região E.U.A. oeste e o pedido não é concluído dentro do `synctimeout` intervalo ou pode estar prontificando tempos limite quando faz a depuração do seu computador de desenvolvimento local. 
   
    É altamente recomendável que a cache e o cliente na mesma região do Azure. Se tiver um cenário que inclui as chamadas de entre várias regiões, deve definir os `synctimeout` intervalo para um valor maior do que o intervalo de 1000 ms predefinido, incluindo um `synctimeout` propriedade na cadeia de ligação. O exemplo seguinte mostra um trecho de cadeia de ligação do stackexchange. redis cache com um `synctimeout` de 2000 ms.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Certifique-se estiver a utilizar a versão mais recente do [pacote NuGet do stackexchange. redis](https://www.nuget.org/packages/StackExchange.Redis/). Existem erros constantemente a ser corrigidos no código para tornar mais robusto para tempos limite, de modo é importante ter a versão mais recente.
3. Se existirem pedidos que estão a obter vinculados por limitações de largura de banda no servidor ou cliente, demora mais tempo para os mesmos concluir e, deste modo, provocar tempos limite. Para ver se o tempo limite é devido a largura de banda de rede no servidor, consulte [largura de banda do lado servidor excedida](#server-side-bandwidth-exceeded). Para ver se o tempo limite é devido a largura de banda de rede de cliente, consulte [largura de banda do lado do cliente excedida](#client-side-bandwidth-exceeded).
4. Se CPU vinculadas no servidor ou no cliente?
   
   * Verifique se está obtendo vinculados por CPU no cliente, que pode fazer com que o pedido de não ser processadas no `synctimeout` intervalo, portanto, fazendo com que um tempo limite. Mover para um tamanho maior de cliente ou distribuir a carga pode ajudar a controlar este problema. 
   * Verifique se estiver recebendo a CPU vinculada no servidor através da monitorização do `CPU` [métrica de desempenho da cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Solicitações recebidas enquanto o Redis é vinculada à CPU podem causar esses pedidos para o tempo limite. Para resolver esta condição, pode distribuir a carga entre várias partições horizontais numa cache premium ou Atualize para um tamanho maior ou o escalão de preço. Para obter mais informações, consulte [excedido de largura de banda de lado do servidor](#server-side-bandwidth-exceeded).
5. Existem comandos a demorar muito tempo a processar no servidor? Há muito tempo a executar comandos que estão a demorar muito tempo a processar no servidor de redis pode provocar tempos limite. Alguns exemplos de comandos de execução demorada são `mget` com um grande número de chaves, `keys *` ou mal escrito scripts de lua. Pode ligar à sua instância de Cache de Redis do Azure com o cliente redis-cli ou utilizar o [consola de Redis](cache-configure.md#redis-console) e execute o [SlowLog](http://redis.io/commands/slowlog) comando para ver se existem pedidos demorar mais tempo do que o esperado. Servidor redis e stackexchange. redis estão otimizados para muitas solicitações pequenas em vez de menos solicitações grandes. Dividir os dados em segmentos mais pequenos, pode melhorar as coisas aqui. 
   
    Para obter informações sobre a ligação ao ponto final de SSL de Cache de Redis do Azure com o redis-cli e túnel, consulte a [anunciando ASP.NET sessão do fornecedor de estado para versão de pré-visualização de Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) postagem de blog. Para obter mais informações, consulte [SlowLog](http://redis.io/commands/slowlog).
6. Carga de servidor da Redis elevada pode provocar tempos limite. Pode monitorizar a carga do servidor através da monitorização do `Redis Server Load` [métrica de desempenho da cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Uma carga de servidor de 100 (valor máximo) significa que o servidor redis tem estado ocupado, com nenhum tempo de inatividade, processamento de pedidos. Para ver se determinadas solicitações estão ocupando toda a capacidade de servidor, execute o comando de SlowLog, conforme descrito no parágrafo anterior. Para obter mais informações, consulte [utilização elevada da CPU / servidor carregar](#high-cpu-usage-server-load).
7. Houve qualquer outro evento no lado do cliente que pode ter causado um blip de rede? No cliente (web, função de trabalho ou uma VM Iaas), verifique se Ocorreu um evento como aumentando ou reduzindo, o número de instâncias de cliente ou implementar uma nova versão do cliente ou o dimensionamento automático está ativado? No nosso teste, Achamos que dimensionamento automático ou aumentar verticalmente/para baixo pode causa conectividade de rede de saída podem ser perdida para vários segundos. Código de stackexchange. redis face a tais eventos e volta. Durante este período de restabelecer a ligação, quaisquer pedidos na fila podem tempo limite.
8. Houve um grande pedido anterior vários pequenos pedidos para a Cache de Redis que excedeu o limite? O parâmetro `qs` no erro mensagem indica o número de pedidos foram enviado do cliente para o servidor, mas ainda não processados uma resposta. Este valor pode continuam a aumentar porque stackexchange. redis utiliza uma única ligação de TCP e só pode ler uma resposta ao mesmo tempo. Mesmo que a primeira operação excedeu o limite de tempo, não impede que os dados enviados de/para o servidor e outras solicitações são bloqueadas até que o pedido de grandes estiver concluído, fazendo com que os detalhes de tempo. Uma solução é minimizar a possibilidade de tempos limite, garantindo que a cache é grande o suficiente para a sua carga de trabalho e a divisão de valores grandes em segmentos mais pequenos. Outra solução possível é utilizar um conjunto de `ConnectionMultiplexer` objetos no seu cliente e escolha menos carregado `ConnectionMultiplexer` ao enviar um pedido de novo. Isso deve impedir que um tempo limite único que faz com que outros pedidos para também o tempo limite.
9. Se estiver a utilizar `RedisSessionStateprovider`, certifique-se de que definiu o tempo limite de repetição corretamente. `retrytimeoutInMilliseconds` deve ser superior a `operationTimeoutinMilliseonds`, caso contrário, não existem repetições ocorrem. No exemplo a seguir `retrytimeoutInMilliseconds` está definido como 3000. Para obter mais informações, consulte [fornecedor de estado de sessão do ASP.NET para a Cache de Redis do Azure](cache-aspnet-session-state-provider.md) e [como utilizar os parâmetros de configuração do fornecedor de estado de sessão e o fornecedor de Cache de saída](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Verificar a utilização de memória no servidor de Cache de Redis do Azure por [monitoramento](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` e `Used Memory`. Se uma política de expulsão estiver em vigor, o Redis começa expulsar chaves quando `Used_Memory` atinge o tamanho da cache. O ideal é que `Used Memory RSS` deve ser apenas ligeiramente superior `Used memory`. Uma grande diferença significa que existe a fragmentação de memória (interno ou externo). Quando `Used Memory RSS` é inferior a `Used Memory`, significa que parte da memória cache tem sido ativado pelo sistema operativo. Se essa troca ocorrer, pode esperar algumas latências significativas. Porque o Redis não tem controle sobre como as respetivas alocações são mapeadas para páginas de memória elevadas `Used Memory RSS` costuma ser o resultado de um pico na utilização da memória. Quando o Redis libera memória, a memória é dado de volta para o alocador e o alocador pode, ou pode não oferecer a memória novamente para o sistema. Pode haver uma discrepância entre o `Used Memory` consumo de memória e de valor, conforme comunicado pelo sistema operativo. Pode ser devido ao fato da memória foi usada e lançada pela Redis, mas não especificado de volta ao sistema. Para ajudar a atenuar os problemas de memória, pode executar os seguintes passos:
   
   * Atualize o cache para um tamanho maior para que não estão em execução se Compare a limitações de memória no sistema.
   * Definir as horas de expiração nas chaves para que os valores mais antigos são expulsos de forma proativa.
   * Monitor de `used_memory_rss` métrica da cache. Quando este valor se aproxima o tamanho da respetiva cache, o que é provável que começar a ver problemas de desempenho. Distribua os dados através de vários shards se estiver a utilizar uma cache premium, ou a atualizar para um tamanho maior de cache.
   
   Para obter mais informações, consulte [pressão de memória no servidor](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Informações adicionais
* [Que tamanho e oferta da Cache de Redis devo utilizar? (What Redis Cache offering and size should I use?)](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Como efetuar testes de desempenho e testar o desempenho de minha cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Como posso executar comandos da Redis?](cache-faq.md#how-can-i-run-redis-commands)
* [Como monitorizar a Cache de Redis do Azure](cache-how-to-monitor.md)

