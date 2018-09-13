---
title: Análise de utilização com o Azure Application Insights | Documentos da Microsoft
description: Compreenda os seus utilizadores e o que fazem com a sua aplicação.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: mbullwin
ms.openlocfilehash: 1217083980355c69fa51d7a6983c71295684f5b1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649750"
---
# <a name="usage-analysis-with-application-insights"></a>Análise de utilização com o Application Insights

Quais recursos da sua aplicação móvel ou web são mais populares? Os seus utilizadores alcançar seus objetivos com a sua aplicação? Que a deixam em pontos específicos e eles voltar mais tarde?  [O Azure Application Insights](app-insights-overview.md) ajuda-o a obter informações importantes sobre como as pessoas utilizam a sua aplicação. Sempre que atualizar a sua aplicação, pode avaliar a eficiência com que ele funciona para os utilizadores. Com esse conhecimento, pode fazer dados orientados pela decisões sobre os ciclos de desenvolvimento seguintes.

## <a name="send-telemetry-from-your-app"></a>Enviar telemetria a partir da sua aplicação

A melhor experiência é obtida pela instalação do Application Insights no seu código de servidor de aplicação e nas suas páginas web. Os componentes de cliente e servidor da sua aplicação enviam telemetria para o portal do Azure para análise.

1. **Código de servidor:** instalar o módulo apropriado para sua [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [node. js](app-insights-nodejs.md), ou [outros](app-insights-platforms.md) aplicação.

    * *Não pretende instalar o código de servidor? Basta [criar um recurso do Azure Application Insights](app-insights-create-new-resource.md).*

2. **Código de página Web:** abrir o [portal do Azure](https://portal.azure.com), abra o recurso do Application Insights para a sua aplicação e, em seguida, abra **introdução > monitorizar e diagnosticar lado do cliente**. 

    ![Copie o script para o cabeçalho da página web mestra.](./media/app-insights-usage-overview/02-monitor-web-page.png)

3. **Código de aplicação móvel:** utilizar o SDK do App Center para recolher eventos a partir da sua aplicação, em seguida, envie cópias desses eventos para o Application Insights para análise por [seguir este guia](app-insights-mobile-center-quickstart.md).

4. **Obter dados de telemetria:** execute seu projeto no modo de depuração para alguns minutos e, em seguida, procure os resultados no painel de descrição geral no Application Insights.

    Publique a sua aplicação para monitorizar o desempenho da sua aplicação e saber o que os usuários estão fazendo com a sua aplicação.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Incluir o ID de utilizador e a sessão na sua telemetria
Para controlar os utilizadores ao longo do tempo, o Application Insights requer uma maneira para identificá-los. A ferramenta de eventos é a única ferramenta de utilização que não é necessário um ID de utilizador ou um ID de sessão.

Começar a enviar o utilizador e IDs da sessão usando [esse processo](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Explorar dados demográficos de utilização e as estatísticas
Descubra quando as pessoas utilizam a sua aplicação, quais são as páginas que estiverem mais interessados, onde estão localizados os seus utilizadores, quais navegadores e sistemas operativos que a utilizam. 

Os relatórios de utilizadores e sessões de filtrar os seus dados por páginas ou eventos personalizados e segmentá-los por propriedades, tais como a localização, ambientais e de página. Também pode adicionar seus próprios filtros.

![Utilizadores](./media/app-insights-usage-overview/users.png)  

Informações à direita ressaltar padrões interessantes no conjunto de dados.  

* O **utilizadores** relatório conta os números de utilizadores exclusivos que suas páginas de acesso nos seus períodos de tempo escolhido. Para aplicações web, os utilizadores são contabilizados ao utilizar cookies. Se alguém acede ao seu local em browsers diferentes ou máquinas de cliente ou limpa os cookies, em seguida, eles serão contados mais de uma vez.
* O **sessões** relatório conta o número de sessões de utilizador ao seu site. Uma sessão é um período de atividade por um utilizador, terminada por um período de inatividade de mais do que meia hora.

[Mais informações sobre as ferramentas de utilizadores, sessões e eventos](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Visualizações da página

No painel de utilização, clique no mosaico de vistas de página para obter uma análise detalhada de suas páginas mais populares:

![No painel Descrição geral, clique no gráfico de vistas de página](./media/app-insights-usage-overview/05-games.png)

O exemplo acima é de um web site de jogos. A partir de gráficos, podemos pode instantaneamente ver:

* Utilização não melhorada na última semana. Talvez é necessário pensar sobre a otimização do mecanismo de pesquisa?
* Tênis é a página de jogo mais popular. Vamos nos concentrar em ainda mais melhorias para esta página.
* Em média, os utilizadores visitam a página de Tênis a cerca de três vezes por semana. (Há cerca de três vezes mais sessões de utilizadores.)
* A maioria dos usuários visite o site durante a semana de trabalho dos EUA e no horário de trabalho. Talvez iremos deve fornecer um botão "hide rápida" na página da web.
* O [anotações](app-insights-annotations.md) no gráfico mostram quando foram implementadas novas versões do Web site. Nenhuma das implementações recentes tinha um efeito considerável na utilização.

E se quiser investigar o tráfego para o seu site em mais detalhes, como divisão por uma propriedade personalizada que seu site envia na sua telemetria de vista de página?

1. Abra o **eventos** ferramenta no menu de recursos do Application Insights. Esta ferramenta permite-lhe analisar o número de vistas de página e eventos personalizados foram enviados pelo seu aplicativo, com base numa variedade de opções de filtragem, cohorting e segmentação.
2. Na lista pendente "Quem utilizou", selecione "Qualquer vista de página".
3. Na lista pendente "Split por", selecione uma propriedade pela qual dividir a telemetria de vista de página.

## <a name="retention---how-many-users-come-back"></a>Retenção - voltar o número de utilizadores?

Retenção ajuda-o a compreender a frequência com que os utilizadores regressam para utilizar a sua aplicação, com base em coortes de utilizadores que efetuou alguma ação de negócios durante um determinado bucket de tempo. 

- Compreender quais recursos específicos fazem com que os utilizadores fique mais que outras pessoas 
- Hipóteses de formulário com base nos dados de utilizador real 
- Determinar se o período de retenção é um problema em seu produto 

![Retenção](./media/app-insights-usage-overview/retention.png) 

Os controles de retenção na parte superior permitem-lhe definir eventos específicos e o intervalo de tempo para calcular a retenção. O gráfico no meio fornece uma representação visual da percentagem de retenção geral por intervalo de tempo especificado. O gráfico na parte inferior representa retenção individual num determinado período de tempo. Este nível de detalhe permite-lhe compreender o que os usuários estão fazendo e o que podem afetar a devolver os utilizadores sobre uma granularidade mais detalhada.  

[Mais informações sobre a ferramenta de retenção](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Eventos de negócios personalizados

Para obter uma compreensão clara de que os utilizadores que fazer com a sua aplicação, é útil inserir linhas de código para registar eventos personalizados. Esses eventos podem controlar qualquer coisa de ações de utilizador detalhadas, como clicar de botões específicos, para eventos de negócios mais significativos, como o fazer uma aquisição ou ganhar um jogo. 

Embora em alguns casos, as vistas de página podem representar eventos úteis, não é verdadeiro em geral. Um utilizador pode abrir uma página de produto, sem ter de comprar o produto. 

Com eventos de negócios específicas, pode gráfico progresso dos seus utilizadores através do seu site. Pode encontrar as suas preferências para as diferentes opções e onde eles drop fora ou têm dificuldades. Com esse conhecimento, pode tomar decisões informadas sobre as prioridades em sua lista de pendências de desenvolvimento.

Eventos podem ser registados no lado do cliente da aplicação:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Ou do lado do servidor:

```csharp
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Pode anexar os valores de propriedade a esses eventos, para que pode filtrar ou divida os eventos quando inspecioná-las no portal. Além disso, um conjunto padrão de propriedades está anexado a cada evento, como ID de utilizador anónimo, que permite-lhe rastrear a sequência de atividades de um utilizador individual.

Saiba mais sobre [eventos personalizados](app-insights-api-custom-events-metrics.md#trackevent) e [propriedades](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Eventos de examinar

Nas ferramentas utilizadores, sessões e eventos, pode segmentar e decomposição de eventos personalizados por utilizador, nome de eventos e propriedades.
![Utilizadores](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Design a telemetria com a aplicação

Durante o projeto de cada funcionalidade da sua aplicação, considere como irá medir seu sucesso com os seus utilizadores. Decida quais eventos comerciais, precisa de gravar e o controlo de chamadas para esses eventos na sua aplicação desde o início do código.

## <a name="a--b-testing"></a>R | Teste de B
Se não souber qual variante de um recurso será a terem mais êxito, versão ambas, tornando cada acessível para diferentes utilizadores. Medir o sucesso de cada uma e, em seguida, mover para uma versão unificada.

Para essa técnica é anexar valores de propriedade distintos para toda a telemetria que é enviada por cada versão da sua aplicação. Pode fazer isso definindo propriedades no TelemetryContext Active Directory. Essas propriedades predefinidas são adicionadas a todas as mensagens de telemetria que a aplicação envia - não apenas as mensagens personalizadas, mas também a telemetria standard.

No portal do Application Insights, filtrar e dividir os dados nos valores de propriedade, de modo a comparar as diferentes versões.

Para fazer isso, [configurar um inicializador de telemetria](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```csharp


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

No inicializador de aplicação web, como Global.asax.cs:

```csharp

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Todos os TelemetryClients novo adiciona automaticamente o valor de propriedade que especificar. Eventos de telemetria individuais podem substituir os valores predefinidos.

## <a name="next-steps"></a>Passos Seguintes
   - [Utilizadores, Sessões, Eventos](app-insights-usage-segmentation.md)
   - [Funis](usage-funnels.md)
   - [Retenção](app-insights-usage-retention.md)
   - [Fluxos do Utilizador](app-insights-usage-flows.md)
   - [Livros](app-insights-usage-workbooks.md)
   - [Adicionar contexto de utilizador](app-insights-usage-send-user-context.md)
