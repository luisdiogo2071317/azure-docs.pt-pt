---
title: Como gerir grandes conjuntos de tópicos do Azure Event Grid e publicação de eventos-las utilizando domínios de eventos
description: Mostra como criar e gerir os tópicos do Azure Event Grid e publicação de eventos-las utilizando domínios de evento.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 48a5356b03e38e864ba76f048febdb0b040893f5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634312"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Tópicos de gerir e publicar eventos através de domínios de eventos

Este artigo mostra como:

* Criar um domínio de grelha de eventos
* Subscrever tópicos
* Listar chaves
* Publicar eventos para um domínio

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Criar um domínio de eventos

Criação de um domínio de evento pode ser feito através da `eventgrid` extensão para o [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Depois de criar um domínio, pode usá-lo para gerir grandes conjuntos de tópicos.

```azurecli-interactive
# if you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name>
  -l <location>
```

A criação com êxito irá devolver o seguinte:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Tenha em atenção a `endpoint` e `id` como serão solicitados para gerir o domínio e publicar eventos.

## <a name="create-topics-and-subscriptions"></a>Criar tópicos e subscrições

O serviço do Event Grid cria e gere o tópico correspondente num domínio com base na chamada para criar uma subscrição de evento para um tópico de domínio automaticamente. Não existe passo separado para criar um tópico num domínio. Da mesma forma, quando a última subscrição de evento para um tópico é excluída, o tópico é eliminado também.

Subscrever um tópico num domínio é igual a subscrição a qualquer outro recurso do Azure:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/<my-topic>" \
  --endpoint https://contoso.azurewebsites.net/api/f1?code=code
```

O ID de recurso fornecido é o mesmo ID devolvido ao criar o domínio anteriormente. Para especificar o tópico que pretende subscrever, adicione `/topics/<my-topic>` ao final do ID de recurso.

Para criar uma subscrição de evento de âmbito de domínio que recebe a todos os eventos no domínio, proporciona o domínio como a `resource-id` sem especificar qualquer tópicos como `/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>`.

Se precisar de um ponto final de teste para subscrever os eventos, sempre pode implementar um [aplicação web pré-criados](https://github.com/Azure-Samples/azure-event-grid-viewer) que apresenta os eventos de entrada. Pode enviar seus eventos ao seu Web site de teste em `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

As permissões que estão definidas para um tópico são armazenadas no Azure Active Directory e tem de ser eliminadas explicitamente. A eliminar uma subscrição de evento não revogar um acesso de utilizadores para criar subscrições de eventos, se tiver acesso de escrita sobre um tópico.

## <a name="manage-access-to-topics"></a>Gerir o acesso aos tópicos

Gerir o acesso aos tópicos é feita por meio [atribuição de função](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-cli). Atribuição de função usa a verificação de acesso com base em função para limitar as operações em recursos do Azure para os utilizadores autorizados num determinado âmbito.

Grelha de eventos tem duas funções incorporadas que pode utilizar para atribuir utilizadores particulares acesso sobre vários tópicos dentro de um domínio. Estas funções são `EventGrid EventSubscription Contributor (Preview)`, que permite a criação e eliminação das subscrições, e `EventGrid EventSubscription Reader (Preview)`, que permite que apenas para a lista de subscrições de eventos.

O seguinte comando limitaria `alice@contoso.com` para criar e eliminar subscrições de eventos apenas no tópico `foo`:

```azurecli-interactive
az role assignment create --assignee alice@contoso.com --role "EventGrid EventSubscription Contributor (Preview)" --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/foo
```

Ver [Event Grid segurança e autenticação](./security-authentication.md) para obter mais informações sobre:

* Controlo de acesso de gestão
* Tipos de operação
* Criar definições de função personalizada

## <a name="publish-events-to-an-event-grid-domain"></a>Publicar eventos para um domínio de grelha de eventos

A publicação de eventos a um domínio é o mesmo que [publicar um tópico personalizado](./post-to-custom-topic.md). A única diferença é que tem de especificar o tópico que desejar que cada evento para ir para. A matriz de eventos a seguir iria resultar em eventos com `"id": "1111"` para o tópico `foo` ao evento com `"id": "2222"` seriam enviados para o tópico `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Para obter as chaves para uma utilização de domínio:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

E, em seguida, utilizar o seu método preferido de fazer com que um HTTP POST para publicar seus eventos ao seu domínio de grelha de eventos.

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre conceitos de alto nível em domínios de eventos e por que eles são úteis, consulte a [descrição geral conceptual dos domínios de evento](./event-domains.md).