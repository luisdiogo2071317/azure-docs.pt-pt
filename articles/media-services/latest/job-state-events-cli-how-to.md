---
title: Encaminhar eventos de serviços de multimédia do Azure para um ponto final web personalizado | Documentos da Microsoft
description: Utilize o Azure Event Grid para subscrever o evento de alteração de estado de tarefa de serviços de multimédia.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: e9df0cd24ef890765b78c25a073d671889be10a7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724065"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Encaminhar eventos de serviços de multimédia do Azure para um ponto final web personalizado com a CLI

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, utilizar a CLI do Azure para subscrever a eventos de alteração de estado de tarefa de serviços de multimédia do Azure e acionar o evento para ver o resultado. 

Normalmente, os eventos são enviados para um ponto final que responde ao evento, como um webhook ou uma Função do Azure. Este tutorial mostra como criar e definir um webhook.

Quando concluir os passos descritos neste artigo, pode ver que os dados do evento foram enviados para um ponto final.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e inicie o **CloudShell** para executar os comandos da CLI, conforme mostrado nos passos seguintes.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI do Azure](/cli/azure/install-azure-cli). 

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

Lembre-se de que não se esqueça dos valores que utilizou para o nome de conta dos Media Services, o nome de armazenamento e o nome do recurso.

## <a name="enable-event-grid-resource-provider"></a>Ativar o fornecedor de recursos do Event Grid

Primeira coisa que precisa fazer é certificar-se de que tem o fornecedor de recursos do Event Grid ativada na sua subscrição. 

Na **Azure** portal efetue o seguinte procedimento:

1. Ir para as subscrições.
2. Selecione a sua subscrição.
3. Em definições, selecione os fornecedores de recursos.
4. Procure "EventGrid".
5. Certifique-se de que o Event Grid está registado. Se não estiver, prima a **registar** botão.  

## <a name="create-a-generic-azure-function-webhook"></a>Criar um webhook genérico de função do Azure 

### <a name="create-a-message-endpoint"></a>Criar um ponto final de mensagem

Antes de subscrever o artigo do Event Grid, crie um ponto final que recolhe as mensagens para que possa vê-las.

Criar uma função acionada por um webhook genérico, conforme descrito no [webhook genérico](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) artigo. Neste tutorial, o **c#** código é usado.

Assim que o webhook for criado, copie o URL ao clicar no *obter URL de função* link na parte superior a **Azure** janela portal. Não é necessário a última parte do URL (*& clientID = default*).

![Criar um webhook](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Validar o webhook

Quando registra seu próprio ponto final do webhook com o Event Grid, ele envia-lhe um pedido POST com um código de validação simples para provar a propriedade de ponto final. A aplicação tem de responder ao repetir o código de validação. Grelha de eventos não entrega eventos para pontos finais de webHook que ainda não passou a validação. Para obter mais informações, consulte [Event Grid segurança e autenticação](https://docs.microsoft.com/azure/event-grid/security-authentication). Esta secção define duas partes que devem ser definidos para a validação passar.

#### <a name="update-the-source-code"></a>Atualizar o código-fonte

Depois de criado o webhook, o **csx** ficheiros é apresentada no browser. Substitua o código de padrão com o código a seguir. 

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"Webhook was triggered!");

    string jsonContent = await req.Content.ReadAsStringAsync();
    string eventGridValidation = 
        req.Headers.FirstOrDefault( x => x.Key == "Aeg-Event-Type" ).Value?.FirstOrDefault();

    dynamic eventData = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"event: {eventData}");

    if (eventGridValidation != String.Empty)
    {
        if (eventData[0].data.validationCode !=String.Empty && eventData[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            return req.CreateResponse(HttpStatusCode.OK, new 
            {
                validationResponse = eventData[0].data.validationCode
            });
        }
    }
    
    log.Info(jsonContent);

    return req.CreateResponse(HttpStatusCode.OK);
}
```

#### <a name="update-test-request-body"></a>Atualizar o corpo do pedido de teste

No lado direito do **Azure** janela portal, verá dois separadores: **ver ficheiros** e **teste**. Selecione o separador **Teste**. Na **corpo do pedido**, cole o seguinte json. Pode colá-lo como está, sem a necessidade de alterar quaisquer valores.

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}
]
```

Prima **salve e execute** na parte superior da janela.

![Corpo do pedido](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Registre-se para a subscrição do Event Grid 

Inscreva-se a um artigo para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve à conta de serviços de multimédia que criou e transmite o URL do webhook de função do Azure que criou como o ponto final para notificação de eventos. 

Substitua `<event_subscription_name>` com um nome exclusivo para a sua subscrição de evento. Para `<resource_group_name>` e `<ams_account_name>`, utilize o valor que criou anteriormente.  Para o `<endpoint_URL>` cole o URL de ponto final. Remova *& clientID = default* da URL. Ao especificar um ponto final quando subscrever, o Event Grid processa o encaminhamento de eventos para esse ponto final. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

O valor de id do recurso de conta dos serviços de multimédia é semelhante a este:

/subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/Providers/Microsoft.Media/mediaservices/amstestaccount

## <a name="test-the-events"></a>Os eventos de teste

Execute uma tarefa de codificação. Por exemplo, conforme descrito no [Stream ficheiros de vídeo](stream-files-dotnet-quickstart.md) início rápido.

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Navegue para o webhook que criou anteriormente. Clique em **Monitor** e **atualizar**. Ver eventos de altera o estado da tarefa: "Em fila", "Agendada", "Processamento", "Concluído", "Error", "Cancelada", "Cancelar".  Para obter mais informações, consulte [esquemas de eventos dos serviços de multimédia](media-services-event-schemas.md).

Por exemplo:

```json
[{
  "topic": "/subscriptions/<subscription id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job id>",
  "eventType": "Microsoft.Media.JobStateChange",
  "eventTime": "2018-04-20T21:17:26.2534881",
  "id": "<id>",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

![Eventos de teste](./media/job-state-events-cli-how-to/test_events.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar a trabalhar com esta conta de armazenamento e subscrição de eventos, não limpe os recursos criados neste artigo. Se não quiser continuar, utilize o comando seguinte para eliminar os recursos que criou neste artigo.

Substitua `<resource_group_name>` pelo grupo de recursos que criou acima.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Passos Seguintes

[Reagir a eventos](reacting-to-media-services-events.md)

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
