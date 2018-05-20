---
title: Autenticação e segurança de grelha de eventos do Azure
description: Descreve os conceitos e grelha de eventos do Azure.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: babanisa
ms.openlocfilehash: 783766c3e12da2c6fd77f919cf0ec44aea7db3b7
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="event-grid-security-and-authentication"></a>Segurança de grelha de eventos e autenticação 

Grelha de eventos do Azure tem três tipos de autenticação:

* Subscrições de eventos
* Publicação do evento
* Entrega de eventos do WebHook

## <a name="webhook-event-delivery"></a>Entrega de eventos do WebHook

Webhooks são uma das várias formas para receber eventos da grelha de eventos do Azure. Quando um novo evento estiver pronto, o Webhook de grelha de eventos envia um pedido HTTP para o ponto final de HTTP configurado com o evento no corpo.

Quando registar o próprio ponto final de WebHook a grelha de evento, envia-lhe um pedido POST com um código de validação simples para provar a propriedade de ponto final. A aplicação tem de responder ao echoing novamente o código de validação. Grelha de eventos não fornecer eventos aos pontos finais de WebHook que ainda não passaram a validação. Se utilizar um serviço de API de terceiros (como [Zapier](https://zapier.com) ou [IFTTT](https://ifttt.com/)), poderá não conseguir eco programaticamente o código de validação. Para esses serviços, pode validar manualmente a subscrição utilizando um URL de validação que é enviado o evento de validação da subscrição. Copiar esse URL e enviar um pedido GET através de um cliente REST ou o seu browser.

Validação manual está em pré-visualização. Para utilizá-lo, tem de instalar o [extensão da grelha de evento](/cli/azure/azure-cli-extensions-list) para [AZ CLI 2.0](/cli/azure/install-azure-cli). Pode instalá-lo com `az extension add --name eventgrid`. Se estiver a utilizar a API REST, certifique-se de que está a utilizar `api-version=2018-05-01-preview`.

### <a name="validation-details"></a>Detalhes de validação

* No momento da criação/atualização da subscrição do evento, o evento grelha mensagens um evento de "SubscriptionValidationEvent" para o ponto final de destino.
* O evento contém um valor de cabeçalho "Tipo de evento Aeg: SubscriptionValidation".
* O corpo de evento tem o mesmo esquema que outros eventos de grelha de eventos.
* Os dados do evento incluem uma propriedade de "validationCode" com uma cadeia gerada aleatoriamente. Por exemplo, "validationCode: acb13...".
* Os dados do evento incluem uma propriedade de "validationUrl" com um URL para validar manualmente a subscrição.
* A matriz contém apenas o evento de validação. Outros eventos são enviados num pedido separado depois de volta eco o código de validação.

Um exemplo SubscriptionValidationEvent é apresentado no exemplo seguinte:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=B2E34264-7D71-453A-B5FB-B62D0FDC85EE&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1BNqCxBBSSE9OnNSfZM4%2b5H9zDegKMY6uJ%2fO2DFRkwQ%3d"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Para provar a propriedade de ponto final, escreverá novamente o código de validação na propriedade validationResponse, conforme mostrado no exemplo seguinte:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Em alternativa, a subscrição de validação manualmente através do envio de um pedido GET para o URL de validação. A subscrição de evento permanece no estado pendente até validada.

### <a name="event-delivery-security"></a>Segurança de entrega de eventos

Pode proteger o ponto final de webhook ao adicionar parâmetros de consulta para o URL do webhook quando criar uma subscrição de evento. Definir um destes parâmetros de consulta para ser um segredo como um [token de acesso](https://en.wikipedia.org/wiki/Access_token) que pode utilizar o webhook para reconhecer o evento é proveniente de grelha de eventos com permissões válidas. Grelha de eventos irá incluir estes parâmetros de consulta em cada entrega de eventos para o webhook.

Ao editar a subscrição de evento, os parâmetros de consulta não irão sejam apresentados ou devolvidos, a menos que o [– incluir-full--url de ponto final](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_show) parâmetro é utilizado no Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).

Por fim, é importante ter em atenção que a grelha de eventos do Azure suporta apenas pontos finais de webhook HTTPS.

## <a name="event-subscription"></a>Subscrição de evento

Para subscrever um evento, tem de ter o **Microsoft.EventGrid/EventSubscriptions/Write** permissão no recurso necessário. Tem esta permissão porque está a escrever uma nova subscrição no âmbito do recurso. O recurso necessário difere consoante se está a subscrever para um tópico de sistema ou um tópico personalizado. Ambos os tipos são descritos nesta secção.

### <a name="system-topics-azure-service-publishers"></a>Tópicos de sistema (editores de serviço do Azure)

Tópicos de sistema, necessita de permissão para escrever uma nova subscrição de eventos no âmbito do recurso publicar o evento. O formato do recurso é: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Por exemplo subscrever um evento de uma conta de armazenamento com o nome **myacct**, é preciso permissão Microsoft.EventGrid/EventSubscriptions/Write em: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Tópicos personalizados

Tópicos personalizado, necessita de permissão para escrever uma nova subscrição de eventos no âmbito do tópico de grelha de eventos. O formato do recurso é: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Por exemplo subscrever um tópico personalizado denominado **mytopic**, é preciso permissão Microsoft.EventGrid/EventSubscriptions/Write em: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Publicação de tópico

Tópicos sobre utiliza a assinatura de acesso partilhado (SAS) ou autenticação de chave. Recomendamos SAS, mas a autenticação de chave fornece programação simple, não sendo compatível com muitos editores de webhook existente. 

Incluir o valor de autenticação no cabeçalho de HTTP. Para SAS, utilize **token de sas aeg** para o valor do cabeçalho. Para autenticação de chave, utilize **chave de sas aeg** para o valor do cabeçalho.

### <a name="key-authentication"></a>Autenticação de chave

Autenticação de chave é a forma mais simples de autenticação. Utilize o formato: `aeg-sas-key: <your key>`

Por exemplo, transmitir uma chave com:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Tokens SAS

Os tokens SAS de grelha de eventos incluem o recurso, uma hora de expiração e uma assinatura. O formato do SAS token é: `r={resource}&e={expiration}&s={signature}`.

O recurso é o caminho para o tópico de grelha de eventos para o qual está a enviar eventos. Por exemplo, é um caminho de recurso válido: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Gerar a assinatura de uma chave.

Por exemplo, um válido **token de sas aeg** valor é:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

O exemplo seguinte cria um token SAS para utilização com grelha de evento:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Controlo de acesso de gestão

Grelha de eventos do Azure permite-lhe controlar o nível de acesso aos diferentes utilizadores várias operações de gestão, tais como as subscrições de eventos de lista, criar novos, e gerar as chaves. Grelha de evento utiliza função de acesso baseado em verificar (RBAC do Azure).

### <a name="operation-types"></a>Tipos de operação

Grelha de eventos do Azure suporta as seguintes ações:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

As últimas três operações devolvem informações potencialmente secretas, que obtém filtradas normais operações de leitura. É recomendado para que possa restringir o acesso a estas operações. Funções personalizadas podem ser criadas utilizando [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), [Interface de linha de comandos do Azure (CLI)](../role-based-access-control/role-assignments-cli.md)e o [REST API](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Impor a função com base em verificação de acesso (RBAC)

Utilize os seguintes passos para impor o RBAC para diferentes utilizadores:

#### <a name="create-a-custom-role-definition-file-json"></a>Criar um ficheiro de definição de função personalizada (. JSON)

Seguem-se as definições de funções de grelha de eventos de exemplo que permitem aos utilizadores efetuar diferentes conjuntos de ações.

**EventGridReadOnlyRole.json**: permitir apenas operações só de leitura.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: permitir ações post restrito, mas não o permitir ações de eliminação.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: permite que todas as ações de grelha de eventos.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Criar e atribuir uma função personalizada com o CLI do Azure

Para criar uma função personalizada, utilize:

```azurecli
az role definition create --role-definition @<file path>
```

Para atribuir a função a um utilizador, utilize:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md)
