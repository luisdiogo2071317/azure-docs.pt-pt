---
title: Referência de esquema de webhook de registo de contentor do Azure
description: Webhook pedido JSON payload referência para o Azure Container Registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: danlep
ms.openlocfilehash: 42790905509e2ea8bbba87587ed01b1929221db5
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329324"
---
# <a name="azure-container-registry-webhook-reference"></a>Referência do webhook de registo de contentor do Azure

Pode [configurar webhooks](container-registry-webhook.md) para o seu registo de contentor que geram eventos quando determinadas ações são efetuadas em relação a ele. Por exemplo, ativar webhooks que são acionados na imagem de contentor `push` e `delete` operações. Quando for acionado um webhook, o Azure Container Registry emite um pedido HTTP ou HTTPS que contém informações sobre o evento para um ponto final que especificar. O ponto final, em seguida, pode processar o webhook e aja em conformidade.

As secções seguintes detalham o esquema de pedidos de webhook gerado por eventos suportados. As secções de eventos contêm o esquema do payload para o tipo de evento, um payload de pedido de exemplo e um ou mais comandos de exemplo que irá acionar o webhook.

Para obter informações sobre como configurar webhooks para o seu registo de contentor do Azure, consulte [webhooks de utilizar o Azure Container Registry](container-registry-webhook.md).

## <a name="webhook-requests"></a>Pedidos de Webhook

### <a name="http-request"></a>Pedido HTTP

Um webhook acionado faz com que um HTTP `POST` pedido para o ponto de final do URL que especificou quando configurou o webhook.

### <a name="http-headers"></a>Cabeçalhos HTTP

Pedidos de Webhook incluir uma `Content-Type` dos `application/json` se não tiver especificado um `Content-Type` cabeçalho personalizado para o webhook.

Não existem outros cabeçalhos são adicionados à solicitação além desses cabeçalhos personalizados que pode ter especificado para o webhook.

## <a name="push-event"></a>Evento de push

Acionado quando uma imagem de contentor é emitidos via push para um repositório de Webhook.

### <a name="push-event-payload"></a>Payload de evento de push

|Elemento|Type|Descrição|
|-------------|----------|-----------|
|`id`|String|O ID do evento de webhook.|
|`timestamp`|DateTime|A hora em que o evento de webhook foi acionado.|
|`action`|String|A ação que acionou o evento de webhook.|
|[target](#target)|Tipo complexo|O destino do evento que acionou o evento de webhook.|
|[request](#request)|Tipo complexo|O pedido que gerou o evento de webhook.|

### <a name="target"></a>destino

|Elemento|Type|Descrição|
|------------------|----------|-----------|
|`mediaType`|String|O tipo MIME do objeto referenciado.|
|`size`|Int32|O número de bytes do conteúdo. Mesmo que o campo de comprimento.|
|`digest`|String|O resumo do conteúdo, conforme definido pela especificação de API de HTTP do registo V2.|
|`length`|Int32|O número de bytes do conteúdo. Mesmo que o campo de tamanho.|
|`repository`|String|O nome do repositório.|
|`tag`|String|O nome da tag de imagem.|

### <a name="request"></a>pedido

|Elemento|Type|Descrição|
|------------------|----------|-----------|
|`id`|String|O ID do pedido que deu início ao evento.|
|`host`|String|O nome de anfitrião acessível externamente da instância do Registro, conforme especificado pelo cabeçalho de anfitrião HTTP em solicitações de entrada.|
|`method`|String|O método de pedido que gerou o evento.|
|`useragent`|String|O cabeçalho do agente de utilizador do pedido.|

### <a name="payload-example-push-event"></a>Exemplo de payload: eventos de push

```JSON
{
  "id": "cb8c3971-9adc-488b-bdd8-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-86cd-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Exemplo [CLI do Docker](https://docs.docker.com/engine/reference/commandline/cli/) comando que aciona a **push** webhook de evento:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="delete-event"></a>Eliminar evento

Acionado quando é eliminado um repositório ou o manifesto de Webhook. Não é acionado quando uma etiqueta é eliminada.

### <a name="delete-event-payload"></a>Eliminar o payload do evento

|Elemento|Type|Descrição|
|-------------|----------|-----------|
|`id`|String|O ID do evento de webhook.|
|`timestamp`|DateTime|A hora em que o evento de webhook foi acionado.|
|`action`|String|A ação que acionou o evento de webhook.|
|[target](#delete_target)|Tipo complexo|O destino do evento que acionou o evento de webhook.|
|[request](#delete_request)|Tipo complexo|O pedido que gerou o evento de webhook.|

### <a name="delete_target"></a> Destino

|Elemento|Type|Descrição|
|------------------|----------|-----------|
|`mediaType`|String|O tipo MIME do objeto referenciado.|
|`digest`|String|O resumo do conteúdo, conforme definido pela especificação de API de HTTP do registo V2.|
|`repository`|String|O nome do repositório.|

### <a name="delete_request"></a> Pedido

|Elemento|Type|Descrição|
|------------------|----------|-----------|
|`id`|String|O ID do pedido que deu início ao evento.|
|`host`|String|O nome de anfitrião acessível externamente da instância do Registro, conforme especificado pelo cabeçalho de anfitrião HTTP em solicitações de entrada.|
|`method`|String|O método de pedido que gerou o evento.|
|`useragent`|String|O cabeçalho do agente de utilizador do pedido.|

### <a name="payload-example-delete-event"></a>Exemplo de payload: Eliminar evento

```JSON
{
    "id": "afc359ce-df7f-4e32-bdde-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-807f-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Exemplo [CLI do Azure](/cli/azure/acr) comandos nesse disparador um **eliminar** webhook de evento:

```azurecli
# Delete repository
az acr repository delete --name MyRegistry --repository MyRepository

# Delete image
az acr repository delete --name MyRegistry --image MyRepository:MyTag
```

## <a name="next-steps"></a>Passos Seguintes

[Utilizar webhooks de registo de contentor do Azure](container-registry-webhook.md)