---
title: O que são os Hubs de Eventos do Azure? -um serviço de ingestão de grandes volumes de dados | Documentos da Microsoft
description: Saiba mais sobre Hubs de Eventos do Azure, um serviço de fluxo de Macrodados que ingere milhões de eventos por segundo.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: overview
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 43f8af4f2afc0f7210e49dc5b483a8d2757e6aba
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215359"
---
# <a name="azure-event-hubs--a-big-data-streaming-platform-and-event-ingestion-service"></a>Os Hubs de eventos do Azure — Um grandes volumes de dados como serviço de ingestão de eventos e plataforma de transmissão em fluxo
Os Hubs de eventos do Azure é um serviço de ingestão de eventos e plataforma de transmissão em fluxo grandes quantidades de dados. Ele pode receber e processar milhões de eventos por segundo. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches.

Os cenários seguintes são alguns dos cenários em que pode utilizar os Hubs de eventos:

- Deteção de anomalias (fraude/valores atípicos)
- Registo de aplicação
- Pipelines de análise, por exemplo, clickstreams
- Dashboarding em direto
- Arquivo de dados
- Processamento de transações
- Processamento de telemetria do utilizador
- Transmissão em fluxo de telemetria do dispositivo 

## <a name="why-use-event-hubs"></a>Por quê utilizar os Hubs de Eventos?

Os dados são valiosos apenas quando existe uma forma fácil de processar e obter informações atempadas de origens de dados. Os Hubs de eventos fornece uma plataforma de processamento de fluxo distribuída, com baixa latência e a integração totalmente integrada, com os serviços de dados e análise dentro e fora do Azure para criar o seu pipeline de grandes volumes de dados completa.

Os Hubs de Eventos representam a "porta de entrada" para um pipeline de eventos, denominado frequentemente *ingestor de eventos* em arquiteturas de solução. Um ingestor de eventos é um componente ou serviço que se encontra entre os publicadores de eventos e os consumidores de eventos para desacoplar a produção de uma transmissão de eventos do consumo desses eventos. Os Event Hubs fornecem uma plataforma unificada de transmissão em fluxo com a memória intermédia de retenção de tempo, desacoplamento produtores de eventos dos consumidores de eventos. 

As secções seguintes descrevem as principais funcionalidades do serviço Hubs de Eventos do Azure: 

## <a name="fully-managed-paas"></a>PaaS totalmente gerido 

Os Event Hubs são um totalmente gerido plataforma-como-um-serviço (PaaS) com pouca sobrecarga de gestão ou de configuração, para que se concentre em suas soluções empresariais. [Os Hubs de Eventos para ecossistemas do Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) oferecem-lhe experiência de PaaS Kafka sem ter de gerir, configurar ou executar os seus clusters.

## <a name="support-for-real-time-and-batch-processing"></a>Suporte para processamento em tempo real e em lotes

Ingira, coloque em memória intermédia, armazene e processe a sua transmissão em fluxo em tempo real para obter informações acionáveis. Os Event Hubs utilizam uma [modelo de consumidor particionado](event-hubs-features.md#partitions), permitindo que vários aplicativos para o fluxo de processo em simultâneo e permitindo que controle a velocidade de processamento.

[Capturar](event-hubs-capture-overview.md) seus dados em tempo quase real num [armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/) ou [armazenamento do Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) para processamento de retenção ou micro-lotes longo prazo. Pode conseguir este comportamento no mesmo fluxo que utiliza para efetuar a derivação de análise em tempo real. Configurar a captura de dados de eventos é rápida. Existem sem os custos administrativos para executá-lo e é escalado automaticamente com os Hubs de eventos [unidades de débito](event-hubs-features.md#throughput-units). Os Hubs de eventos permite-lhe concentrar-se no processamento de dados em vez de captura de dados.

Os Hubs de Eventos do Azure também se integram nas [Funções do Azure](/azure/azure-functions/) para uma arquitetura sem servidor.

## <a name="scalable"></a>Escalável 

Com os Hubs de Eventos, pode começar com fluxos de dados em megabytes e aumentar para gigabytes ou terabytes. A funcionalidade [Ampliação automática](event-hubs-auto-inflate.md) é uma das muitas opções disponíveis para dimensionar o número de unidades de débito para satisfazer as suas necessidades de utilização. 

## <a name="rich-ecosystem"></a>Ecossistema avançado

[Os Hubs de eventos para o ecossistema do Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) permite [Apache Kafka (1.0 e posterior)](https://kafka.apache.org/) clientes e aplicações para comunicar com os Hubs de eventos. Não é necessário configurar, configurar e gerir os seus próprios clusters do Kafka.
 
Com um ecossistema abrangente disponível em várias [linguagens (.NET, Java, Python, Go, Node.js)](https://github.com/Azure/azure-event-hubs), pode começar facilmente a processar transmissões em fluxo dos Hubs de Eventos. Todas as linguagens de cliente suportadas oferecem integração de nível baixo. O ecossistema também fornece integração total com os serviços do Azure como o Azure Stream Analytics e funções do Azure e, portanto, permite-lhe criar arquiteturas sem servidor.

## <a name="key-architecture-components"></a>Principais componentes da arquitetura
Os Hubs de Eventos contêm os seguintes [componentes principais](event-hubs-features.md):

- **Os produtores de eventos**: Qualquer entidade que envie dados para um hub de eventos. Os publicadores de eventos podem publicar eventos através de HTTPS ou AMQP 1.0 ou Apache Kafka (1.0 ou superior)
- **Partições**: Cada consumidor só lê um subconjunto específico, ou partição, do fluxo de mensagens.
- **Grupos de consumidores**: Uma vista (estado, posição ou desvio) de um hub de eventos completo. Consumidor grupos ativar aplicações de consumo e cada um tem uma vista separada do fluxo de eventos. Eles ler o fluxo de forma independente em seu próprio ritmo e com seus próprios desvios.
- **Unidades de débito**: Previamente adquiridas unidades de capacidade que controlam a capacidade de débito dos Hubs de eventos.
- **Receptores de eventos**: Qualquer entidade que leia os dados de eventos de um hub de eventos. Ligue todos os consumidores de Hubs de eventos através da sessão AMQP 1.0. O serviço de Hubs de eventos fornece eventos através de uma sessão à medida que ficam disponíveis. Todos os consumidores de Kafka ligam através do protocolo de Kafka 1.0 e posterior.

A imagem seguinte mostra a arquitetura do processamento de fluxos dos Hubs de Eventos:

![Hubs de Eventos](./media/event-hubs-about/event_hubs_architecture.png)


## <a name="next-steps"></a>Passos Seguintes

Para começar a utilizar os Hubs de Eventos, veja os seguintes artigos:

1. **Criar um hub de eventos**: [Portal do Azure](event-hubs-create.md), [CLI do Azure](event-hubs-quickstart-cli.md), [Azure PowerShell](event-hubs-quickstart-powershell.md), [modelo Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md)
2. **Enviar eventos para um hub de eventos**: [.NET Core](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [denode.js](event-hubs-node-get-started-send.md), [Vá](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
3. **Receber eventos de um hub de eventos**: [.NET Core](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [node. js ](event-hubs-node-get-started-receive.md), [Vá](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)   

Para saber mais sobre os Hub de Eventos, veja os artigos seguintes:

- [Descrição geral das funcionalidades dos Hubs de Eventos](event-hubs-features.md)
- [Perguntas mais frequentes](event-hubs-faq.md).


