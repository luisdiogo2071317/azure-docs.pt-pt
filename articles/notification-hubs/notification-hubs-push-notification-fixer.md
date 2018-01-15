---
title: "Os Hubs de notificação do Azure ignorados diagnóstico de notificação"
description: "Saiba como diagnosticar problemas comuns notificações ignorados nos Notification Hubs do Azure."
services: notification-hubs
documentationcenter: Mobile
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 3925208fe56bcd9513ec4c0f21aa1e2dd8fbf9c5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="diagnose-dropped-notifications-in-notification-hubs"></a>Diagnosticar ignoradas as notificações de Hubs de notificação

Uma das perguntas mais comuns de clientes de Notification Hubs do Azure é como resolver problemas quando as notificações que são enviadas a partir de uma aplicação não são apresentadas nos dispositivos cliente. Pretende saber onde e por que razão as notificações foram removidas e saber como corrigir o problema. Este artigo identifica a razão pela qual as notificações podem obter removidas ou não ser recebidas pelas dispositivos. Saiba como analisar e determinar a causa raiz. 

É fundamental compreender primeiro como Notification Hubs pushes notificações para um dispositivo.

![Arquitetura de Hubs de notificação][0]

Um fluxo de notificação envio típica, a mensagem é enviada do *back-end da aplicação* para os Notification Hubs. Os Notification Hubs efetua algumas processamento em todos os registos. O processamento tem em consideração as etiquetas configuradas e expressões de etiqueta para determinar o "destino". Destinos são todos os registos que necessitam para receber a notificação push. Estes registos podem abranger quaisquer ou todas as nossas plataformas suportadas: iOS, Google, Windows, Windows Phone, Kindle e Baidu para China Android.

Com os destinos estabelecidos, os Notification Hubs pushes notificações para o *push do serviço de notificação* para a plataforma do dispositivo. Os exemplos incluem o serviço Apple Push Notification (APNs) para a Apple e o Firebase Cloud Messaging (FCM) para o Google. Notificações de pushes de Hubs de notificação dividir por lotes de vários de registos. Os Notification Hubs autentica com o serviço de notificação push correspondentes com base nas credenciais que definiu no portal do Azure, em **configurar o Notification Hub**. A notificação push do serviço, em seguida, forwards as notificações para o respetivo *dispositivos cliente*. 

Tenha em atenção que a fase final da entrega de notificações ocorre entre o serviço de notificações de push de plataforma e o dispositivo. Qualquer um dos quatro componentes principais do processo de notificação push (cliente, back-end da aplicação, os Hubs de notificação e o serviço de notificação push plataforma) pode fazer com que as notificações ser removido. Para obter mais informações sobre a arquitetura de Notification Hubs, consulte [descrição geral dos Notification Hubs].

Falha entregar notificações poderão ocorrer durante o inicial/testes fase. Notificações ignoradas nesta fase, poderão indicar um problema de configuração. Se ocorrer uma falha para a entrega de notificações em produção, algumas ou todas as notificações podem ser removidas. Neste caso, é indicada uma aplicação mais aprofundada ou problema de padrão de mensagens. 

A secção seguinte analisa cenários em que poderão ser removidas notificações, entre comuns mais raro.

## <a name="notification-hubs-misconfiguration"></a>Configuração incorreta de Hubs de notificação
Com êxito enviar notificações para o serviço de notificação push respetivos, os Notification Hubs necessita para se autenticar no contexto da aplicação do programador. Para esta situação ocorrer, o programador cria uma conta de programador com a respetiva plataforma (Google, Apple, Windows e assim sucessivamente). Em seguida, o programador regista as respetivas aplicações com a plataforma de onde obter credenciais. 

Tem de adicionar as credenciais de plataforma para o portal do Azure. Se não existem notificações são alcançar o dispositivo, deve ser o primeiro passo para se certificar de que as credenciais corretas são configuradas no Notification Hubs. As credenciais têm de corresponder a aplicação que é criada com uma conta de programador específicos da plataforma. 

