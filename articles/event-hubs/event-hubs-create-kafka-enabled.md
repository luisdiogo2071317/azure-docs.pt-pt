---
title: Criar Kafka ativada Event Hubs do Azure | Microsoft Docs
description: Criar um Kafka ativada espaço de nomes de Event Hubs do Azure no portal do Azure
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 4f1d21be3c19dfbc764485fea47b6d4cb2171b3c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942551"
---
# <a name="create-kafka-enabled-event-hubs"></a>Criar Kafka ativada Event Hubs

Os Hubs de eventos do Azure é um grande transmissão em fluxo plataforma como serviço (PaaS) que ingere milhões de eventos por segundo e fornece baixa latência e débito elevado para análise em tempo real e visualização de dados.

Os Event Hubs do Azure para ecossistemas Kafka fornece-lhe um ponto final. Este ponto final permite que o espaço de nomes de Event Hubs nativamente compreender [Apache Kafka](https://kafka.apache.org/intro) protocolo e APIs de mensagens. Com esta capacidade, pode comunicar com os Hubs de eventos, tal como faria com tópicos Kafka sem alterar os clientes de protocolo ou os seus próprios clusters em execução. Hubs de eventos para Kafka ecossistemas suporta [versões Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) e mais tarde.

Este artigo descreve como criar um espaço de nomes de Event Hubs e obter a ligação cadeia necessária para ligar aplicações Kafka Kafka ativada Event Hubs.

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie um [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Criar um Kafka ativada espaço de nomes de Event Hubs

1. Inicie sessão no [portal do Azure][Azure portal]e clique em **crie um recurso** na parte superior esquerda do ecrã.

2. Procure os Event Hubs e selecione as opções apresentadas aqui:
    
    ![Pesquisa para os Event Hubs no portal do](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. **Criar espaço de nomes**, ao fornecer um nome exclusivo e ativar Kafka no espaço de nomes. Clique em **Criar**.
    
    ![Criar um espaço de nomes](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Quando o espaço de nomes for criado, no **definições** separador, clique em **políticas de acesso partilhado** para obter a cadeia de ligação.

    ![Clique em políticas de acesso partilhado](./media/event-hubs-create/create-event-hub7.png)

5. Pode escolher a predefinição **RootManageSharedAccessKey**, ou adicione uma nova política. Clique no nome da política e copie a cadeia de ligação. 
    
    ![Selecione uma política](./media/event-hubs-create/create-event-hub8.png)
 
6. Adicione esta cadeia de ligação à sua configuração de aplicação Kafka.

Agora pode transmitir eventos das suas aplicações que utilizam o protocolo de Kafka para os Event Hubs.

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre os Event Hubs, visite estas ligações:

* [Fluxo para os Event Hubs das suas aplicações Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Saiba mais sobre os Event Hubs do ecossistema de Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Saiba mais sobre os Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
