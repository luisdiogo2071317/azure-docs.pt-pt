---
title: Perfil de aplicações em direto do serviço de aplicações do Azure com o Application Insights | Documentos da Microsoft
description: Criar perfis para aplicações em direto no serviço de aplicações do Azure com o Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: f3ec10a970406cbb1bb6a1a52ffa8508e37fc516
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414172"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Perfil aplicações de serviço de aplicações do Azure em direto com o Application Insights

Profiler trabalha atualmente para aplicações ASP.NET e ASP.NET Core em execução no App Service do Azure. Basic camada de serviço ou superior é obrigatório para utilizar o Profiler. Ativar Profiler no Linux atualmente só é possível por meio [esse método](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Ative Profiler para a sua aplicação
Para ativar o Profiler para uma aplicação, siga as instruções abaixo. Se estiver a executar um tipo diferente de serviço do Azure, aqui estão as instruções de ativação do Profiler em outras plataformas suportadas:
* [Serviços Cloud](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Service Fabric](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler previamente é instalado como parte do tempo de execução dos serviços de aplicações, mas terá de ativá-la para perfis de get para a sua aplicação de serviço de aplicações. Assim que tiver implementado uma aplicação, mesmo se o ter incluído o SDK do App Insights no código-fonte, siga os passos abaixo para ativar o gerador de perfis.

1. Vá para o **dos serviços de aplicações** painel no portal do Azure.
2. Navegue para **definições > Application Insights** painel.

   ![Ativar o App Insights no portal dos serviços de aplicações](./media/profiler/AppInsights-AppServices.png)

3. Ou siga as instruções no painel para criar um novo recurso ou selecione um recurso do App Insights existente para monitorizar a aplicação. Além disso, certifique-se de que o Profiler está **no**.

   ![Adicionar extensão de site do App Insights][Enablement UI]

4. Profiler está agora ativada com uma definição de aplicação de serviços de aplicação.

    ![Definição de aplicação para Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Desativar Profiler

Para parar ou reiniciar Profiler para a instância de uma aplicação individual, em **Webjobs**, vá para o recurso de aplicação. Para eliminar o Profiler, aceda a **extensões**.

![Desativar Profiler para um trabalho web][disable-profiler-webjob]

Recomendamos que tenha Profiler ativada em todas as suas aplicações para detetar problemas de desempenho mais cedo possível.

Se usar o WebDeploy para implementar as alterações ao seu aplicativo web, certifique-se de que exclua a pasta App_Data seja eliminado durante a implementação. Caso contrário, os ficheiros da extensão de Profiler são eliminados da próxima vez em que implementou a aplicação web no Azure.



## <a name="next-steps"></a>Passos Seguintes

* [Trabalhar com o Application Insights no Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
