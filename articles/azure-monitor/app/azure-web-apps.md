---
title: Monitorizar o desempenho de serviços de aplicações do Azure | Documentos da Microsoft
description: Desempenho de aplicações, monitorização dos serviços de aplicações do Azure. Crie gráficos de tempos de carregamento e resposta, informações de dependências e defina alertas relativos ao desempenho.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: mbullwin
ms.openlocfilehash: 19e0e5797e05589baa1e104f3e9ab8b4d9cc2d6c
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267296"
---
# <a name="monitor-azure-app-service-performance"></a>Monitorizar o desempenho do serviço de aplicações do Azure
Na [portal do Azure](https://portal.azure.com) pode configurar a monitorização de desempenho de aplicações para as suas aplicações web, móveis back-ends e aplicações API no [App Service do Azure](../../app-service/overview.md). O [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) instrui a sua aplicação a enviar telemetria sobre as atividades para o serviço Application Insights, onde são armazenadas e analisadas. Aí, podem ser utilizados gráficos de métricas e ferramentas de pesquisa para ajudar a diagnosticar problemas, melhorar o desempenho e avaliar a utilização.

## <a name="run-time-or-build-time"></a>Hora de execução ou hora de compilação
Para configurar a monitorização, pode instrumentar a aplicação de uma de duas formas:

* **Tempo de execução** -pode selecionar uma extensão de monitorização, quando o serviço de aplicações já está em direto. Não é necessário recompilar ou reinstalar a aplicação. Obtém um conjunto padrão de pacotes que monitorizam os tempos de resposta, as taxas de êxito, as exceções, as dependências e assim sucessivamente. 
* **Tempo de compilação** - pode instalar um pacote na sua aplicação durante a programação. Esta opção é mais versátil. Para além dos mesmos pacotes padrão, pode escrever código para personalizar a telemetria ou enviar a sua própria telemetria. Pode registar atividades específicas ou eventos, de acordo com a semântica do domínio da aplicação. 

## <a name="run-time-instrumentation-with-application-insights"></a>Instrumentação do tempo de execução com o Application Insights
Se já estiver a executar um serviço de aplicações no Azure, já tem alguma monitorização: taxas de pedidos e de erros. Adicionar o Application Insights para beneficiar de mais, como tempos de resposta, monitorização de chamadas para dependências, deteção inteligente e a poderosa Kusto linguagem de consulta. 

1. **Selecione o Application Insights** no painel de controlo do Azure para o serviço de aplicações.

    ![Em definições, escolha Application Insights](./media/azure-web-apps/settings-app-insights.png)

   * Opte por criar um novo recurso, a menos que já configurou a um recurso do Application Insights para esta aplicação. 

    > [!NOTE]
    > Quando clica em **OK** para criar o novo recurso, será solicitado a **aplicar definições de monitorização**. Selecionando **continuar** irá ligar o seu novo recurso do Application Insights ao seu serviço de aplicações, fazendo assim, será também **acionar um reinício do serviço de aplicações**. 

    ![Instrumente a sua aplicação Web](./media/azure-web-apps/create-resource.png)

2. Depois de especificar o recurso a utilizar, pode escolher como pretende que o application insights para recolher dados por plataforma para a sua aplicação. (Monitorização de aplicações do ASP.NET é por padrão com dois níveis diferentes de coleção.)

    ![Escolher opções por plataforma](./media/azure-web-apps/choose-options-new.png)

    * .NET **conjunto básico** nível oferece funcionalidades essenciais de APM de instância única.
    
    * .NET **recomendado coleção** nível:
        * Adiciona as tendências de utilização de CPU, memória e e/s.
        * Correlaciona microsserviços em limites de pedido/dependência.
        * Recolhe as tendências de utilização e permite a correlação de resultados de disponibilidade para transações.
        * Recolhe as exceções não processadas pelo processo de host.
        * Melhora a precisão de métricas APM sob carga, quando a amostragem é usada.
    
    .NET core oferece **recomendado coleção** ou desativado para .NET Core 2.0 e 2.1.

3. **Instrumentar o seu serviço de aplicações** após instalação do Application Insights.

   **Ativar a monitorização do lado do cliente** para a telemetria de utilizador e vista de página.

    (Isto é ativado por predefinição para aplicações de .NET Core com **recomendado coleção**, independentemente de se a 'APPINSIGHTS_JAVASCRIPT_ENABLED' de definição de aplicação está presente. Suporte baseado em granular da interface do Usuário para a desativação da monitorização do lado do cliente não está atualmente disponível para .NET Core.)
    
   * Selecione Definições > Definições da Aplicação
   * Em Definições da Aplicação, adicione um par de chaves-valores novo:

    Chave: `APPINSIGHTS_JAVASCRIPT_ENABLED`

    Valor:`true`
   * **Guarde** as definições e **reinicie** a aplicação.

4. Explorar dados de monitorização da sua aplicação, selecionando **configurações** > **Application Insights** > **ver mais no Application Insights**.

Mais tarde, pode criar a aplicação com o Application Insights, se pretender.

*Como posso remover o Application Insights ou mudar para enviar para outro recurso?*

* No Azure, abra o painel de controlo de aplicação web e, em definições, abra **Application Insights**. Pode desativar o Application Insights clicando **desativar** na parte superior, ou selecione um novo recurso no **alterar seu recurso** secção.

## <a name="build-the-app-with-application-insights"></a>Compilar a aplicação com o Application Insights
O Application Insights pode proporcionar telemetria mais detalhada, mediante a instalação de um SDK na sua aplicação. Em particular, pode recolher registos de rastreio, [escrever telemetria personalizada](../../azure-monitor/app/api-custom-events-metrics.md)e obter relatórios de exceções mais detalhados.

1. **No Visual Studio** (2013 update 2 ou posterior), configure o Application Insights para o seu projeto.

    Com o botão direito no projeto web e selecione **adicionar > Application Insights** ou **projeto** > **Application Insights**  >  **Configurar o Application Insights**.

    ![Clique com o botão direito do rato no projeto e escolha Adicionar ou Configurar o Application Insights](./media/azure-web-apps/03-add.png)

    Se lhe for pedido para iniciar sessão, utilize as credenciais da sua conta do Azure.

    A operação tem dois efeitos:

   1. Cria um recurso do Application Insights no Azure, onde telemetria é armazenada, analisada e apresentada.
   2. Adiciona o pacote NuGet do Application Insights ao seu código (se ainda não estiver lá) e configura-o para enviar a telemetria para o recurso do Azure.
2. Execute a aplicação no seu computador de programação (F5) para **testar a telemetria**.
3. **Publique a aplicação** no Azure, da forma habitual. 

*Como posso mudar para enviar para outro recurso do Application Insights?*

* No Visual Studio, clique com botão direito no projeto, escolha **configurar o Application Insights**e escolha o recurso que pretende. Terá a opção de criar um recurso novo. Recompile e reimplemente.

## <a name="more-telemetry"></a>Mais telemetria

* [Web page load data](../../azure-monitor/app/javascript.md) (Dados de carregamento de páginas Web)
* [Custom telemetry](../../azure-monitor/app/api-custom-events-metrics.md) (Telemetria personalizada)

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED faz com que a resposta HTML incompleta em aplicativos de web do NET CORE.

Ativar Javascript por meio de serviços de aplicação pode fazer com que as respostas de html para serão cortadas.

* Solução 1: definir a definição de aplicação APPINSIGHTS_JAVASCRIPT_ENABLED como false ou removê-la completamente e reiniciar
* Solução 2: adicionar o sdk por meio do código e remover a extensão (o depurador Profiler e o instantâneo não com esta configuração)

Estamos a acompanhar este problema [aqui](https://github.com/Microsoft/ApplicationInsights-Home/issues/277)

Para .NET Core seguem-se atualmente **nepodporuje**:

* Implementação independente.
* Aplicações visando o .NET Framework.
* Aplicações de .NET core 2.2.

> [!NOTE]
> .NET core 2.0 e 2.1 do .NET Core são suportadas. Quando é adicionado suporte de .NET Core 2.2 neste artigo será atualizado.

## <a name="next-steps"></a>Passos Seguintes
* [Run the profiler on your live app](../../azure-monitor/app/profiler.md) (Executar o gerador de perfis na sua aplicação publicada).
* [Funções do Azure](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitorize Funções do Azure com o Application Insights
* [Ativar os diagnósticos do Azure](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) para serem enviados para o Application Insights.
* [Monitorizar as métricas de estado de funcionamento de serviço](../../azure-monitor/platform/data-collection.md) para se certificar de que o serviço está disponível e a responder.
* [Receber notificações de alertas](../../azure-monitor/platform/alerts-overview.md) sempre que ocorrem eventos operacionais ou quando as métricas ultrapassam um determinado limiar.
* Utilizar o [Application Insights para aplicações JavaScript e páginas Web](../../azure-monitor/app/javascript.md) para obter telemetria de clientes a partir dos browsers que visitam as páginas Web.
* [Configurar os Testes Web de disponibilidade](../../azure-monitor/app/monitor-web-app-availability.md) para ser alertado se o seu site estiver em baixo.

