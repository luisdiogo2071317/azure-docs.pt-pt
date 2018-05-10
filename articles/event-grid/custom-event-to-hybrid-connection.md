---
title: Enviar eventos personalizados para a grelha de eventos do Azure para a ligação híbrida | Microsoft Docs
description: Utilize o Azure Event Grid e a CLI do Azure para publicar um tópico e subscrever esse evento. Uma ligação híbrida é utilizada para o ponto final.
services: event-grid
keywords: ''
author: tfitzmac
ms.author: tomfitz
ms.date: 05/04/2018
ms.topic: article
ms.service: event-grid
ms.openlocfilehash: 42b3e88d4bf411aa8a0d3bb129795f0d8ab98525
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="route-custom-events-to-azure-relay-hybrid-connections-with-azure-cli-and-event-grid"></a>Encaminhar eventos personalizados para o reencaminhamento híbrido ligações das Azure com a CLI do Azure e a grelha de eventos

O Azure Event Grid é um serviço de eventos para a cloud. As ligações híbridas do reencaminhamento do Azure é uma dos processadores de eventos suportados. Utilize as ligações híbridas como o processador de eventos quando necessita de processar os eventos de aplicações que não tenham um ponto final público. Estas aplicações podem ser dentro da sua rede empresarial. Neste artigo, a CLI do Azure é utilizada para criar um tópico personalizado, subscrever o tópico e acionar o evento para ver o resultado. Enviar eventos para a ligação híbrida.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já tem uma ligação híbrida e uma aplicação de serviço de escuta. Para começar a utilizar com as ligações híbridas, consulte o artigo [começar a utilizar ligações híbridas de reencaminhamento - .NET](../service-bus-relay/relay-hybrid-connections-dotnet-get-started.md) ou [começar a utilizar ligações híbridas de reencaminhamento - nó](../service-bus-relay/relay-hybrid-connections-node-get-started.md).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os tópicos do Event Grid são recursos do Azure e têm de ser colocados num grupo de recursos do Azure. Um grupo de recursos é uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). 

O exemplo seguinte cria um grupo de recursos com o nome *gridResourceGroup* na localização *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Criar um tópico personalizado

Um tópico do Event Grid fornece um ponto final definido pelo utilizador no qual publica os eventos. O exemplo seguinte cria o tópico personalizado no seu grupo de recursos. Substitua `<topic_name>` por um nome exclusivo para o seu tópico. O nome do tópico deve ser exclusivo, porque este é representado por uma entrada DNS.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="subscribe-to-a-topic"></a>Subscrever um tópico

Subscreva um tópico para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve o tópico que criou e transmite o ID de recurso da ligação híbrida para o ponto final. O ID da ligação híbrida está no formato:

`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Relay/namespaces/<relay-namespace>/hybridConnections/<hybrid-connection-name>`

O script seguinte obtém o ID de recurso do espaço de nomes de reencaminhamento. Constrói o ID da ligação híbrida e subscreve um tópico da grelha de eventos. Define o tipo de ponto final `hybridconnection` e utiliza o ID da ligação híbrida para o ponto final.

```azurecli-interactive
relayname=<namespace-name>
relayrg=<resource-group-for-relay>
hybridname=<hybrid-name>

relayid=$(az resource show --name $relayname --resource-group $relayrg --resource-type Microsoft.Relay/namespaces --query id --output tsv)
hybridid="$relayid/hybridConnections/$hybridname"

az eventgrid event-subscription create \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --name <event_subscription_name> \
  --endpoint-type hybridconnection \
  --endpoint $hybridid
```

## <a name="send-an-event-to-your-topic"></a>Enviar um evento para o seu tópico

Vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Em primeiro lugar, vamos obter o URL e a chave do tópico personalizado. Novamente, utilize o nome do tópico de `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Para simplificar este artigo, vai utilizar dados do evento de exemplo para enviar para o tópico. Normalmente, uma aplicação ou serviço do Azure enviaria os dados do evento. CURL é um utilitário que envia os pedidos HTTP. Neste artigo, utilize o CURL para enviar o evento para o tópico.  O exemplo seguinte envia três eventos para o tópico de grelha de evento:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

A aplicação de serviço de escuta deverá receber a mensagem de evento.

## <a name="clean-up-resources"></a>Limpar recursos
Se quiser continuar a trabalhar com este evento, não limpe os recursos criados neste artigo. Caso contrário, utilize o comando seguinte para eliminar os recursos que criou neste artigo.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como criar tópicos e subscrições de eventos, saiba mais sobre o que o Event Grid pode ajudá-lo a fazer:

- [Sobre o Event Grid](overview.md)
- [Encaminhar eventos de armazenamento de Blobs para um ponto final Web personalizado](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorizar alterações de máquina virtual com o Azure Event Grid e Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Transmitir macrodados em fluxo para um armazém de dados](event-grid-event-hubs-integration.md)
