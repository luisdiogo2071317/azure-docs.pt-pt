---
title: Enviar eventos para Hubs de eventos do Azure com Python | Documentos da Microsoft
description: Introdução ao envio de eventos para Hubs de eventos com o Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 762e21cfc7d16b614eb637c569f8bfc5b6115db1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703796"
---
# <a name="send-events-to-event-hubs-using-python"></a>Enviar eventos para Hubs de eventos com Python

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise.

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em Python. Para receber eventos, consulte [o artigo correspondente na receção](event-hubs-python-get-started-receive.md).

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

## <a name="create-a-python-script-to-send-events"></a>Criar um script de Python para enviar eventos

Em seguida, crie uma aplicação de Python para enviar eventos para um hub de eventos:

1. Abra o seu editor favorito do Python, tal como [Visual Studio Code][Visual Studio Code].
2. Criar um script chamado **send.py**. Este script para enviar eventos de 100 para o hub de eventos.
3. Cole o seguinte código no send.py, substituindo os valores de endereço, utilizador e a chave com os valores que obteve a partir do portal do Azure na secção anterior: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

## <a name="send-events"></a>Enviar eventos

Para executar o script, abra um prompt de comando que tem o Python no respetivo caminho e, em seguida, execute este comando:

```bash
start python send.py
```
 
## <a name="next-steps"></a>Passos Seguintes

Agora que enviou eventos para um hub de eventos com o Python, para receber eventos veja [o artigo correspondente na receção](event-hubs-python-get-started-receive.md).

Visite as páginas seguintes para saber mais sobre os Hubs de eventos:

* [Descrição geral dos Hubs de Eventos][Event Hubs overview]
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
