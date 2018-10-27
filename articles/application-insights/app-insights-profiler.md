---
title: Perfil live aplicações web do Azure com o Application Insights | Documentos da Microsoft
description: Aplicações web em direto de perfil no Azure com o Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 9aae08aa5906f341a890ac15e30d2863109d83a2
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140013"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Aplicações web do Azure em direto de perfil com o Application Insights

Profiler trabalha atualmente para aplicações de web ASP.NET e ASP.NET Core em execução em aplicações Web. Basic camada de serviço ou superior é obrigatório para utilizar o Profiler.

## <a id="installation"></a> Ative Profiler para as suas aplicações Web
Para ativar o Profiler para uma aplicação web, siga as instruções abaixo. Se estiver a executar um tipo diferente de serviço do Azure, aqui estão as instruções de ativação do Profiler em outras plataformas suportadas:
* [Serviços Cloud](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


Application Insights Profiler é instalado com a extensão de site do Application Insights. Tem de instalar a extensão de site e configurá-lo para obter os perfis para as suas aplicações Web do Azure. Assim que tiver implementado uma aplicação Web, mesmo se o ter incluído o SDK do App Insights no código-fonte, siga os passos abaixo para ativar o gerador de perfis.

1. Vá para o **dos serviços de aplicações** painel no portal do Azure.
1. Navegue para **definições > monitorização** painel.

   ![Ativar o App Insights no portal dos serviços de aplicações](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Ou siga as instruções no painel para criar um novo recurso ou selecione um recurso do App Insights existente para monitorizar a aplicação web. Aceite todas as opções padrão. **Diagnóstico ao nível de código** está ativada por predefinição e permite que o Profiler.

   ![Adicionar extensão de site do App Insights][Enablement UI]

1. Profiler está agora instalado com a extensão de site do App Insights e é habilitada com uma definição de aplicação de serviços de aplicação.

    ![Definição de aplicação para Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Desativar Profiler

Para parar ou reiniciar Profiler para a instância de uma aplicação de web individuais, em **Webjobs**, vá para o recurso de aplicações Web. Para eliminar o Profiler, aceda a **extensões**.

![Desativar Profiler para um trabalho web][disable-profiler-webjob]

Recomendamos que tenha Profiler ativada em todas as suas aplicações web para detetar problemas de desempenho mais cedo possível.

Se usar o WebDeploy para implementar as alterações ao seu aplicativo web, certifique-se de que exclua a pasta App_Data seja eliminado durante a implementação. Caso contrário, os ficheiros da extensão de Profiler são eliminados da próxima vez em que implementou a aplicação web no Azure.



## <a name="next-steps"></a>Passos Seguintes

* [Trabalhar com o Application Insights no Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

