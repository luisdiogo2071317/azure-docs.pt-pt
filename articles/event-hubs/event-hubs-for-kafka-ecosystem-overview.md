---
title: Os Hubs de eventos do Azure para o Apache Kafka | Documentos da Microsoft
description: Descrição geral e introdução ao Kafka ativados os Hubs de eventos do Azure
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 05/07/2018
ms.author: darosa
ms.openlocfilehash: 51f2ad736ccbf27cafb05b8f68653f5effdecbf0
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503507"
---
# <a name="azure-event-hubs-for-apache-kafka-preview"></a>Hubs de eventos do Azure para o Apache Kafka (pré-visualização)

Os Hubs de eventos fornece um ponto de extremidade do Kafka que pode ser utilizado por já existentes Kafka com base em aplicações como uma alternativa à execução de seu próprio cluster do Kafka. Os Hubs de eventos suportam [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) e mais recentes versões de cliente e funciona com seus aplicativos existentes do Kafka, incluindo o MirrorMaker. Altere a cadeia de ligação e iniciar a transmissão em fluxo de eventos a partir das suas aplicações que utilizam o protocolo de Kafka nos Hubs de eventos.

## <a name="what-does-event-hubs-for-kafka-provide"></a>O que o Hubs de eventos para o Kafka fornece?

Os Hubs de eventos para a funcionalidade de Kafka fornece uma vantagem de protocolo na parte superior dos Hubs de eventos do Azure que é compatível com o binário com versões do Kafka 1.0 e, mais tarde para de leitura e gravação para tópicos do Kafka. Conceitualmente Kafka e os Hubs de eventos são quase idênticos: são os dois registos particionados criados para dados de transmissão em fluxo. A tabela seguinte mapeia conceitos entre Kafka e os Hubs de eventos.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Mapeamento de conceitual Kafka e o Hub de eventos

| Conceito de Kafka | Conceito de Hubs de eventos|
| --- | --- |
| Cluster | Espaço de Nomes |
| Tópico | Hubs de Eventos |
| Partição | Partição|
| Grupo de Consumidores | Grupo de Consumidores |
| Desvio | Desvio|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Principais diferenças entre o Kafka e os Hubs de eventos

Embora [Apache Kafka](https://kafka.apache.org/) é um software, que pode ser executada sempre que escolher, os Event Hubs são um serviço em nuvem semelhante ao armazenamento de Blobs do Azure. Existem não existem servidores ou redes para gerir e não mediadores para configurar. Criar um espaço de nomes, o que é um FQDN no qual seus tópicos em direto, e, em seguida, criar Hubs de eventos ou tópicos dentro desse namespace. Para obter mais informações sobre os Hubs de eventos e espaços de nomes, consulte [o que é o Hubs de eventos](event-hubs-what-is-event-hubs.md). Como um serviço cloud, os Event Hubs utilizam um único endereço IP virtual estável como o ponto final, para que os clientes não precisam saber sobre os corretores ou máquinas de um cluster. 

Escala nos Hubs de eventos é controlada pelas unidades de débito quantos comprar, com cada unidade de débito entitling 1 MB por segundo ou 1000 eventos por segundo de entrada. Por predefinição, os Hubs de eventos verticalmente as unidades de débito quando atingir o limite com o [ampliação automática](event-hubs-auto-inflate.md) funcionalidade; essa funcionalidade também funciona com os Hubs de eventos para a funcionalidade do Kafka. 

### <a name="security-and-authentication"></a>Segurança e autenticação

Os Hubs de eventos do Azure requer SSL ou TLS para todas as comunicações e utiliza assinaturas de acesso partilhado (SAS) para autenticação. Este requisito também é verdadeiro para um ponto de extremidade do Kafka dentro dos Hubs de eventos. Para compatibilidade com o Kafka, os Event Hubs utilizam simples do SASL para autenticação e SSL do SASL para a segurança de transporte. Para obter mais informações sobre a segurança nos Hubs de eventos, consulte [os Hubs de eventos de autenticação e segurança](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Outros recursos de Hubs de eventos disponíveis para o Kafka

Os Hubs de eventos para a funcionalidade de Kafka permite-lhe escrever com um protocolo e ler com outra, para que os produtores de Kafka atuais podem continuar a publicar através do Kafka, e pode adicionar os leitores com os Hubs de eventos, como o Azure Stream Analytics ou as funções do Azure. Além disso, os recursos de Hubs de eventos, como [capturar](event-hubs-capture-overview.md) e [recuperação após desastre geográfico](event-hubs-geo-dr.md) também funcionam com os Hubs de eventos para a funcionalidade do Kafka.

## <a name="features-that-are-not-supported-in-the-preview"></a>Funcionalidades que não são suportadas na pré-visualização

Para a pré-visualização pública dos Hubs de eventos para a integração do Kafka, não são suportadas as seguintes funcionalidades de Kafka:

*   Produtor de Idempotentes
*   Transação
*   Compressão
*   Retenção com base no tamanho
*   Compactação de registo
*   Adicionar partições para um tópico existente
*   Suporte de API do HTTP Kafka
*   Kafka ligar
*   Fluxos do Kafka

## <a name="next-steps"></a>Passos Seguintes

Este artigo fornecido uma introdução aos Hubs de eventos para o Kafka. Para obter mais informações, consulte as seguintes ligações:

* [Como criar Kafka ativado os Hubs de eventos](event-hubs-create-kafka-enabled.md)
* [Stream nos Hubs de eventos a partir das suas aplicações de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

 
 