Para obter instruções passo a passo concluir este processo, consulte [começar com Notification Hubs do Azure].

Seguem-se algumas configurações incorretas comuns para procurar:

* **Geral**
   
    * Certifique-se de que o nome de hub de notificação (sem erros de digitação) é o mesmo em cada um destas localizações:

        * Onde regista do cliente.
        * Para onde enviar notificações de back-end.
        * Onde configurou as credenciais de serviço de notificação push.
    
    * Certifique-se de que, utilize as cadeias de configuração de assinatura de acesso partilhado corretos no cliente e em back-end da aplicação. Geralmente, tem de utilizar **DefaultListenSharedAccessSignature** no cliente e **DefaultFullSharedAccessSignature** na aplicação de back-end (concede permissões para enviar notificações para Hubs de notificação).

* **Configuração do APNs**
   
    Tem de manter dois hubs diferentes: um hub para produção e outro hub para fins de teste. Isto significa que tem de carregar o certificado que utilizar um ambiente sandbox para um hub separado que o certificado e hub que vai utilizar na produção. Não tente carregar diferentes tipos de certificados para o mesmo hub. Isto poderá originar falhas de notificação. 
    
    Se carregar inadvertidamente diferentes tipos de certificados para o mesmo hub, recomendamos que elimine o hub e começar do início com um novo hub. Se por alguma razão não é possível eliminar o hub, no mínimo, tem de eliminar todos os registos existentes do hub. 

* **Configuração do FCM** 
   
    1. Certifique-se de que o *chave do servidor* que obteve do Firebase corresponde a chave de servidor registado no portal do Azure.
   
    ![Chave de servidor firebase][3]
   
    2. Certifique-se de que configurou **ID de projeto** no cliente. Pode obter o valor para **ID de projeto** partir do Firebase dashboard.
   
    ![ID de projeto firebase][1]

## <a name="application-issues"></a>Problemas de aplicações
* **As etiquetas e expressões de etiqueta**

    Se utilizar etiquetas ou expressões de etiqueta para segmentar o público-alvo, é possível que enviar a notificação, nenhum destino encontra-se com base no etiquetas ou expressões de etiqueta que especificou na sua chamada de envio. 
    
    Reveja os registos para se certificar de que existem etiquetas correspondentes ao enviar uma notificação. Em seguida, certifique-se a receção de notificações apenas a partir de clientes que têm esses registos. 
    
    Por exemplo, se todos os seus registos com os Notification Hubs efetuados utilizando a tag "Política" e enviar uma notificação com a etiqueta "Desporto", a notificação não é enviada a todos os dispositivos. Caso complexo poderão envolver expressões de etiqueta no qual registado utilizando "A etiqueta" ou "Tag B", mas durante o envio de notificações, de destino "Tag A e e etiqueta B." Na secção de sugestões de self-diagnosis mais tarde no artigo, vamos mostrar-lhe como rever os registos e as respetivas etiquetas. 

* **Problemas de modelo**

    Se utilizar modelos, certifique-se de que siga as diretrizes descritas [modelos]. 

