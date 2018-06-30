---
title: O que é o anfitrião do processador de eventos Hubs do Azure eventos e porquê utilizá-lo | Microsoft Docs
description: Descrição geral e a introdução para o anfitrião do processador de eventos Hubs do Azure eventos
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: 4907004f4bb88cf0fe9fb799cab236ebf98bba7a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133015"
---
# <a name="azure-event-hubs-event-processor-host-overview"></a>Descrição geral de anfitrião do processador de eventos do Event Hubs do Azure

Os Hubs de eventos do Azure é um serviço de ingestão de telemetria de elevado desempenho que pode ser utilizado para fluxo milhões de eventos de custo reduzido. Este artigo descreve como consumir os eventos transmissões em utilizando o *anfitrião do processador de eventos* (EPH); um agente de consumidor inteligente que simplifica a gestão dos pontos de verificação, leasing e os leitores dos eventos paralela.  

A chave de dimensionamento para os Event Hubs é a ideia dos consumidores particionadas. Contrast para o [consumidores concorrentes](http://msdn.microsoft.com/en-us/library/dn568101.aspx) padrão, o padrão de consumidor particionado permite escala elevada removendo estrangulamento a contenção e facilitar o início paralelismo de ponto a ponto.

## <a name="home-security-scenario"></a>Cenário de segurança inicial

Como um cenário de exemplo, considere uma empresa de segurança inicial que monitoriza 100.000 homes. Cada minuto, este obtém dados de sensores vários como um detector movimento sensor abrir porta/janela, detector de break glass, etc., instalado em cada inicial. A empresa fornece um web site para residentes na monitorizar a atividade da respetiva home em tempo real.

Cada sensor envia dados para um hub de eventos. O hub de eventos está configurado com 16 partições. No final consumo, precisa de um mecanismo que pode ler estes eventos, consolidá-los (filtro, agregação, etc.) e informação do Estado do agregado para um blob de armazenamento, em seguida, é projetado uma página web amigável de utilizador.

## <a name="write-the-consumer-application"></a>Escreva a aplicação de consumidor

Ao conceber o consumidor num ambiente distribuído, o cenário tem de lidar com os seguintes requisitos:

1. **Escala:** criar vários consumidores, com cada consumidor a propriedade de ler a partir do alguns partições de Event Hubs.
2. **O balanceamento de carga:** aumentar ou reduzir os consumidores dinamicamente. Por exemplo, quando é adicionado um novo tipo de sensor (por exemplo, um detector carbono monoxide) para cada home page, aumenta o número de eventos. Nesse caso, o operador (um humanos) aumenta o número de instâncias de consumidor. Em seguida, o conjunto de consumidores pode rebalancear o número de partições possuem, para partilhar a carga com os consumidores adicionados recentemente.
3. **Retomar totalmente integrada em falhas:** se um consumidor (**consumidor A**) falha (por exemplo, a máquina virtual que aloja o consumidor subitamente falhas), e outros consumidores devem ser capazes de recolher as partições pertencentes ao **consumidor A** e continuar. Além disso, o continuação ponto, chamado um *ponto de verificação* ou *desvio*, deve ser o momento exato em que **consumidor A** falhado ou ligeiramente antes que.
4. **Consumir eventos:** enquanto os pontos de três anteriores lidam com a gestão de consumidor, tem de existir código para consumir os eventos e fazer algo útil com a mesma; por exemplo, agregá-lo e carregue-o para o blob storage.

Em vez de criar a sua própria solução para isto, os Event Hubs fornecem esta funcionalidade através do [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface e o [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) classe.

## <a name="ieventprocessor-interface"></a>IEventProcessor interface

Em primeiro lugar, implementar aplicações de consumo de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface, o que tem quatro métodos: [OpenAsync, CloseAsync, ProcessErrorAsync e ProcessEventsAsnyc](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Esta interface contém o código real para consumir os eventos que envia os Event Hubs. O código seguinte mostra uma implementação simple:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
       Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
       return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
       Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
       return Task.CompletedTask;
     }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
       Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
       return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
       foreach (var eventData in messages)
       {
          var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
             Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
       }
       return context.CheckpointAsync();
    }
}
```

Em seguida, instanciar um [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) instância. Consoante a sobrecarga, ao criar o [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) instância no construtor, são utilizados os seguintes parâmetros:

- **nome do anfitrião:** o nome de cada instância de consumidor. Cada instância de **EventProcessorHost** tem de ter um valor exclusivo para esta variável dentro de um grupo de consumidores, pelo que é melhor não rígido code este valor.
- **eventHubPath:** o nome do hub de eventos.
- **consumerGroupName:** os Event Hubs utilizam **$Default** como o nome do grupo de consumidores predefinido, mas é recomendável criar um grupo de consumidores para o aspeto específico do processamento.
- **eventHubConnectionString:** a cadeia de ligação para o hub de eventos, que pode ser obtido a partir do portal do Azure. Esta cadeia de ligação deve ter **escutar** permissões no hub de eventos.
- **storageConnectionString:** a conta de armazenamento utilizada para gestão de recursos internos.

Por fim, os consumidores de registar o [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) instância com o serviço de Event Hubs. Registar dá instruções ao serviço de Event Hubs esperar que a aplicação de consumidor consome eventos a partir de algumas das respetivas partições e para invocar a [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) código implementação sempre que este empurra eventos consumir.

### <a name="example"></a>Exemplo

Por exemplo, imagine que existem 5 máquinas virtuais (VMs) dedicados ao consumir eventos e uma aplicação de consola simples em cada VM, que o consumo real funciona. Cada aplicação de consola, em seguida, cria um [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) instância e regista-o com o serviço de Event Hubs.

Neste cenário de exemplo, vamos supor que o se 16 partições são atribuídas a 5 **EventProcessorHost** instâncias. Alguns **EventProcessorHost** instâncias poderão proprietário alguns mais partições que outros. Para cada partição que um **EventProcessorHost** proprietário de instância, cria uma instância do `SimpleEventProcessor` classe. Por conseguinte, existem 16 instâncias de `SimpleEventProcessor` geral, com um atribuído para cada partição.

A lista seguinte resume neste exemplo:

- 16 partições de event Hubs.
- 5 VMs, 1 consumidor aplicação (por exemplo, Consumer.exe) em cada VM.
- 5 EPH instâncias registadas, 1 em cada VM por Consumer.exe.
- 16 `SimpleEventProcessor` objetos criados por instâncias EPH 5.
- 1 Consumer.exe aplicação poderá conter 4 `SimpleEventProcessor` objetos, uma vez que 1 instância EPH poderá ser 4 partições.

## <a name="partition-ownership-tracking"></a>Propriedade de partição de controlo

Propriedade de uma partição para uma instância EPH (ou um consumidor) é controlada através da conta de armazenamento do Azure que é fornecida para o controlo. Pode visualizar o controlo como uma tabela simple, da seguinte forma. Pode ver a implementação real, examinando os blobs na conta de armazenamento fornecidos:

| **Nome do grupo de consumidores** | **ID de partição** | **Nome de anfitrião (owner)** | **Tempo de adquirir concessão (ou propriedade)** | **O desvio na partição (ponto de verificação)** |
| --- | --- | --- | --- | --- |
| $Predefinição | 0 | Consumidor\_VM3 | 2018-04-15T01:23:45 | 156 |
| $Predefinição | 1 | Consumidor\_VM4 | 2018-04-15T01:22:13 | 734 |
| $Predefinição | 2 | Consumidor\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| $Predefinição | 15 | Consumidor\_VM3 | 2018-04-15T01:22:56 | 976 |

Aqui, cada anfitrião adquire a propriedade de uma partição para um determinado período de tempo (a duração da concessão). Se uma falha de anfitrião (VM posterior é encerrado para baixo), em seguida, expira a concessão. Outros anfitriões tentarem obter a propriedade da partição e um dos anfitriões do é concluída com êxito. Este processo repõe o período de concessão na partição com um novo proprietário. Desta forma, apenas um único leitor numa altura pode ler a partir de qualquer determinada partição dentro de um grupo de consumidores.

## <a name="receive-messages"></a>Receber mensagens

Cada chamada para [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) fornece uma coleção de eventos. É da responsabilidade do cliente para lidar com estes eventos. Recomenda-se que efetue coisas relativamente rápidas; ou seja, fazê-lo como processamento reduzida quanto possível. Em alternativa, utilize grupos de consumidores. Se precisar de escrever para o armazenamento e efetuar algumas encaminhamento, é, geralmente, é melhor utilizar dois grupos de consumidores e ter dois [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementações que executem separadamente.

Numa determinada fase durante o processamento, poderá pretender controlar o que que tenha lido e concluída. Manter controlar é essencial se tem de reiniciar a leitura, pelo que não a voltar ao início da sequência. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) simplifica a este controlo utilizando *pontos de verificação*. Um ponto de verificação é uma localização ou desvio, para uma determinada partição, dentro de um grupo de consumidores fornecido, no qual ponto estiver satisfeito que ter processados as mensagens. Marcar um ponto de verificação no **EventProcessorHost** é conseguido ao chamar o [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) método no [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) objeto. Esta operação é geralmente feita dentro de [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) método, mas também pode ser feito no [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync).

## <a name="checkpointing"></a>Ponto de verificação

O [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) método tem dois sobrecargas: o primeiro, sem parâmetros, os pontos de verificação para o deslocamento mais elevado de eventos dentro da coleção devolvido pelo [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Este desvio é uma marca de "máximo de alta"; pressupõe que tem de processar todos os eventos recentes quando efetua a chamada. Se utilizar este método desta forma, lembre-se de que é esperado que chamá-lo Depois devolveu o código de processamento de eventos. A segunda sobrecarga permite-lhe especificar uma [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) instância para o ponto de verificação. Este método permite-lhe utilizar um tipo diferente de marca d'água para o ponto de verificação. Com esta marca de água, pode implementar um nível de "baixa máximo": o mais baixa eventos sequenciado são determinadas foi processado. Esta sobrecarga é fornecida para permitir flexibilidade na gestão de deslocamento.

Quando o ponto de verificação é executado, é escrito um ficheiro JSON com informações específicas de partição (especificamente, o deslocamento), a conta de armazenamento fornecida no construtor para [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Este ficheiro é continuamente atualizado. É essencial considerar os pontos de verificação no contexto - seria unwise ao ponto de verificação cada mensagem. A conta de armazenamento utilizada para pontos de verificação, provavelmente, não seria processar este carregamento, mas mais acima de tudo, pontos de verificação cada evento individual é indicativa de um padrão de mensagens em fila para o qual uma fila do Service Bus poderá ser uma opção melhor de um hub de eventos. A ideia atrás de Event Hubs é que obtém ", pelo menos, uma vez" entrega em grande escala. Ao tornar o idempotent de sistemas a jusante, é fácil de recuperar de falhas ou reinicia que resultam nos eventos mesmos a ser recebidos várias vezes.

## <a name="thread-safety-and-processor-instances"></a>As instâncias de segurança e de processador de thread

Por predefinição, [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) é thread seguro e funciona de forma síncrona no que respeita à instância do [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor). Quando chegam novos eventos para uma partição, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) é chamado para o **IEventProcessor** instância para que a partição e irá impedir mais chamadas para **ProcessEventsAsync**para a partição. Subsequentes mensagens e chamadas para **ProcessEventsAsync** em fila nos bastidores como bomba a mensagem continua a ser executado em segundo plano em outros threads. Esta segurança de thread remove a necessidade de coleções seguro para thread e aumenta significativamente o desempenho.

## <a name="shut-down-gracefully"></a>Encerrar corretamente

Por fim, [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) permite a um encerramento correto de todos os leitores de partição e sempre deve ser chamado quando a encerrar uma instância de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Falha ao fazê-lo pode provocar atrasos ao iniciar a outras instâncias de **EventProcessorHost** devido à expiração de concessão e conflitos de época. Gestão de época é descrito em detalhe deste [mensagem de blogue](https://blogs.msdn.microsoft.com/gyan/2014/09/02/event-hubs-receiver-epoch/)

## <a name="lease-management"></a>Gestão da concessão

Tal como explicado anteriormente, a tabela de controlo simplifica bastante a natureza o dimensionamento automático [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync). Como uma instância do **EventProcessorHost** é iniciado, adquire concessões tantas quanto possível e começar a ler eventos. Como concessões prestes a expirar, **EventProcessorHost** tenta renová-los por colocar uma reserva. Se a concessão está disponível para renovação, o processador continua a leitura, mas se não for, o leitor está fechado e [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) é chamado. **CloseAsync** é uma boa altura para efetuar quaisquer limpeza final para esse partição.

**EventProcessorHost** inclui um [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) propriedade. Esta propriedade permite controlo sobre a gestão de concessão. Defina estas opções antes de registar o [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementação.

## <a name="control-event-processor-host-options"></a>Opções de controlo de anfitrião do processador de eventos

Além disso, uma sobrecarga da [RegisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) demora um [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) objeto como parâmetro. Utilize este parâmetro para controlar o comportamento da [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) próprio. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) define as propriedades de quatro e um evento:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): O tamanho máximo da coleção de que pretende receber numa invocação de [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Este tamanho não é o mínimo, apenas o tamanho máximo. Se existirem menos mensagens para que sejam recebidas **ProcessEventsAsync** executa com como muitas como estavam disponíveis.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): um valor utilizado pelo canal subjacente AMQP para determinar o limite superior de quantos mensagens o cliente deve receber. Este valor deve ser maior que ou igual a [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize).
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): se este parâmetro for **verdadeiro**, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) é chamado quando a chamada subjacente para receber eventos numa partição exceder o tempo limite. Este método é útil para efetuando baseados no tempo ações durante períodos de inatividade na partição.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): permite que uma função ponteiro ou numa expressão lambda ser definida, que é chamada para fornecer inicial quando um leitor começa a leitura de uma partição de deslocamento. Sem especificar este desvio, o leitor é iniciada à eventos mais antigos, a menos que já foi guardado na conta de armazenamento fornecida para um ficheiro JSON com um desvio igual a [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) construtor. Este método é útil se pretender alterar o comportamento de arranque do leitor. Quando este método é invocado, o parâmetro de objeto contém o ID de partição para que o leitor está a ser iniciado.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): permite-lhe receber a notificação de quaisquer exceções subjacentes que ocorrem no [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Se as coisas não estiverem a funcionar como esperado, este evento é um bom local para começar a procurar.

## <a name="next-steps"></a>Passos Seguintes

Agora que está familiarizado com o anfitrião do processador de eventos, consulte os artigos seguintes para obter mais informações sobre os Event Hubs:

* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Guia de programação dos Event Hubs](event-hubs-programming-guide.md)
* [Disponibilidade e consistência em Hubs de Eventos](event-hubs-availability-and-consistency.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)
* [Exemplos de Hubs de eventos no GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)