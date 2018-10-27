---
title: Enviar o contexto de usuário IDs para ativar a utilização de experiências no Application Insights do Azure | Documentos da Microsoft
description: Controle como os utilizadores passam por meio do seu serviço atribuindo uma cadeia de ID exclusiva, persistente no Application Insights para cada um deles.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/02/2017
ms.reviewer: abgreg;mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: bfb04f596a61ff79c75cd38473c9480a29b0e6c4
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139895"
---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Enviar o contexto de utilizador IDs para permitir experiências de utilização no Azure Application Insights

## <a name="tracking-users"></a>Usuários de rastreamento

O Application Insights permite-lhe monitorizar e controlar os seus utilizadores através de um conjunto de ferramentas de utilização de produto: 
* [Utilizadores, Sessões, Eventos](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Funis](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Retenção](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Coortes
* [Livros](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Para poder controlar o que faz um utilizador ao longo do tempo, o Application Insights tem um ID para cada utilizador ou a sessão. Inclua os seguintes IDs de em cada vista de evento ou página personalizada.
- Os utilizadores, funis, retenção e coortes: incluem o ID de utilizador.
- Sessões: Incluem o ID de sessão.

Se a sua aplicação está integrada com o [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), utilizador ID é controlado automaticamente.

## <a name="choosing-user-ids"></a>Escolher os IDs de utilizador

IDs de utilizador devem manter em sessões de utilizador para controlar o comportam de utilizadores ao longo do tempo. Existem várias abordagens para persistir o ID.
- Uma definição de um utilizador que já tem no seu serviço.
- Se o serviço tem acesso a um browser, que pode transmitir o navegador um cookie com um ID na mesma. O ID serão mantidas para, desde que o cookie permanece no navegador do usuário.
- Se necessário, pode utilizar um novo ID de cada sessão, mas os resultados sobre os utilizadores serão limitados. Por exemplo, não será capaz de ver como o comportamento de um utilizador altera ao longo do tempo.

O ID deve ser um Guid ou outra cadeia suficientemente complexa para identificar exclusivamente a cada utilizador. Por exemplo, pode ser um número aleatório muito tempo.

Se o ID contém informações de identificação pessoal sobre o utilizador, não é um valor adequado para enviar para o Application Insights como um ID de utilizador. Pode enviar esse um ID como um [autenticado o ID de utilizador](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), mas não cumpre o requisito de ID de utilizador para cenários de utilização.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>Aplicações do ASP.NET: definir o contexto de usuário num ITelemetryInitializer

Criar um inicializador de telemetria, conforme descrito em detalhe [aqui](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer)e defina o Context.User.Id e o Context.Session.Id.

Este exemplo define o ID de utilizador como um identificador que expira após a sessão. Se possível, utilize um ID de utilizador que persiste entre sessões.

```csharp

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Passos Seguintes
- Para permitir experiências de utilização, começar a enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [vistas de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se já tiver enviar eventos personalizados ou vistas de página, explore as ferramentas de utilização para saber como os utilizadores utilizam o seu serviço.
    * [Descrição geral da utilização](app-insights-usage-overview.md)
    * [Os utilizadores, sessões e eventos](app-insights-usage-segmentation.md)
    * [Funis](usage-funnels.md)
    * [Retenção](app-insights-usage-retention.md)
    * [Livros](app-insights-usage-workbooks.md)