* **Registos inválidos**

    Se o hub de notificação foi configurado corretamente e se as etiquetas expressões de etiqueta foram utilizadas ou corretamente, encontram-se destinos válidos. As notificações devem ser enviadas para nestes destinos. Os Notification Hubs, em seguida, é desencadeado desativar várias de processamento de lotes em paralelo. Cada lote envia mensagens para um conjunto de registos. 

    > [!NOTE]
    > Porque o processamento é executado em paralelo, a ordem pela qual as notificações são entregues não é garantida. 

    Hubs de notificação está otimizada para um modelo de entrega de mensagem de "no máximo uma vez". Estamos a tentativa de eliminação de duplicados, para que não existem notificações são fornecidas mais do que uma vez num dispositivo. Para garantir que isto, verifique os registos e certifique-se de que apenas um é enviado pelo identificador do dispositivo antes da mensagem é enviada para o serviço de notificação push. 

    Como cada lote é enviado para o serviço de notificação push, que por sua vez está a aceitar e validar os registos, é possível que o serviço de notificação push irá detetar um erro com um ou mais dos registos num batch. Neste caso, o serviço de notificação push devolve um erro para os Notification Hubs e interrompe o processo. O serviço de notificação push ignora esse batch completamente. Isto é especialmente verdade com APNS, o que utiliza um protocolo de fluxo TCP. 

    Estão otimizados para no máximo uma vez entrega. No entanto, neste caso, o registo faulting é removido da base de dados. Em seguida, iremos Repita entrega da notificação para o resto dos dispositivos que batch.

    Para obter mais informações de erro sobre a tentativa falhada de entrega em relação a um registo, pode utilizar as APIs REST do Notification Hubs [por mensagem de telemetria: obter telemetria de mensagem de notificação](https://msdn.microsoft.com/library/azure/mt608135.aspx) e [comentários PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Para o código de exemplo, consulte o [exemplo enviar REST](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Problemas de notificação do serviço de push
Depois da mensagem de notificação foi recebida pelo serviço de notificação push de plataforma, é da responsabilidade do serviço de notificações push para fornecer a notificação para o dispositivo. Neste momento, os Hubs de notificação está fora da imagem e não tem nenhum controlo sobre quando ou se a notificação é enviada para o dispositivo. 

Uma vez que os serviços de notificação de plataforma são robustos, notificações têm tendência para chegarem aos dispositivos do serviço de notificação push dentro de alguns segundos. Se o serviço de notificação push é a limitação, os Notification Hubs aplica-se uma estratégia de término exponencial. Se o serviço de notificação push permanece inacessível durante 30 minutos, temos uma política no local para expirar e remover permanentemente as mensagens. 

Se um serviço de notificação push tentativas enviar uma notificação, mas o dispositivo estiver offline, a notificação é armazenada pelo serviço de notificação push para um período de tempo limitado. A notificação é entregue ao dispositivo quando o dispositivo fica disponível. 

Para cada aplicação, apenas uma notificação recente é armazenada. Se várias notificações são enviadas enquanto um dispositivo estiver offline, cada nova notificação faz com que a notificação anterior sejam eliminadas. Manter apenas a notificação mais recente é referido como *notificações agregação* no APNs, e *collapsing* no FCM (que utiliza uma chave collapsing). Se o dispositivo irá permanecer offline durante muito tempo, são eliminadas quaisquer notificações que foram que está a ser armazenadas para o dispositivo. Para obter mais informações, consulte [descrição geral do APNs] e [mensagens sobre FCM].

Notification hubs do Azure, pode passar uma chave de agregação através de um cabeçalho de HTTP utilizando a API de SendNotification genérico. Por exemplo, para o SDK .NET, utilizaria **SendNotificationAsync**. A API de SendNotification também demora cabeçalhos HTTP que são transmitidos como-é para o serviço de notificação push correspondentes. 

## <a name="self-diagnosis-tips"></a>Sugestões de Self-Diagnosis
Seguem-se os caminhos para diagnosticar a causa de raiz de notificações ignoradas num Hubs de notificação:

### <a name="verify-credentials"></a>Verificar credenciais
* **Portal de Programador de serviço de notificação Push**
   
    Certifique-se as credenciais nas respetivas push notification service portal do programador (do APNs, FCM, serviço de notificação do Windows e assim sucessivamente). Para obter mais informações, consulte [começar com Notification Hubs do Azure].

* **Portal do Azure**
   
    Para rever e corresponder às credenciais com as que obteve do push notification service portal do programador, no portal do Azure, visite o **políticas de acesso** separador. 
   
    ![Portal do Azure, as políticas de acesso][4]

### <a name="verify-registrations"></a>Verifique os registos
* **Visual Studio**
   
    Se utilizar o Visual Studio para desenvolvimento, pode ligar ao Azure através do Explorador de servidores para ver e gerir vários serviços do Azure, incluindo os Hubs de notificação. Isto é principalmente útil para o seu ambiente de desenvolvimento/teste. 
   
    ![Explorador de servidores do Visual Studio][9]
   
    Pode ver e gerir todos os registos do seu hub, categorizado pela plataforma, de modo nativa ou registo de modelo, qualquer etiquetas, identificador de serviço de notificação push, ID de registo e data de expiração. Também pode editar um registo nesta página. Isto é especialmente útil para editar etiquetas. 
   
    ![Registos de dispositivo do Visual Studio][8]
   
   > [!NOTE]
   > Utilize o Visual Studio para editar registos apenas durante o desenvolvimento/teste e com um número limitado de registos. Se precisar de editar os registos em volume, considere utilizar a exportação e importação funcionalidade de registo descrita em [exportar e modificar os registos em massa](https://msdn.microsoft.com/library/dn790624.aspx).
   > 
   > 
* **Explorador do Service Bus**
   
    Utilizam muitos clientes [Explorador do Service Bus] para ver e gerir o seu hub de notificação. Explorador do Service Bus é um projeto de fonte aberta. Para exemplos, consulte [código do Explorador do Service Bus].

### <a name="verify-message-notifications"></a>Certifique-se de notificações de mensagens
* **Portal do Azure**
   
    Enviar uma notificação de teste para os seus clientes sem ter um serviço de back-end e funcional, em **suporte + resolução de problemas**, selecione **teste de envio**. 
   
    ![Testar a funcionalidade de envio no Azure][7]
* **Visual Studio**
   
    Também pode enviar notificações de teste a partir do Visual Studio.
   
    ![Testar a funcionalidade de envio no Visual Studio][10]
   
    Para obter mais informações sobre como utilizar os Notification Hubs com o Explorador de servidores do Visual Studio, consulte estes artigos: 
   
   * [Ver registos de dispositivos para os notification hubs]
   * [Descrição detalhada da: Visual Studio 2013 atualização 2 RC e o Azure SDK 2.3]
   * [Anunciar o lançamento do Visual Studio 2013 atualização 3 e o Azure SDK 2.4]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Depurar falhas notificações e reveja o resultado da notificação
**Propriedade de EnableTestSend**

Enviar uma notificação através de Notification Hubs, inicialmente, a notificação é colocada na fila para processamento nos Notification Hubs. Hubs de notificação determina os destinos corretos e, em seguida, envia a notificação para o serviço de notificação push. Se estiver a utilizar a API REST ou de qualquer um dos SDKs de cliente, o retorno com êxito a chamada de envio apenas significa que a mensagem tem sido com êxito em fila com os Notification Hubs. Não tem qualquer aprofundadas sobre o que aconteceu quando os Hubs de notificação enviados, eventualmente, a mensagem para o serviço de notificação push. 

Se a sua notificação não chegam a dispositivo cliente, é possível que ocorreu um erro quando os Notification Hubs tentou entregar a mensagem para o serviço de notificação push. Por exemplo, o tamanho do payload poderá exceder o máximo permitido pelo serviço de notificação push, ou as credenciais configuradas no Notification Hubs poderão ser inválidas. 

Para conhecer os push erros do serviço de notificação, pode utilizar o [EnableTestSend] propriedade. Esta propriedade é ativada automaticamente quando enviar mensagens de teste do portal ou de cliente do Visual Studio. Pode utilizar esta propriedade para ver informações detalhadas de depuração. Também pode utilizar a propriedade através de APIs. Atualmente, pode utilizar o SDK .NET. Eventualmente, será adicionado a todos os SDKs do cliente. 

Para utilizar o **EnableTestSend** propriedade com chamada REST, acrescentar um parâmetro de cadeia de consulta denominado *testar* ao fim da chamada de envio. Por exemplo: 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

**Exemplo (.NET SDK)**

Eis um exemplo de como utilizar o SDK .NET para enviar uma notificação de pop-up de nativo (alerta):

```csharp
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
```

No final da execução de **resultado. Estado** simplesmente Estados **colocados em fila**. Os resultados não fornecem quaisquer informações sobre o que aconteceu ao seu notificação push. 

Em seguida, pode utilizar o **EnableTestSend** propriedade booleana. Utilize o **EnableTestSend** propriedade enquanto a inicializar **NotificationHubClient** obter um Estado de detalhado sobre push erros de serviço de notificação que ocorrem quando a notificação é enviada. A chamada de envio demora mais tempo para devolver porque é devolvido apenas depois dos Hubs de notificação tem entregar a notificação para o serviço de notificação push para determinar o resultado. 

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

**Saída de exemplo**

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Esta mensagem indica que a credenciais inválidas estão configuradas nos Notification Hubs ou há um problema com os registos no hub. Recomendamos que elimine este registo e permitir que o cliente volte a criar o registo antes de enviar a mensagem. 

> [!NOTE]
> Utilizar o **EnableTestSend** propriedade descontos elevados é limitada. Utilize esta opção apenas num ambiente de desenvolvimento/teste e com um conjunto limitado de registos. Iremos enviar notificações de depuração para apenas 10 dispositivos. Além disso, temos um limite de processamento envia de depuração para 10 por minuto. 
> 
> 

### <a name="review-telemetry"></a>Reveja a telemetria
* **Utilizar o portal do Azure**
   
    No portal, pode obter uma descrição geral de todas as atividades do seu hub de notificação. 
   
    1. No **descrição geral** separador, pode ver uma vista de agregados de registos, notificações e erros pela plataforma. 
   
        ![Dashboard de descrição geral de Hubs de notificação][5]
   
    2. No **Monitor** separador, pode adicionar várias outras métricas específicas da plataforma para ver mais aprofundado. Pode observar especificamente quaisquer erros relacionados com o serviço de notificação push que são devolvidos quando tenta Notification Hubs enviar a notificação para o serviço de notificação push. 
   
        ![Registo de atividade de portal do Azure][6]
   
    3. Comece por rever **mensagens a receber**, **operações de registo**, e **notificações com êxito**. Em seguida, aceda ao separador por plataforma para rever erros que são específicos para o serviço de notificação push. 
   
    4. Se as definições de autenticação para o seu hub de notificação estão incorretas, a mensagem **erro de autenticação PNS** aparece. Este é um bom indicador para verificar as credenciais de serviço de notificação push. 

* **Acesso programático**

    Para obter mais informações sobre acesso programático, consulte estes artigos: 

    * [Acesso programático telemetria]  
    * [Acesso de telemetria através de exemplo APIs] 

    > [!NOTE]
    > Várias funcionalidades relacionadas com a telemetria, como a exportação e importação de registos e acesso de telemetria através de APIs, estão disponíveis apenas na camada de serviço Standard. Se tentar utilizar estas funcionalidades de gratuito ou básico de camada de serviço, receberá uma mensagem de exceção, se utilizar o SDK e um erro de HTTP 403 (proibido), se utilizar as funcionalidades diretamente a partir de REST APIs. 
    >
    >Para utilizar as funcionalidades relacionadas com a telemetria, primeiro certifique-se no portal do Azure que está a utilizar a camada de serviço Standard.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/FCMConfigure.png
[3]: ./media/notification-hubs-diagnosing/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[descrição geral dos Notification Hubs]: notification-hubs-push-notification-overview.md
[começar com Notification Hubs do Azure]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[modelos]: https://msdn.microsoft.com/library/dn530748.aspx 
[descrição geral do APNs]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[mensagens sobre FCM]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: http://msdn.microsoft.com/library/dn790624.aspx
[Explorador do Service Bus]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[código do Explorador do Service Bus]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Ver registos de dispositivos para os notification hubs]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Descrição detalhada da: Visual Studio 2013 atualização 2 RC e o Azure SDK 2.3]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Anunciar o lançamento do Visual Studio 2013 atualização 3 e o Azure SDK 2.4]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Acesso programático telemetria]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Acesso de telemetria através de exemplo APIs]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

