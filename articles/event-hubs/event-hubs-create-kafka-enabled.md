---
title: Criar Kafka ativada Event Hubs do Azure | Documentos da Microsoft
description: Criar um Kafka ativado o espaço de nomes de Hubs de eventos do Azure com o portal do Azure
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 7a05a89fdf99efb6470ee9c8695f349cf22b8ebb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442132"
---
# <a name="create-kafka-enabled-event-hubs"></a>Criar Kafka ativado os hubs de eventos

Os Hubs de eventos do Azure é uma transmissão em fluxo de plataforma como serviço (PaaS) que ingere milhões de eventos por segundo e oferece baixa latência e alto débito para análise em tempo real e visualização de grandes volumes de dados.

Os Hubs de eventos do Azure para ecossistemas de Kafka fornece um ponto de extremidade. Este ponto final permite que o espaço de nomes de Hubs de eventos compreender de forma nativa [Apache Kafka](https://kafka.apache.org/intro) protocolo e APIs de mensagens. Com esta capacidade, pode comunicar com os hubs de eventos, tal como faria com tópicos do Kafka sem alterar os seus clientes de protocolo ou os seus próprios clusters em execução. Os Hubs de eventos para suporta de ecossistemas de Kafka [versões do Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) e mais tarde.

Este artigo descreve como criar um espaço de nomes de Hubs de eventos e obter a cadeia de ligação necessária para ligar aplicações de Kafka com os hubs de eventos de Kafka-ativado.

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Criar um Kafka ativado o espaço de nomes de Hubs de eventos

1. Inicie sessão para o [portal do Azure][Azure portal]e clique em **criar um recurso** na parte superior esquerda do ecrã.

2. Procure os Hubs de eventos e selecione as opções mostradas aqui:
    
    ![Procure Hubs de eventos no portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Forneça um nome exclusivo e ativar o Kafka no espaço de nomes. Clique em **Criar**.
    
    ![Criar um espaço de nomes](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Depois de criar o espaço de nomes, diante a **definições** separador clique **políticas de acesso partilhado** para obter a cadeia de ligação.

    ![Clique em políticas de acesso partilhado](./media/event-hubs-create/create-event-hub7.png)

5. Pode escolher a predefinição **RootManageSharedAccessKey**, ou adicionar uma nova política. Clique no nome de política e copie a cadeia de ligação. 
    
    ![Selecione uma política](./media/event-hubs-create/create-event-hub8.png)
 
6. Adicione esta cadeia de ligação à sua configuração de aplicação do Kafka.

Agora pode transmitir eventos a partir das suas aplicações que utilizam o protocolo de Kafka nos Hubs de eventos.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os Hubs de eventos, visite estas ligações:

* [Stream nos Hubs de eventos a partir das suas aplicações de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Saiba mais sobre os Hubs de eventos do ecossistema do Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
