---
title: Painel de definições do Application Insights Profiler do Azure | Documentos da Microsoft
description: Ver estado do criador de perfil e iniciar sessões de criação de perfis
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: b656fb521ad72256e91de63e96aa261f1e94bd13
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083446"
---
# <a name="configure-application-insights-profiler"></a>Configurar Application Insights Profiler

## <a name="profiler-settings-page"></a>Página de definições do Profiler

A página de definições do criador de perfil pode ser aberta a partir da página de desempenho do Application Insights ao premir o **Profiler** botão.

![configurar a entrada do painel de criador de perfil][configure-profiler-entry]

A página de configurar o Application Insights Profiler contém quatro recursos: 
1. **Perfil agora** -clicar neste botão, fará com que sessões de criação de perfis iniciar para todas as aplicações que estejam ligadas a esta instância do Application Insights
1. **Ligado aplicações** -lista de aplicativos de envio de criador de perfil para este recurso do Application Insights
1. **Sessões em curso** - quando pressiona **perfil agora**, o estado da sessão serão apresentadas aqui)
1. **Recentes de sessões de criação de perfis** -mostra informações sobre sessões de criação de perfis nos últimos.

![Profiler sob demanda][profiler-on-demand]

## <a name="app-service-environments-ase"></a>Ambientes de serviço de aplicações (ASE)
Dependendo de como o seu ASE é configurado, a chamada para verificar o estado do agente pode ser bloqueada. Esta página será apresentada a mensagem de que o agente não está em execução quando na verdade é. Para garantir que pode verificar o webjob em seu aplicativo. Mas se todas as definições de aplicação estão definidas corretamente e a extensão de site do App Insights está instalada no seu aplicativo, o criador de perfil será executado e verá recentes sessões de criação de perfil na lista, se houver tráfego adequado para a aplicação.

## <a id="profileondemand"></a> Acionar manualmente o Profiler

Profiler pode ser acionada manualmente com o clique de um botão. Suponha que está executando um teste de desempenho da web. Precisará rastreios para ajudar a compreender sobre a execução da sua aplicação web sob carga. É crucial ter controlo sobre o quando os rastreios são capturados, uma vez que sabe quando executará o teste de carga, mas o intervalo de amostragem aleatória pode perdê-lo.
Os passos seguintes mostram como funciona este cenário:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Opcional) Passo 1: Gerar tráfego para a sua aplicação web ao iniciar um teste de desempenho da web

Se a sua aplicação web já tiver o tráfego de entrada ou se quiser gerar tráfego manualmente, ignore esta secção e continue no passo 2.

Navegue até ao portal do Application Insights **configurar > testes de desempenho**. Clique no botão de nova para iniciar um novo teste de desempenho.

![Criar novo teste de desempenho][create-performance-test]

Na **novo teste de desempenho** painel, configurar o URL de destino do teste. Aceite todas as configurações padrão e comece a executar o teste de carga.

![Configurar teste de carga][configure-performance-test]

Verá que o novo teste é colocada na fila em primeiro lugar, seguido de um Estado de "em curso".

![teste de carga é submetido e colocados em fila][load-test-queued]

![teste de carga está em execução em curso][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Passo 2: Iniciar o gerador de perfis sob demanda

Assim que estiver a executar o teste de carga, podemos começar a profiler para capturar rastreios na aplicação web, enquanto ele está recebendo a carga.
Navegue até ao painel Configurar Profiler:


### <a name="step-3-view-traces"></a>Passo 3: Ver rastreios

Assim que o criador de perfil termina a execução, siga as instruções na notificação para aceder à rastreios de página e vista de desempenho.

## <a name="troubleshooting-on-demand-profiler"></a>Criador de perfil sob demanda de resolução de problemas

Às vezes, poderá ver a mensagem de erro de tempo limite do Profiler depois de uma sessão a pedido:

![Erro de tempo limite do Profiler][profiler-timeout]

Pode haver dois motivos por que vir este erro:

1. A sessão de criador de perfil sob demanda foi concluída com êxito, mas demorou mais tempo a processar os dados recolhidos do Application Insights. Se os dados não foi concluída a ser processado em 15 minutos, o portal irá apresentar uma mensagem de tempo limite. Embora após algum tempo, rastreios do Profiler serão exibido. Se isto acontecer, simplesmente ignore a mensagem de erro por enquanto. Estamos a trabalhar ativamente numa solução.

1. A aplicação web tem uma versão mais antiga do agente do Profiler que não tenha a funcionalidade sob demanda. Se ativou anteriormente o perfil do Application Insights, as chances são de que tem de atualizar o agente do Profiler para começar a utilizar a funcionalidade sob demanda.
  
Siga estes passos para verificar e instalar o Profiler mais recente:

1. Aceda às definições de aplicação de serviços de aplicação e verificar se as seguintes definições estão configuradas:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Substitua a chave de instrumentação adequada para o Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 se qualquer uma destas definições não se encontram definidos, vá para o painel de ativação do Application Insights para instalar a extensão de site mais recente.

1. Aceda ao painel do Application Insights no portal dos serviços de aplicações.

    ![Ativar o Application Insights a partir do portal dos serviços de aplicações][enable-app-insights]

1. Se vir um botão 'Update' na página seguinte, clique nele para atualizar a extensão de site do Application Insights que vai instalar o agente mais recente do Profiler.

    ![Atualizar a extensão de site][update-site-extension]

1. Em seguida, clique em **alterar** para garantir que o Profiler está ativado e selecione **OK** para guardar as alterações.

    ![Alterar e salvar o app insights][change-and-save-appinsights]

1. Volte ao **as definições da aplicação** separador para o serviço de aplicações para verificar novamente os seguintes itens de definições de aplicação estão definidos:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Substitua a chave de instrumentação adequada para o application insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![definições de aplicações para o criador de perfil][app-settings-for-profiler]

1. Opcionalmente, verifique a versão de extensão e certificar-se de que não existe nenhuma atualização disponível.

    ![verificação de atualização da extensão][check-for-extension-update]

## <a name="next-steps"></a>Próximos Passos
[Ative Profiler e ver rastreios](profiler-overview.md ?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png