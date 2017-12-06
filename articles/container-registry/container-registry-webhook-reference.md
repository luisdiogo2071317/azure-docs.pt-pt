---
title: "Referência do esquema de webhook de registo de contentor do Azure"
description: "Webhook pedido JSON payload referência para o registo de contentor do Azure."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: 84f0277a7b1a5bd7dfe2178f78f34140b1dd2642
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-webhook-reference"></a>Referência de webhook de registo de contentor do Azure

Pode [configurar webhooks](container-registry-webhook.md) para o registo de contentor gerar eventos quando determinadas ações são efetuadas nele. Por exemplo, pode ativar webhooks são acionados na imagem de contentor `push` e `delete` operações. Quando é acionado um webhook, o registo de contentor do Azure emite um pedido HTTP ou HTTPS que contém informações sobre o evento para um ponto final que especificar. O ponto final, em seguida, pode processar o webhook e agir em conformidade.

As secções seguintes detalhe o esquema de pedidos de webhook gerado por eventos suportados. As secções de eventos contêm o esquema de payload para o tipo de evento, um payload de pedido de exemplo e um ou mais comandos de exemplo que seriam acionar o webhook.

Para obter informações sobre como configurar webhooks para o registo de contentor do Azure, consulte [webhooks de registo de contentor Azure utilizando](container-registry-webhook.md).

## <a name="webhook-requests"></a>Pedidos de Webhook

### <a name="http-request"></a>Pedido HTTP

Um webhook accionado faz com que um HTTP `POST` pedido para o ponto final do URL especificado quando configurou o webhook.

### <a name="http-headers"></a>Cabeçalhos de HTTP

Pedidos de Webhook incluir um `Content-Type` de `application/json` se não especificou um `Content-Type` cabeçalho personalizado para o webhook.

Não existem outros cabeçalhos são adicionados para o pedido para além desses cabeçalhos personalizados, que pode ter especificado para o webhook.

## <a name="push-event"></a>Evento de push

Acionado quando é feita uma imagem de contentor para um repositório de Webhook.

### <a name="push-event-payload"></a>Payload de evento de push

|Elemento|Tipo|Descrição|
|-------------|----------|-----------|
|`id`|Cadeia|O ID do evento webhook.|
|`timestamp`|DateTime|A hora em que o evento de webhook foi acionado.|
|`action`|Cadeia|A ação que acionou o evento de webhook.|
|[destino](#target)|Tipo complexo|O destino do evento que acionou o evento de webhook.|
|[pedido](#request)|Tipo complexo|O pedido que gerou o evento de webhook.|

### <a name="target"></a>destino

|Elemento|Tipo|Descrição|
|------------------|----------|-----------|
|`mediaType`|Cadeia|O tipo MIME do objeto referenciado.|
|`size`|Int32|O número de bytes do conteúdo. Mesmo que o comprimento do campo.|
|`digest`|Cadeia|O resumo do conteúdo, tal como definido pela especificação de API do registo V2 HTTP.|
|`length`|Int32|O número de bytes do conteúdo. Igual ao campo tamanho.|
|`repository`|Cadeia|O nome do repositório.|
|`tag`|Cadeia|O nome da tag de imagem.|

### <a name="request"></a>Pedido

|Elemento|Tipo|Descrição|
|------------------|----------|-----------|
|`id`|Cadeia|O ID do pedido que iniciou o evento.|
|`host`|Cadeia|O nome de anfitrião externamente acessível da instância do registo, tal como especificado pelo cabeçalho de anfitrião HTTP em pedidos recebidos.|
|`method`|Cadeia|O método de pedido que gerou o evento.|
|`useragent`|Cadeia|O cabeçalho de agente de utilizador do pedido.|

### <a name="payload-example-push-event"></a>Exemplo de payload: evento de push

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

Webhook acionado quando um repositório ou manifesto é eliminado. Não é acionado quando é eliminada uma etiqueta.

### <a name="delete-event-payload"></a>Eliminar payload do evento

|Elemento|Tipo|Descrição|
|-------------|----------|-----------|
|`id`|Cadeia|O ID do evento webhook.|
|`timestamp`|DateTime|A hora em que o evento de webhook foi acionado.|
|`action`|Cadeia|A ação que acionou o evento de webhook.|
|[destino](#delete_target)|Tipo complexo|O destino do evento que acionou o evento de webhook.|
|[pedido](#delete_request)|Tipo complexo|O pedido que gerou o evento de webhook.|

### <a name="delete_target"></a>destino

|Elemento|Tipo|Descrição|
|------------------|----------|-----------|
|`mediaType`|Cadeia|O tipo MIME do objeto referenciado.|
|`digest`|Cadeia|O resumo do conteúdo, tal como definido pela especificação de API do registo V2 HTTP.|
|`repository`|Cadeia|O nome do repositório.|

### <a name="delete_request"></a>pedido

|Elemento|Tipo|Descrição|
|------------------|----------|-----------|
|`id`|Cadeia|O ID do pedido que iniciou o evento.|
|`host`|Cadeia|O nome de anfitrião externamente acessível da instância do registo, tal como especificado pelo cabeçalho de anfitrião HTTP em pedidos recebidos.|
|`method`|Cadeia|O método de pedido que gerou o evento.|
|`useragent`|Cadeia|O cabeçalho de agente de utilizador do pedido.|

### <a name="payload-example-delete-event"></a>Exemplo de payload: eliminar o evento

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

Exemplo [Azure CLI 2.0](/cli/azure/acr) comandos que acionador um **eliminar** webhook de evento:

```azurecli
# Delete repository
az acr repository delete -n MyRegistry --repository MyRepository

# Delete manifest
az acr repository delete -n MyRegistry --repository MyRepository --tag MyTag --manifest
```

## <a name="next-steps"></a>Passos seguintes

[Utilizar o registo de contentor do Azure webhooks](container-registry-webhook.md)