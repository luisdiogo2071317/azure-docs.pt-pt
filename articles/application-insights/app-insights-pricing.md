---
title: Gerir o volume de dados e preços para o Azure Application Insights | Microsoft Docs
description: Volumes de telemetria de gerir e monitorizar os custos no Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 2c06c2220d3a3ed0a27b4f0febb4de95b2137ddc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Gerir o volume de preços e os dados no Application Insights

Preços para [Azure Application Insights] [ start] baseia-se no volume de dados por aplicação. Cada recurso do Application Insights é cobrado como um serviço separado e contribui para a fatura para a sua subscrição do Azure.

O Application Insights tem dois planos de preços: Basic e Enterprise. O plano de preços básico é o plano predefinido. Inclui todas as funcionalidades do plano de Enterprise, sem custos adicionais. Faturas o plano básico principalmente no volume de dados é ingeridos. 

O plano de Enterprise tem cobrada uma taxa por nó, e cada nó recebe um allowance dados diários. A empresa preços plano, os dados é cobrados ingeridos acima o allowance incluído. Se utilizar o Operations Management Suite, deve escolher o plano de empresa. 

Se tiver dúvidas sobre como preços funciona para o Application Insights, pode colocar uma pergunta nos nossos [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Planos de preços

Para obter preços atuais na sua moeda e região, consulte [preços do Application Insights][pricing].

> [!NOTE]
> Em Abril de 2018, iremos [introduzida](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) um novo modelo de preços para monitorização do Azure. Este modelo adopts um modelo simples "pay as you go" entre o portefólio completado de monitorização de serviços. Saiba mais sobre o [novo modelo de preços](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), como a [avaliar o impacto de mover para este modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) com base nos seus padrões de utilização, e [como optar ativamente por participar no modelo de nova](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Plano básico

O plano básico é a predefinição plano de preços, quando é criado um novo recurso do Application Insights. O plano básico é o ideal para todos os clientes, exceto aqueles que tiver uma subscrição do Operations Management Suite.

* No plano básico, são-lhe cobrados pelo volume de dados. Volume de dados é o número de bytes de telemetria recebida pelo Application Insights. 
    
    Volume de dados é medido como o tamanho do pacote de dados JSON descomprimido que é recebido pelo Application Insights da sua aplicação.

    Para [importados para análise de dados de tabela](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), volume de dados é medido como o tamanho descomprimido de ficheiros que são enviados para o Application Insights.
* [Transmissão de velocidade de métricas](app-insights-live-stream.md) dados não estão contabilizados para fins de preço.
* [A exportação contínua](app-insights-export-telemetry.md) e [conector Log Analytics do Azure](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) no plano básico a partir de Abril de 2018 estão disponíveis, sem encargos adicionais.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Plano de empresa e a elegibilidade de subscrição do Operations Management Suite

Os clientes que comprar E1 Operations Management Suite e E2 podem obter o Application Insights empresarial como um componente sem custos adicionais como [anteriormente anunciada](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Especificamente, cada unidade de Operations Management Suite E1 e E2 inclui uma elegibilidade para um nó do plano empresarial do Application Insights. Cada nó do Application Insights inclui até 200 MB de dados ingeridos por dia (separado da ingestão de dados de análise de registos), com retenção de dados de 90 dias sem custos adicionais. O plano é descrito em obter mais informações detalhadas mais tarde no artigo. 

Como este plano é aplicável apenas aos clientes com uma subscrição do Operations Management Suite, os clientes que não tem uma subscrição do Operations Management Suite não veem uma opção para selecionar este plano.

> [!NOTE]
> Para garantir que recebem esta elegibilidade, tem de ser os recursos do Application Insights na empresa plano de preços. Esta elegibilidade aplica-se apenas como nós. Recursos de informações de aplicação no plano básico não tenha em consideração qualquer benefício em. Esta elegibilidade não está visível os custos estimados mostrado no **utilização e o custo estimado** painel. Além disso, se mover uma subscrição para o novo Azure monitorização modelo de preços em Abril de 2018, o plano básico é o plano apenas disponível. A mover de uma subscrição do Azure de novo modelo de preços de monitorização não é aconselhável se tiver uma subscrição do Operations Management Suite.

Para obter mais informações sobre o plano de empresa, consulte [detalhes de preços do Enterprise](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Testes Web com vários passos

[Testes web com vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests) cobrado um custo adicional. Testes web com vários passos são testes web que execute uma sequência de ações.

Há sem qualquer encargo separado para *ping testes* de uma única página. A telemetria de testes de ping e testes de vários passos é cobrada o mesmo que outros a telemetria da sua aplicação.

## <a name="review-pricing-plans-and-estimate-costs"></a>Reveja os planos de preços e estimar os custos

Application Insights torna mais fácil de compreender os planos de preços disponíveis e que os custos é prováveis que estão a ser com base nos padrões de utilização recente. Para começar a utilizar, no portal do Azure, para o recurso do Application Insights, vá para o **utilização e os custos estimados** painel:

![Escolha de preços](./media/app-insights-pricing/pricing-001.png)

A. Reveja o volume de dados para o mês. Isto inclui todos os dados que recebeu e mantidos (após qualquer [amostragem](app-insights-sampling.md)) do seu servidor e as aplicações cliente e de testes de disponibilidade.  
B. É efetuada cobrada uma taxa separada [testes web com vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Isto não incluir testes de disponibilidade simples, que estão incluídos na taxa de volume de dados.)  
C. Ver tendências de volume de dados do último mês.  
D. Ativar a ingestão de dados [amostragem](app-insights-sampling.md).   
E. Defina a extremidade de volume de dados diária.  

Custos do Application Insights são adicionados à fatura do Azure. Pode ver os detalhes do seu Azure faturar-na **faturação** secção do portal do Azure ou no [portal de faturação do Azure](https://account.windowsazure.com/Subscriptions). 

![No menu à esquerda, selecione faturação](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Taxa de dados
O volume de dados que envie é limitado de três formas diferentes:

* **Amostragem**: pode utilizar a amostragem para reduzir a quantidade de telemetria que é enviada das suas aplicações de servidor e cliente, com distortion mínima de métricas. A amostragem é a principal ferramenta que pode utilizar para otimizar a quantidade de dados que envie. Saiba mais sobre [amostragem funcionalidades](app-insights-sampling.md). 
* **Cap diária**: ao criar um recurso do Application Insights no portal do Azure, a extremidade diária está definida para 100 GB diário. Quando cria um recurso do Application Insights no Visual Studio, a predefinição é pequena (apenas 32.3 MB/dia). A predefinição de extremidade diária está definida para facilitar a testar. Destina-se que o utilizador irá elevar o limite diário antes de implementar a aplicação em produção. 

    O limite máximo é 1000 GB diário, exceto se solicitar um máximo superior para uma aplicação de tráfego elevado. 

    Utilize cuidado ao definir o limite diário. A intenção deve ser a *nunca atingiu o limite diário*. Se atingiu o limite diário, perder dados para o resto do dia e não é possível monitorizar a sua aplicação. Para alterar o limite diário, utilize o **diária extremidade de volume** opção. Pode aceder a esta opção no **utilização e os custos estimados** painel (descrito em mais detalhe posteriormente no artigo).
    Removemos a restrição em alguns tipos de subscrição que tem crédito que não foi utilizado para o Application Insights. Anteriormente, se a subscrição tem um limite de gastos, a caixa de diálogo de extremidade diária tem instruções para remover o limite de gastos e ativar a cap diária gerado para além do diário 32.3 MB.
* **Limitação**: os limites de velocidade de dados para 32,000 eventos por segundo, apresentou uma média de mais de 1 minuto.

*O que acontece se a taxa de limitação de exceder a minha aplicação?*

* O volume de dados que a aplicação envia é avaliado a cada minuto. Se exceder a taxa de por segundo média o minuto, o servidor recusa-se alguns pedidos. O SDK coloca os dados e, em seguida, tenta reenviá-lo. E propaga com um aumento através de alguns minutos. Se a sua aplicação consistentemente envia dados em mais do que a taxa de limitação, alguns dados serão ignorados. (O ASP.NET, Java e JavaScript SDKs tentarem reenviar dados desta forma; outros SDKs poderão simplesmente largar limitada dados). Se ocorrer a limitação, um aviso de notificação alertas que ocorreu esta situação.

*Como sei a minha aplicação está a enviar quantidade de dados?*

Pode utilizar uma das seguintes opções para ver a quantidade de dados da aplicação está a enviar:

* Vá para o **utilização e o custo estimado** painel para ver o gráfico de volume de dados diária. 
* No Explorador de métricas, adicione um novo gráfico. Para a métrica de gráfico, selecione **volume do ponto de dados**. Ativar **agrupamento**e, em seguida, grupo por **tipo de dados**.

## <a name="reduce-your-data-rate"></a>Reduz a taxa de dados
Eis algumas coisas que pode fazer para reduzir o volume de dados:

* Utilize [amostragem](app-insights-sampling.md). Esta tecnologia reduz a taxa de dados sem skewing as métricas. Não perderá a capacidade de navegar entre itens relacionados na pesquisa. Nas aplicações de servidor, amostragem funciona automaticamente.
* [Limitar o número de chamadas Ajax que podem ser comunicadas](app-insights-javascript.md#detailed-configuration) em cada vista de página ou comutador desativar relatórios de Ajax.
* [Edite Applicationinsights](app-insights-configuration-with-applicationinsights-config.md) para desativar os módulos de coleção que não precisa. Por exemplo, poderá decidir se os contadores de desempenho ou dados de dependência são inessential.
* Divida a telemetria entre chaves de instrumentação separado. 
* Pré-agrega as métricas. Se o put chamadas para TrackMetric na sua aplicação, pode reduzir o tráfego utilizando a sobrecarga que aceita o cálculo da média e o desvio-padrão de um lote de medidas. Em alternativa, pode utilizar um [previamente agregar pacote](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Gerir o volume de dados diária máximo

Pode utilizar a extremidade de volume diária para limitar os dados recolhidos. No entanto, se a extremidade for cumprida, ocorre a perda de toda a telemetria enviada a partir da sua aplicação para o resto do dia. É *não recomendado* ter a sua aplicação atingiu o limite diário. Não é possível controlar o estado de funcionamento e desempenho da aplicação depois de atingir o limite diário.

Em vez de utilizar a cap diária de volume, utilize [amostragem](app-insights-sampling.md) para otimizar o volume de dados para o nível que pretende. Em seguida, utilize a cap diária apenas como um "último recurso", no caso da aplicação inesperadamente começa a enviar muito superiores dos volumes de telemetria.

Para alterar a cap diária, no **configurar** secção de recurso do Application Insights, no **utilização e os custos estimados** painel, selecione **extremidade diária**.

![Ajustar a extremidade de volume de telemetria diária](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Amostragem
[Amostragem](app-insights-sampling.md) é um método de reduzir a velocidade a que é enviada telemetria à sua aplicação, mantendo a capacidade para encontrar eventos relacionados durante pesquisas de diagnóstico. Também retêm contagens de evento correto.

A amostragem é uma forma eficaz para reduzir os custos e permanecer dentro da sua quota mensal. O algoritmo de amostragem mantém relacionadas com itens de telemetria, por isso, por exemplo, quando utilizar a pesquisa, pode encontrar o pedido relacionadas com uma exceção específica. O algoritmo mantém também contagens corretas para ver os valores corretos no Explorador de métrica de taxas de pedidos, taxas de exceção e outras contagens.

Existem várias formas de amostragem.

* [Amostragem adaptável](app-insights-sampling.md) é a predefinição para o SDK do ASP.NET. Amostragem adaptável se ajusta automaticamente para o volume de telemetria que envia a sua aplicação. Funciona automaticamente no SDK na sua aplicação web para que o tráfego de telemetria na rede é reduzido. 
* *A amostragem de ingestão* é uma alternativa que opera no ponto onde a telemetria da sua aplicação introduz o serviço do Application Insights. A amostragem de ingestão não afeta o volume de telemetria enviado a partir da sua aplicação, mas reduz o volume que é mantido pelo serviço. Pode utilizar a amostragem de ingestão para reduzir a quota utilizada pela telemetria de outros SDKs e browsers.

Para definir a amostragem de ingestão, vá para o **preços** painel:

![A Quota e o painel de preço, selecione o mosaico de exemplos e, em seguida, selecione uma fração de amostragem](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> O **amostragem de dados** painel controla apenas o valor da amostragem da ingestão. Esta não reflete a frequência de amostragem, que é aplicada ao Application Insights SDK na sua aplicação. Se a telemetria de entrada já amostragem no SDK, não é aplicada a amostragem da ingestão.
>

Para detetar a frequência de amostragem real, independentemente de onde é foi aplicado, utilize um [consulta de análise](app-insights-analytics.md). A consulta tem o seguinte aspeto:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Em cada retidos registo, `itemCount` indica o número de registos originais que representa. É igual a 1 + o número de registos rejeitados anteriores. 

## <a name="automation"></a>Automatização

Pode escrever um script para definir o plano de preços utilizando a gestão de recursos do Azure. [Saiba como](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Resumo de limites

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Amostragem](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
