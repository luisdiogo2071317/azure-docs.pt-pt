---
title: Os Event Hubs do Azure para ecossistemas Kafka | Microsoft Docs
description: Descrição geral e a introdução ao Kafka ativada Event Hubs do Azure
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 05/07/2018
ms.author: darosa
ms.openlocfilehash: 51e32737be4eaf3c61cdbe50b82a05c205800ac4
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="event-hubs-for-kafka-ecosystems"></a>Hubs de eventos de ecossistemas Kafka

Hubs de eventos de ecossistemas Kafka fornece um ponto final de Kafka que pode ser utilizado por existentes Kafka com base em aplicações como uma alternativa à execução do seu próprio cluster Kafka. Hubs de eventos para Kafka ecossistema suporta [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) e versões de cliente mais recente e funciona com as suas aplicações Kafka existentes, incluindo MirrorMaker. Altere a cadeia de ligação e começar a transmissão em fluxo de eventos de aplicações que utilizam o protocolo de Kafka para os Event Hubs.

## <a name="what-does-event-hubs-for-kafka-ecosystems-provide"></a>O que fornece os Hubs de eventos de ecossistemas Kafka?

Hubs de eventos para ecossistema Kafka fornece um cabeçalho de protocolo por cima do Azure Event Hubs é binária compatível com versões de Kafka 1.0 e mais tarde para ler a partir de e para escrever tópicos Kafka. Concecionais Kafka e Event Hubs são praticamente idênticos: estão ambos os registos particionados incorporados para dados de transmissão em fluxo. A seguinte tabela mapeia conceitos entre Kafka e do Event Hubs.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Hub de eventos e Kafka mapeamento conceptual

| Conceito de Kafka | Conceito de Hubs de eventos|
| --- | --- |
| Cluster | Espaço de Nomes |
| Tópico | Event Hubs |
| Partição | Partição|
| Grupo de Consumidores | Grupo de Consumidores |
| Desvio | Desvio|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Principais diferenças entre Kafka e do Event Hubs

Enquanto [Apache Kafka](https://kafka.apache.org/) é software, o que pode ser executado sempre que escolher, os Event Hubs são um serviço em nuvem semelhante ao Blob Storage do Azure. Existem não existem servidores ou redes para gerir e não os corretores para configurar. Pode criar um espaço de nomes, que é um FQDN no qual os tópicos em direto, e, em seguida, cria Hubs de eventos ou tópicos dentro desse espaço de nomes. Para obter mais informações sobre espaços de nomes e Hubs de eventos, consulte [novidades dos Event Hubs](event-hubs-what-is-event-hubs.md). Como um serviço em nuvem, os Event Hubs utilizam um único endereço IP virtual estável como o ponto final, para que os clientes não é necessário saber sobre o mediadores ou máquinas de um cluster. 

Escala em Event Hubs é controlada pelas unidades de débito quantos comprar, com cada unidade de débito entitling é 1 MB por segundo ou 1000 eventos por segundo de entrada. Por predefinição, os Event Hubs ajusta-se as unidades de débito quando atingir o limite com o [Auto-Inflate](event-hubs-auto-inflate.md) funcionalidade; esta funcionalidade também funciona com os Event Hubs para ecossistemas Kafka. 

### <a name="security-and-authentication"></a>Segurança e autenticação

Os Hubs de eventos do Azure necessita de SSL ou TLS para todas as comunicações e utiliza assinaturas de acesso partilhado (SAS) para autenticação. Este requisito também se verifica para um ponto final de Kafka dentro dos Event Hubs. Para compatibilidade com Kafka, os Event Hubs utilizam SASL simples para autenticação e SASL SSL para a segurança de transporte. Para obter mais informações sobre a segurança no Event Hubs, consulte [autenticação dos Event Hubs e segurança](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Outras funcionalidades de Event Hubs disponíveis para Kafka

Hubs de eventos para ecossistema Kafka permite-lhe escrever com um protocolo e ler com outra, para que os produtores Kafka atuais podem continuar a publicação através de Kafka e pode adicionar os leitores de Event hubs, tais como o Azure Stream Analytics ou as funções do Azure. Além disso, as funcionalidades de Event Hubs, tais como [capturar](event-hubs-capture-overview.md) e [recuperação de desastres Georreplicação](event-hubs-geo-dr.md) também funcionar com os Event Hubs para ecossistemas Kafka.

## <a name="features-that-are-not-supported-in-the-preview"></a>Funcionalidades que não são suportadas na pré-visualização

Para a pré-visualização pública dos Event Hubs, para a integração de ecossistemas Kafka, não são suportadas as seguintes funcionalidades de Kafka:

*   Idempotent produtor
*   Transação
*   Compressão
*   Retenção baseados no tamanho
*   Compactação do registo
*   A adição de partições para um tópico existente
*   Suporte de HTTP Kafka API
*   Kafka ligar
*   Fluxos Kafka

## <a name="next-steps"></a>Passos Seguintes

Este artigo fornecer uma introdução ao Event Hubs Kafka ecossistemas. Para obter mais informações, consulte as seguintes ligações:

* [Como criar Kafka ativada Event Hubs](event-hubs-create-kafka-enabled.md)
* [Fluxo para os Event Hubs das suas aplicações Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

 
 

