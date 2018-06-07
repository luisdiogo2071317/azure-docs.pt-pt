---
title: Encaminhar eventos de Media Services do Azure para um ponto final web personalizado | Microsoft Docs
description: Utilize a grelha de eventos do Azure para subscrever o evento de alteração de estado de tarefa de Media Services.
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
ms.openlocfilehash: 6a098f43819bb6581b2c5978fbcc4a378a8514c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638505"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Eventos de Media Services do Azure de rota para um ponto final de web personalizado ao utilizar a CLI

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, pode utiliza a CLI do Azure para subscrever eventos de alteração de estado de tarefa de Media Services do Azure e acionar o evento para ver o resultado. 

Normalmente, os eventos são enviados para um ponto final que responde ao evento, como um webhook ou uma Função do Azure. Este tutorial mostra como criar e definir um webhook.

Quando concluir os passos descritos neste artigo, pode ver que os dados do evento foram enviados para um ponto final.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e inicie o **CloudShell** para executar os comandos da CLI, conforme mostrado nos passos seguintes.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

Certifique-se lembrar-se os valores que utilizou para o nome de conta dos Media Services, o nome de armazenamento e o nome de recurso.

## <a name="enable-event-grid-resource-provider"></a>Ativar o fornecedor de recursos de grelha de eventos

Primeira coisa que precisa de fazer é Certifique-se de que tem ativado na sua subscrição do fornecedor de recursos de grelha de eventos. 

No **Azure** portal efetue o seguinte procedimento:

1. Aceda às subscrições.
2. Selecione a sua subscrição.
3. Em definições, selecione os fornecedores de recursos.
4. Procure "EventGrid".
5. Certifique-se a que grelha de evento é registada. Se não, prima a **registar** botão.  

## <a name="create-a-generic-azure-function-webhook"></a>Criar um webhook genérico de função do Azure 

### <a name="create-a-message-endpoint"></a>Criar um ponto final de mensagem

Antes de subscrever artigo a grelha de evento, crie um ponto final que recolhe as mensagens para que possa vê-los.

Criar uma função acionada por um webhook genérico, conforme descrito no [webhook genérico](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) artigo. Neste tutorial, o **c#** código é utilizado.

Depois de criar o webhook, copie o URL ao clicar no *obter URL de função* ligação na parte superior a **Azure** janela portal. Não é necessário a última parte do URL (*& clientID = predefinido*).

![Criar um webhook](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Validar o webhook

Quando registar o próprio ponto final de webhook a grelha de evento, envia-lhe um pedido POST com um código de validação simples para provar a propriedade de ponto final. A aplicação tem de responder ao echoing novamente o código de validação. Grelha de eventos não fornecer eventos aos pontos finais de webHook que ainda não passaram a validação. Para obter mais informações, consulte [grelha de eventos de segurança e autenticação](https://docs.microsoft.com/azure/event-grid/security-authentication). Esta secção define duas partes, que tem de ser definidas para passar a validação.

#### <a name="update-the-source-code"></a>Atualize o código fonte

Depois de criar o webhook, o **run.csx** ficheiro é apresentada no browser. Substitua o código de predefinição com o seguinte código. 

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

À direita do **Azure** janela portal, verá dois separadores: **ver ficheiros** e **teste**. Selecione o separador **Teste**. No **corpo do pedido**, cole o seguinte json. Colar como está, sem necessidade de alterar quaisquer valores.

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

Prima **guarde e execute** na parte superior da janela.

![Corpo do pedido](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Registar-se para a subscrição de evento grelha 

Subscrever um artigo dizer eventos grelha os eventos que pretende controlar. O exemplo seguinte subscreve a conta de Media Services criou e transmite o URL do webhook de função do Azure que criou como o ponto final da notificação de evento. 

Substitua `<event_subscription_name>` com um nome exclusivo para a sua subscrição de evento. Para `<resource_group_name>` e `<ams_account_name>`, utilize o valor que criou anteriormente.  Para o `<endpoint_URL>` cole o URL de ponto final. Remover *& clientID = predefinido* partir do URL. Ao especificar um ponto final quando subscrever, o Event Grid processa o encaminhamento de eventos para esse ponto final. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

O valor de id de recurso de conta de Media Services semelhante a isto:

/subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/Providers/Microsoft.Media/mediaservices/amstestaccount

## <a name="test-the-events"></a>Os eventos de teste

Execute uma tarefa de codificação. Por exemplo, conforme descrito no [transmitir ficheiros de vídeo](stream-files-dotnet-quickstart.md) início rápido.

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Navegue para o webhook que criou anteriormente. Clique em **Monitor** e **atualizar**. Ver eventos de alterações do Estado da tarefa: "Em fila", "Agendada", "Processamento", "Concluída", "Error", "Cancelada", "Cancelar".  Para obter mais informações, consulte [esquemas de eventos de Media Services](media-services-event-schemas.md).

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

[Reagir a eventos](reacting-to-media-services-events.md)# # Consulte também

## <a name="see-also"></a>Consulte também

[CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
