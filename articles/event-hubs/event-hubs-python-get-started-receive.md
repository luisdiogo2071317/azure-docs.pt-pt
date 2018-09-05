---
title: Receber eventos dos Hubs de eventos do Azure com Python | Documentos da Microsoft
description: Começar a receber eventos dos Hubs de eventos com o Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: f5388f2de599d94f68a1d24a7d701a2cb4795915
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703730"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Receber eventos dos Hubs de eventos com o Python

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise.

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial descreve como receber eventos de um hub de eventos a partir de um aplicativo escrito em Python. Para enviar eventos, consulte [o artigo de envio correspondente](event-hubs-python-get-started-send.md).

O código neste tutorial é obtido a partir [estes exemplos do GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), que pode examinar para ver toda a aplicação de trabalho, incluindo instruções de importação e declarações de variável. Outros exemplos estão disponíveis na mesma pasta do GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

- Python 3.4 ou posterior.
- Os Hubs de eventos espaço de nomes e o event hub existente. Pode criar estas entidades, seguindo as instruções em [este artigo](event-hubs-create.md). 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Instalar pacote do Python

Para instalar o pacote do Python para os Hubs de eventos, abra um prompt de comando que tem o Python no respetivo caminho e, em seguida, execute este comando: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Criar um script de Python para receber eventos

Em seguida, crie uma aplicação de Python que recebe eventos de um hub de eventos:

1. Abra o seu editor favorito do Python, tal como [Visual Studio Code][Visual Studio Code].
2. Criar um script chamado **recv.py**.
3. Cole o seguinte código no recv.py, substituindo os valores de endereço, utilizador e a chave com os valores que obteve a partir do portal do Azure na secção anterior: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
CONSUMER_GROUP = "$default"
OFFSET = Offset("-1")
PARTITION = "0"

total = 0
last_sn = -1
last_offset = "-1"
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
try:
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
    client.run()
    start_time = time.time()
    for event_data in receiver.receive(timeout=100):
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
        total += 1

    end_time = time.time()
    client.stop()
    run_time = end_time - start_time
    print("Received {} messages in {} seconds".format(total, run_time))

except KeyboardInterrupt:
    pass
finally:
    client.stop()
```

## <a name="receive-events"></a>Receber eventos

Para executar o script, abra um prompt de comando que tem o Python no respetivo caminho e, em seguida, execute este comando:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Passos Seguintes

Para enviar eventos, consulte [o artigo de envio correspondente](event-hubs-python-get-started-send.md).

Visite as páginas seguintes para saber mais sobre os Hubs de eventos:

* [Descrição geral dos Hubs de Eventos][Event Hubs overview]
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
