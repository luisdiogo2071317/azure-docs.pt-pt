---
title: 'Application Insights: linguagens, plataformas e integrações | Microsoft Docs'
description: Linguagens, plataformas e integrações disponíveis para o Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/01/2016
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 3fe57fe0aac4c355b65b55ee37213221eb854a5c
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976560"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Análise do programador: linguagens, plataformas e integrações
Estes itens são implementações do [Application Insights](app-insights-overview.md) que já conhecemos, incluindo alguns de terceiros.

## <a name="languages---officially-supported-by-application-insights-team"></a>Linguagens – oficialmente suportadas pela equipa do Application Insights
* [C#|VB (.NET)](../azure-monitor/app/asp-net.md)
* [Java](../azure-monitor/app/java-get-started.md)
* [Páginas Web JavaScript](../azure-monitor/app/javascript.md)
* [Node.JS](app-insights-nodejs.md)

## <a name="languages---community-supported"></a>Linguagens – com suporte da comunidade
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Tudo o resto](#projects)

## <a name="platforms-and-frameworks"></a>Plataformas e estruturas
* [ASP.NET](../azure-monitor/app/asp-net.md)
* [ASP.NET – para aplicações que já estão em direto](../azure-monitor/app/monitor-performance-live-website-now.md)
* [Núcleo do ASP.NET](../azure-monitor/app/asp-net-core.md)
* [Android](app-insights-mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Serviço de Aplicações do Azure](../azure-monitor/app/azure-web-apps.md)
* [Cloud Services do Azure](../azure-monitor/app/cloudservices.md): incluindo funções Web e funções de trabalho
* [Funções do Azure](https://github.com/christopheranderson/azure-functions-app-insights-sample)
* [Docker](../azure-monitor/app/docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](app-insights-mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [J2EE](../azure-monitor/app/java-get-started.md)
* [J2EE – para aplicações que já estão em direto](../azure-monitor/app/java-live.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [Pilha SEGURA](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](https://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Aplicação Universal do Windows](app-insights-mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Aplicações de ambiente de trabalho, serviços e funções de trabalho do Windows](app-insights-windows-desktop.md)
* [Tudo o resto](#projects)

## <a name="logging-frameworks"></a>Arquiteturas de registo
* [Log4Net, NLog ou System.Diagnostics.Trace](../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J ou Logback](../azure-monitor/app/java-trace-logs.md)
* [Semantic Logging (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) – integra-se no [Semantic Logging Application Block](https://msdn.microsoft.com/library/dn440729.aspx)
* [Teste de carga baseado na nuvem](https://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [Plug-in do LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)

## <a name="content-management-systems"></a>Sistemas de Gestão de Conteúdo
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Análise de Dados e Exportação
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](app-insights-export-power-bi.md)

## <a name="projects"></a> Criar o seu próprio SDK
Se ainda não tem um SDK para a sua linguagem ou plataforma, gostaria de criar um? Observe o código dos SDKs existentes listados no [Projeto Application Insights SDK no GitHub](https://github.com/Microsoft/AppInsights-Home).
