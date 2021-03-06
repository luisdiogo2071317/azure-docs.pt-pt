---
title: Application Insights para aplicações de web de Java que já estão em direto
description: Iniciar a monitorização de uma aplicação web que já está em execução no seu servidor
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/10/2016
ms.author: mbullwin
ms.openlocfilehash: dbc16718358f86b02c6970ec42c4bc0ff68a951d
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301048"
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Application Insights para aplicações de web de Java que já estão em direto

Se tiver uma aplicação web que já está em execução no seu servidor de Java EE, pode iniciar a monitorização com [Application Insights](../../azure-monitor/app/app-insights-overview.md) sem a necessidade de fazer alterações de código nem recompilar o projeto. Com esta opção, obtém informações sobre os pedidos HTTP enviados para o seu servidor, exceções sem tratamento e contadores de desempenho.

Precisará de uma subscrição do [Microsoft Azure](https://azure.com).

> [!NOTE]
> O procedimento nesta página adiciona o SDK à sua aplicação web em tempo de execução. Essa instrumentação do tempo de execução é útil se não quiser atualizar ou recriar seu código-fonte. Mas se puder, recomendamos que [adicione o SDK ao código-fonte](java-get-started.md) em vez disso. Isso dá mais opções, como escrever código para controlar a atividade do utilizador.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Obter uma chave de instrumentação do Application Insights
1. Entrar para a [portal do Microsoft Azure](https://portal.azure.com)
2. Criar um novo recurso do Application Insights e defina o tipo de aplicação para a aplicação web Java.
   
    ![Preencher um nome, escolher aplicação Web em Java e clicar em criar](./media/java-live/02-create.png)

    O recurso é criado dentro de alguns segundos.

4. Abra o recurso novo e obtenha a chave de instrumentação. Terá de colar esta chave no seu projeto de código em breve.
   
    ![Na descrição geral do novo recurso, clique em Propriedades e copie a Chave de Instrumentação](./media/java-live/03-key.png)

## <a name="2-download-the-sdk"></a>2. Transferir o SDK
1. Transfira o [Application Insights SDK para Java](https://aka.ms/aijavasdk). 
2. No seu servidor, extraia os conteúdos SDK para o diretório a partir do qual seus binários de projeto são carregados. Se estiver a utilizar o Tomcat, este diretório normalmente seria em `webapps/<your_app_name>/WEB-INF/lib`

Tenha em atenção que terá de repetir isso em cada instância de servidor e para cada aplicação.

## <a name="3-add-an-application-insights-xml-file"></a>3. Adicionar um arquivo xml do Application Insights
Crie o applicationinsights. xml na pasta onde adicionou o SDK. Coloque no seguinte XML.

Substitua a chave de instrumentação que recebeu do portal do Azure.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* A chave de instrumentação é enviada juntamente com todos os itens de telemetria e diz ao Application Insights para apresentá-la no seu recurso.
* O componente de Pedido HTTP é opcional. Envia automaticamente telemetria sobre pedidos e tempos de resposta para o portal.
* A correlação de eventos é uma adição ao componente de pedido HTTP. Atribui um identificador a cada pedido recebido pelo servidor e adiciona este identificador como uma propriedade a todos os itens de telemetria como a propriedade "Operation.Id". Permite-lhe correlacionar a telemetria associada a cada pedido, definindo um filtro em [pesquisa de diagnóstico](../../azure-monitor/app/diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. Adicionar um filtro HTTP
Localize e abra o ficheiro Web. no seu projeto e intercale o seguinte fragmento de código sob o nó da aplicação web, onde os filtros da aplicação estão configurados.

Para obter resultados mais exatos, o filtro deve ser mapeado antes de todos os outros filtros.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a>5. Exceções de firewall de verificação
Talvez seja preciso [definir exceções para enviar dados de saída](../../azure-monitor/app/ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Reinicie a aplicação web
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Ver a telemetria no Application Insights
Regresse ao seu recurso do Application Insights no [Portal do Microsoft Azure](https://portal.azure.com).

Telemetria sobre pedidos HTTP é apresentada no painel de descrição geral. (Se não aparecerem, aguarde alguns segundos e, em seguida, clique em Atualizar.)

![dados de exemplo](./media/java-live/5-results.png)

Clique em qualquer gráfico para ver métricas mais detalhadas. 

![](./media/java-live/6-barchart.png)

E, ao visualizar as propriedades de um pedido, pode ver os eventos de telemetria associados, como os pedidos e exceções.

![](./media/java-live/7-instance.png)

[Saiba mais sobre métricas.](../../azure-monitor/app/metrics-explorer.md)

## <a name="next-steps"></a>Passos Seguintes
* [Adicione a telemetria às suas páginas da web](javascript.md) para monitorizar vistas de página e métricas de utilizador.
* [Configure testes web](../../azure-monitor/app/monitor-web-app-availability.md) para certificar-se de que a aplicação permanece em direto e reativa.
* [Capturar rastreios de registos](java-trace-logs.md)
* [Pesquise eventos e registos](../../azure-monitor/app/diagnostic-search.md) para ajudar a diagnosticar problemas.
* [Configurar uma aplicação do inicializador de Spring Boot](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)
