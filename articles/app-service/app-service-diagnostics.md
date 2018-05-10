---
title: Descrição geral de diagnóstico do Azure App Service | Microsoft Docs
description: Saiba como pode resolver problemas com a sua aplicação web com o diagnóstico de serviço de aplicações.
keywords: serviço de aplicações, serviço de aplicações do azure, diagnóstico, suporte, aplicação web, de resolução de problemas, ajuda autónoma
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.openlocfilehash: 50e0e9f5edc18aac42ee80e232f70e09736124bc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-app-service-diagnostics-overview"></a>Descrição geral de diagnóstico de serviço de aplicações do Azure 

Quando estiver a executar uma aplicação web, que pretende ser preparado para quaisquer problemas que possam surgir, de 500 erros para os seus utilizadores informá-lo de que o site está inativo. Diagnóstico de serviço de aplicações é uma experiência interativa e inteligente para o ajudar a resolver problemas da aplicação de web sem qualquer configuração necessária. Quando depare com problemas com a sua aplicação web, diagnóstico de serviço de aplicações irá apontar o que é incorreto para ajudá-lo para as informações corretas para mais fácil e rápida resolver o problema. 
 
Apesar desta experiência mais úteis quando está a ter problemas com a sua aplicação web nas últimas 24 horas, todos os gráficos diagnóstico estarão disponíveis para que possa analisar permanente. Ferramentas adicionais de resolução de problemas e ligações para documentação útil e fóruns estão localizadas na coluna direita.

Diagnóstico de serviço de aplicações funciona para não só a aplicação no Windows, mas também as aplicações em [Linux/contentores](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro), [ambiente de serviço de aplicações](https://docs.microsoft.com/en-us/azure/app-service/environment/intro), e [das funções do Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview). 

## <a name="open-app-service-diagnostics"></a>Abra diagnóstico de serviço de aplicações

Para aceder ao diagnóstico do serviço de aplicações, navegue até à sua aplicação de serviço de aplicações ou o ambiente de serviço de aplicações no [portal do Azure](https://portal.azure.com). No painel de navegação esquerdo, clique em **diagnosticar e resolver problemas**. 

Para as funções do Azure, navegue para a sua aplicação de função e, no painel de navegação superior, clique em **funcionalidades da plataforma** e selecione **diagnosticar e resolver problemas** do **monitorização**secção. 

![Home page](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Checkup de estado de funcionamento

Se não souber o que é incorreto com a sua aplicação web ou não sabe onde iniciar os problemas de resolução de problemas, checkup o estado de funcionamento é um bom local para iniciar. O checkup de estado de funcionamento irá analisar as suas aplicações web para lhe dar uma descrição geral rápida, interativa que aponta para os quais está em bom estado e o que é o problema, informá-lo onde parecer para investigar o problema. A interface inteligente e interativa fornece orientação durante o processo de resolução de problemas.  

![Checkup de estado de funcionamento](./media/app-service-diagnostics/HealthCheckup2.png)

Se for detetado um problema com uma categoria de problema específico nas últimas 24 horas, pode ver o relatório de diagnóstico completa e diagnóstico do serviço de aplicações pode solicitar-lhe ver conselhos resolução de problemas mais e passos para uma experiência assistida mais.

![Passos de resolução de problemas e seguintes](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Atalhos de mosaico

Se souber exatamente que tipo de procura de informações de resolução de problemas, os atalhos de mosaico leva-o diretamente para o relatório de diagnóstico completa da categoria de problema que está interessado em. Em comparação com a checkup de estado de funcionamento, os atalhos de mosaico são a mais direta, mas inferior orientado forma de aceder a métricas de diagnóstico. Como parte de atalhos de mosaico, este é também onde encontrará **as ferramentas de diagnóstico** que mais avançado ferramentas que o irão ajudar a investigar problemas relacionados com problemas de código da aplicação, slowness, cadeias de ligação e muito mais. 

![Atalhos de mosaico](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Relatório de diagnóstico

Se quiser obter mais informações, após a execução um [checkup de estado de funcionamento](#health-checkup) ou clicar do [mosaico atalhos](#tile-shortcuts), o relatório de diagnóstico completa irá mostrar-lhe relevantes métricas graphed das últimas 24 horas. Se a sua aplicação experiências qualquer período de inatividade, é representada por uma barra laranja sob a linha cronológica. Pode selecionar uma das laranja barras para selecionar o período de indisponibilidade para ver as observações sobre esse período de inatividade e os passos de resolução de problemas sugeridos. 

![Relatório de diagnóstico](./media/app-service-diagnostics/DiagnosticReport5.png)


## <a name="investigating-application-code-issues"></a>Investigar problemas de código da aplicação

Porque muitos problemas de aplicação estão relacionadas com problemas no código da aplicação, diagnóstico de serviço de aplicações se integra com [Application Insights](https://azure.microsoft.com/services/application-insights/) para realçar as exceções e problemas de dependência para correlacionar com o período de indisponibilidade selecionado. Application Insights tem de ser ativada em separado. 

Para ver as exceções do Application Insights e dependências, selecione o **Web App para baixo** ou **lenta de aplicação Web** mosaico atalhos. 

![Application Insights](./media/app-service-diagnostics/AppInsights6.png)

