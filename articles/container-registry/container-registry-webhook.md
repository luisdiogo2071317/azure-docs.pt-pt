---
title: Webhooks de registo de contentor do Azure
description: "Saiba como utilizar webhooks para acionar eventos quando ocorrem determinadas ações no seu repositórios de registo."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: nepeters
ms.openlocfilehash: 133e36179a500dc65c3a543266a7afcf9988b87d
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="using-azure-container-registry-webhooks"></a>Utilizar o registo de contentor do Azure webhooks

Um registo de contentor do Azure armazena e gere privadas Docker contentor imagens de forma semelhantes ao que docker Hub armazena públicas imagens de Docker. Pode utilizar webhooks para acionar eventos quando determinadas ações ocorrer dos seus repositórios de registo. Webhooks pode responder a eventos ao nível de registo ou podem ser confinados para uma etiqueta de repositório específico.

Para obter detalhes sobre pedidos de webhook, consulte [referência de esquema do registo de contentor do Azure webhook](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Pré-requisitos

* Registo de contentor do Azure - criar um registo de contentor na sua subscrição do Azure. Por exemplo, utilizar o [portal do Azure](container-registry-get-started-portal.md) ou [CLI do Azure](container-registry-get-started-azure-cli.md).
* Instalar o docker CLI - configurar o computador local como um anfitrião do Docker e aceder aos comandos da CLI do Docker, [motor de Docker](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Criar o webhook portal do Azure

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)
1. Navegue para o registo de contentor no qual pretende criar um webhook.
1. Em **serviços**, selecione **Webhooks**.
1. Selecione **adicionar** na barra de ferramentas do webhook.
1. Concluir o *criar o webhook* formulário com as seguintes informações:

| Valor | Descrição |
|---|---|
| Nome | O nome que pretende dar para o webhook. Pode conter apenas letras minúsculas e números e tem de ser 5 e 50 carateres de comprimento. |
| URI de serviço | O URI onde o webhook deve enviar notificações de POST. |
| Cabeçalhos personalizados | Os cabeçalhos que pretende passar juntamente com o pedido POST. Devem ser na "chave: valor" formato. |
| Ações de Acionador | Ações que o acionam o webhook. Webhooks atualmente pode ser acionada por push de imagem e/ou eliminar ações. |
| Estado | O estado para o webhook depois de criado. Está ativada por predefinição. |
| Âmbito | O âmbito no qual o webhook funciona. Por predefinição, o âmbito é para todos os eventos no registo. Pode ser especificado para um repositório ou uma etiqueta, utilizando o formato "repositório: tag". |

Formulário de webhook de exemplo:

![Criação de webhook ACR IU no portal do Azure](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Criar o webhook CLI do Azure

Para criar um webhook utilizando a CLI do Azure, utilize o [az acr webhook criar](/cli/azure/acr/webhook#create) comando.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Webhook de teste

### <a name="azure-portal"></a>Portal do Azure

Antes de utilizar o webhook no contentor de push de imagem e eliminar ações, pode testar com a **Ping** botão. Ping envia um pedido POST genérico para o ponto final especificado e regista a resposta. Utilizar a funcionalidade de ping pode ajudá-lo certifique-se de que configurou corretamente o webhook.

1. Selecione o webhook que pretende testar.
2. Na barra de ferramentas superior, selecione **Ping**.
3. Verifique a resposta do ponto final no **ao estado HTTP** coluna.

![Criação de webhook ACR IU no portal do Azure](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>CLI do Azure

Para testar um webhook ACR com a CLI do Azure, utilize o [ping de webhook az acr](/cli/azure/acr/webhook#ping) comando.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Para ver os resultados, utilize o [az acr webhook lista-eventos](/cli/azure/acr/webhook#list-events) comando.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Eliminar webhook

### <a name="azure-portal"></a>Portal do Azure

Cada webhook pode ser eliminado, selecionando o webhook e, em seguida, o **eliminar** botão no portal do Azure.

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Passos seguintes

[Referência do esquema de webhook de registo de contentor do Azure](container-registry-webhook-reference.md)