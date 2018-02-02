---
title: "Publicar o evento personalizado tópico de grelha de eventos do Azure"
description: "Descreve como publicar um evento para um tópico personalizado para a grelha de eventos do Azure"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 43dcdf9ab0fee5f7e61ecdc42aaf40430e272d92
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Publicar tópico personalizado para a grelha de eventos do Azure

Este artigo descreve como publicar um evento para um tópico personalizado. Mostra o formato dos dados post e o evento. O [contrato de nível de serviço (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) só se aplica para publicações que corresponde ao formato esperado.

## <a name="endpoint"></a>Ponto Final

Ao enviar o HTTP POST para um tópico personalizado, utilize o formato do URI: `https://<topic-endpoint>?api-version=2018-01-01`.

Por exemplo, é um URI válido: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Para obter o ponto final para um tópico personalizado com a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Para obter o ponto final para um tópico personalizado com o Azure PowerShell, utilize:

```powershell
(Get-AzureRmEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Cabeçalho

O pedido de incluir um valor de cabeçalho denominado `aeg-sas-key` que contém uma chave de autenticação.

Por exemplo, é um valor de cabeçalho válido `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Para obter a chave para um tópico personalizada com a CLI do Azure, utilize:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

Para obter a chave para um tópico personalizada com o PowerShell, utilize:

```powershell
(Get-AzureRmEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Dados do evento

Tópicos personalizado, os dados de nível superior contém os campos mesmos como eventos definidos pelo recurso padrão. Uma dessas propriedades é uma propriedade de dados que contém as propriedades exclusivas para o tópico personalizado. Como publicador de eventos, determinar as propriedades para esse objeto de dados. Utilize o esquema seguinte:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Para obter uma descrição destas propriedades, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md).

Por exemplo, é um esquema de dados de evento válido:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução ao encaminhamento eventos personalizados, consulte [criar e rota eventos personalizados com a CLI do Azure e eventos grelha](custom-event-quickstart.md) ou [eventos personalizados, criar e rota com o Azure PowerShell e o evento grelha](custom-event-quickstart-powershell.md).
* Para obter mais informações sobre a chave de autenticação, consulte [grelha de eventos de segurança e autenticação](security-authentication.md).
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
