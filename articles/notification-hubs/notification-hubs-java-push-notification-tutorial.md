---
title: Como utilizar os Notification Hubs com Java
description: Saiba como utilizar Notification Hubs do Azure de um back-end de Java.
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 88e3ab3cc03cc1e760672120bc5c484af1ba4722
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33778380"
---
# <a name="how-to-use-notification-hubs-from-java"></a>Como utilizar os Hubs de notificação de Java
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Este tópico descreve as funcionalidades principais do novo totalmente suportados oficial Azure Notification Hub Java SDK. Este projeto é um projeto de fonte aberta e pode ver o código SDK todo no [Java SDK]. 

Em geral, pode aceder a todas as funcionalidades de Hubs de notificação de um back-end Java/PHP/Python/Ruby utilizando a interface REST do Notification Hub, conforme descrito no tópico do MSDN [APIs REST do Notification Hubs](http://msdn.microsoft.com/library/dn223264.aspx). Este SDK de Java fornece um wrapper dinâmico estas interfaces REST em Java. 

O SDK suporta atualmente:

* CRUD em Hubs de notificação 
* CRUD nos registos
* Gestão de instalação
* Importar/Exportar registos
* Envia regular
* Envia agendada
* Operações assíncronas através de NIO de Java
* Plataformas suportadas: APNS (iOS), GCM (Android), o WNS (aplicações da loja Windows), MPNS (Windows Phone), ADM (Amazon Kindle Fire), o Baidu (Android sem serviços do Google) 

## <a name="sdk-usage"></a>Utilização do SDK
### <a name="compile-and-build"></a>Compilação e compilação
Utilize [Maven]

Para criar:

    mvn package

## <a name="code"></a>Código
### <a name="notification-hub-cruds"></a>CRUDs de Hub de notificação
**Crie um NamespaceManager:**

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Crie o Hub de notificação:**

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 OU

    hub = new NotificationHub("connection string", "hubname");

**Obter o Hub de notificação:**

    hub = namespaceManager.getNotificationHub("hubname");

**Hub de notificação de atualização:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Elimine o Hub de notificação:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Registo CRUDs
**Crie um Hub de notificação de cliente:**

    hub = new NotificationHub("connection string", "hubname");

**Crie registo do Windows:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**Crie registo de iOS:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

Da mesma forma, pode criar registos para Android (GCM), Windows Phone (MPNS) e Fire Kindle (ADM).

**Crie registos de modelo:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Criar registos ao utilizar a criar o ID de registo + upsert padrão**

Remove os duplicados devido a qualquer respostas perdidas se armazenar os IDs de registo do dispositivo:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Atualize registos:**

    hub.updateRegistration(reg);

**Elimine registos:**

    hub.deleteRegistration(regid);

**Registos de consulta:**

* **Obter registo único:**
  
        hub.getRegistration(regid);

* **Obter todos os registos no hub:**
  
        hub.getRegistrations();

* **Obter registos com etiqueta:**
  
        hub.getRegistrationsByTag("myTag");

* **Obter registos por canal:**
  
        hub.getRegistrationsByChannel("devicetoken");


Todas as consultas de coleção suportam tokens $top e continuação.

### <a name="installation-api-usage"></a>Utilização de instalação API
API de instalação é um mecanismo alternativo para a gestão de registo. Em vez de manutenção de vários registos, que não são trivial e podem ser feitos facilmente erradamente ou inefficiently, agora é possível utilizar um objeto de instalação única. Instalação contém tudo o que necessita: push canal (token do dispositivo), as etiquetas, modelos, mosaicos secundários (para WNS e APNS). Não tem de chamar o serviço para obter o ID já - apenas gerar GUID ou qualquer outro tipo de identificador, mantenha-a no dispositivo e enviar para o seu back-end, juntamente com o canal de emissão (token do dispositivo). No back-end, só deve fazer uma única chamada: CreateOrUpdateInstallation, este fica totalmente idempotent, por isso, não hesite se necessário, repita.

Como exemplo Amazon Kindle Fire:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Se pretende atualizá-lo: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Para cenários avançados, utilize a capacidade de atualização parcial, o que permite modificar propriedades só específicas do objeto de instalação. Atualização parcial é o subconjunto de operações de Patch de JSON, que pode executar o objeto de instalação.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Elimine a instalação:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, Patch e Delete são eventualmente consistente com Get. A operação pedida apenas vai para a fila de sistema durante a chamada e é executada em segundo plano. Get não foi concebido para o cenário de tempo de execução principal, mas apenas para depuração e resolução de problemas, é totalmente limitado pelo serviço.

Fluxo de envio para instalações de é igual aos registos. Destino de notificação para uma determinada instalação-apenas utilizar a tag "InstallationId: {desired-id}". Neste caso, o código é:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

Para um dos vários modelos:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Agendar notificações (disponíveis para o escalão STANDARD)
O mesmo como enviar regular mas com um parâmetro adicional - scheduledTime, que indica quando deve ser entregue notificação. Serviço aceita qualquer ponto de tempo entre agora + 5 minutos e agora + 7 dias.

**Agende uma notificação nativa do Windows:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>Importar/exportar (disponível para o escalão STANDARD)
Por vezes, é necessário para efetuar a operação em massa em relação a registos. Normalmente, é para a integração com outro sistema ou apenas uma correção em massa e diga Atualize as etiquetas. Não se recomenda utilizar fluxo Get/atualização se estiverem envolvidos milhares de registos. Capacidade de importação/exportação foi concebida para o cenário. Basicamente é fornecer um acesso a algumas contentor de BLOBs na sua conta de armazenamento como uma origem de dados recebidos e a localização de saída.

**Submeta a tarefa de exportação:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**Submeta a tarefa de importação:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**Aguarde até que a tarefa é concluída:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Obter todas as tarefas:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**URI com assinatura SAS:** este URL é o URL de alguns ficheiros de blob ou o contentor de blob e o conjunto de parâmetros, tais como permissões e a hora de expiração e assinatura de todos os estas ações efetuadas utilizando a chave SAS da conta. O SDK de Java de armazenamento do Azure tem capacidades avançadas, incluindo a criação desse tipo de URIs. Como alternativa simple que pode tomar uma vista de olhos ImportExportE2E classe de teste (a partir da localização do github) que tem uma implementação básica e compacta do algoritmo de assinatura.

### <a name="send-notifications"></a>Enviar notificações
O objeto de notificação é simplesmente um corpo com cabeçalhos, alguns métodos de utilitário ajudam na criação de objetos de notificações nativo e modelo.

* **Loja Windows e Windows Phone 8.1 (não Silverlight)**
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* **iOS**
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* **Android**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* **Windows Phone 8.0 e 8.1 Silverlight**
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* **Kindle Fire**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **Enviar para etiquetas**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Enviar a expressão da etiqueta**       
  
        hub.sendNotification(n, "foo && ! bar");
* **Enviar notificação de modelo**
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Executar o seu código de Java deve agora produzir uma notificação de apresentação no seu dispositivo de destino.

## <a name="next-steps"></a>Passos Seguintes
Este tópico mostrou como criar um cliente de REST de Java simple para os Notification Hubs. A partir daqui, pode:

* Transferir o completo [Java SDK], que contém o código SDK completo. 
* Reproduzir com os exemplos:
  * [Introdução aos Hubs de notificação]
  * [Enviar notícias de última hora]
  * [Notícias de última hora envio localizado]
  * [Enviar notificações aos utilizadores autenticados]
  * [Enviar notificações de plataforma para os utilizadores autenticados]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Introdução aos Hubs de notificação]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Enviar notícias de última hora]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Notícias de última hora envio localizado]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Enviar notificações aos utilizadores autenticados]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Enviar notificações de plataforma para os utilizadores autenticados]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

