---
title: Webhooks de registo de contentor do Azure
description: Saiba como utilizar webhooks para acionar eventos quando ocorrem determinadas ações em seus repositórios de registo.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/20/2017
ms.author: marsma
ms.openlocfilehash: c424e81b13c3c60e975d3721693b1f80e00cfdd7
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059932"
---
# <a name="using-azure-container-registry-webhooks"></a>Utilizar webhooks de registo de contentor do Azure

Um registo de contentor do Azure armazena e gerencia imagens privadas do Docker container, semelhantes à forma como o Docker Hub armazena imagens do Docker públicas. Pode utilizar webhooks para acionar eventos quando determinadas ações em um dos seus repositórios do registo. Webhooks pode responder a eventos ao nível do registo ou elas podem ser confinadas para uma etiqueta de repositório específico.

Para obter detalhes sobre pedidos de webhook, veja [referência de esquema de webhook do Azure Container Registry](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Pré-requisitos

* Registo de contentor do Azure - criar um registo de contentor na sua subscrição do Azure. Por exemplo, utilizar o [portal do Azure](container-registry-get-started-portal.md) ou o [CLI do Azure](container-registry-get-started-azure-cli.md).
* Docker CLI - configurar o seu computador local como anfitrião Docker e aceder aos comandos CLI do Docker, instale [motor do Docker](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Criar webhook portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
1. Navegue para o registo de contentor no qual pretende criar um webhook.
1. Sob **serviços**, selecione **Webhooks**.
1. Selecione **adicionar** na barra de ferramentas da webhook.
1. Concluir o *criar webhook* formulário com as seguintes informações:

| Valor | Descrição |
|---|---|
| Nome | O nome que pretende dar para o webhook. Pode conter apenas letras minúsculas e números e tem de ser 5 a 50 carateres de comprimento. |
| URI de serviço | O URI onde o webhook deve enviar as notificações de POST. |
| Cabeçalhos personalizados | Cabeçalhos que pretende passar juntamente com o pedido POST. Eles devem estar na "chave: valor" formato. |
| Ações de Acionador | Ações que acionam o webhook. Webhooks atualmente pode ser acionados por push de imagem e/ou eliminar ações. |
| Estado | O estado do webhook depois de criado. Ele é habilitado por padrão. |
| Âmbito | O âmbito em que o webhook funciona. Por predefinição, o escopo é para todos os eventos no registo. Pode ser especificado para um repositório ou uma etiqueta com o formato "repositório: etiqueta". |

Formulário de webhook de exemplo:

![Criação do ACR webhook da interface do Usuário no portal do Azure](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Criar webhook da CLI do Azure

Para criar um webhook com a CLI do Azure, utilize o [az acr webhook criar](/cli/azure/acr/webhook#az-acr-webhook-create) comando.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Testar webhook

### <a name="azure-portal"></a>Portal do Azure

Antes de utilizar o webhook no contentor de push de imagem e eliminar ações, pode testá-lo com o **Ping** botão. Ping envia um pedido POST genérico para o ponto final especificado e regista a resposta. Utilizar a funcionalidade de ping pode ajudá-lo a certifique-se de que configurou corretamente o webhook.

1. Selecione o webhook que pretende testar.
2. Na barra de ferramentas superior, selecione **Ping**.
3. Verifique a resposta do ponto de extremidade na **estado de HTTP** coluna.

![Criação do ACR webhook da interface do Usuário no portal do Azure](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>CLI do Azure

Para testar um webhook do ACR com a CLI do Azure, utilize o [ping do az acr webhook](/cli/azure/acr/webhook#az-acr-webhook-ping) comando.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Para ver os resultados, utilize o [az acr webhook lista-eventos](/cli/azure/acr/webhook#list-events) comando.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Eliminar webhook

### <a name="azure-portal"></a>Portal do Azure

Cada webhook pode ser eliminado ao selecionar o webhook e, em seguida, o **eliminar** botão no portal do Azure.

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Passos Seguintes

### <a name="webhook-schema-reference"></a>Referência de esquema de Webhook

Para obter detalhes sobre o formato e propriedades do conteúdo de eventos do JSON emitida pelo Azure Container Registry, veja a referência de esquema de webhook:

[Referência de esquema de webhook de registo de contentor do Azure](container-registry-webhook-reference.md)

### <a name="event-grid-events"></a>Eventos do Event Grid

Além dos eventos de webhook de registo nativo discutidos neste artigo, o Azure Container Registry pode emitir eventos para o Event Grid:

[Início rápido: Enviar contentor eventos de registo para o Event Grid](container-registry-event-grid-quickstart.md)