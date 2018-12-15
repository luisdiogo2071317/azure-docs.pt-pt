---
title: Gerir preços e volumes de dados do Azure Application Insights | Documentos da Microsoft
description: Gerir volumes de telemetria e monitorizar os custos no Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: Dale.Koetke
ms.date: 08/11/2018
ms.author: mbullwin
ms.openlocfilehash: 8a0acbfa18053b6b50bd872d109b02d556a6f5f3
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436066"
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Gerir preços e volumes de dados no Application Insights

> [!NOTE]
> Este artigo descreve como analisar a utilização dos dados do Application Insights.  Veja os artigos seguintes para obter informações relacionadas.
> - [Monitorizar a utilização e custos estimados](../azure-monitor/platform/usage-estimated-costs.md) descreve como ver a utilização e custos estimados no Azure de várias funcionalidades para diferentes modelos de preços de monitorização. Também descreve como alterar o modelo de preços.

Preços para [do Azure Application Insights] [ start] baseia-se no volume de dados por aplicação. Cada recurso do Application Insights é cobrado como um serviço separado e contribui para a faturação da sua subscrição do Azure.

O Application Insights tem dois planos de preços: Básico e Enterprise. O plano de preços básico é o plano predefinido. Inclui todas as funcionalidades do plano de Enterprise, sem custos adicionais. As listas de plano básico, principalmente no volume de dados que são ingeridos. 

O plano de Enterprise tem uma cobrança por nó e cada nó recebe um montante diário de dados. Na empresa preços do plano, é-lhe cobrada aos dados ingeridos acima do montante incluído. Se utilizar o Operations Management Suite, deve escolher o plano Enterprise. 

Se tiver perguntas sobre preços do Application Insights, pode postar uma pergunta no nosso [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Planos de preços

Para obter preços atuais na sua moeda e região, veja [os preços do Application Insights][pricing].

> [!NOTE]
> Em Abril de 2018, vamos [introduzida](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) um novo modelo de preços para a monitorização do Azure. Esse modelo adota um modelo simples de "pay as you go" em todo o portefólio completado dos serviços de monitorização. Saiba mais sobre o [novo modelo de preços](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), como a [avaliar o impacto da migração para esse modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) com base nos seus padrões de utilização, e [como otimizado para o novo modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Plano básico

O plano Basic é a predefinição de plano de preços, quando é criado um novo recurso do Application Insights. O plano básico é ideal para todos os clientes, exceto aqueles que tiver uma subscrição do Operations Management Suite.

* No plano Basic, é cobrado por volume de dados. Volume de dados é o número de bytes de telemetria recebida pelo Application Insights. Volume de dados é medido como o tamanho do pacote de dados JSON não comprimido que é recebido pelo Application Insights a partir da sua aplicação. Para [importados para a análise de dados tabulares](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), volume de dados é medido como o tamanho descomprimido de ficheiros que são enviados para o Application Insights.
* Encargos de dados do seu aplicativo agora são comunicados num novo contador de faturação com o nome **ingestão de dados** a partir de Abril de 2018. Este é o novo medidor ser partilhadas entre a monitorização de tecnologias, como informações de aplicativos e do Log Analytics e está atualmente sob o nome do serviço **dos serviços de aplicações** (e a alteração em breve para **do Log Analytics**). 
* [Live Stream métricas](app-insights-live-stream.md) dados não são contabilizados para fins de preços.
* [A exportação contínua](app-insights-export-telemetry.md) e o [conector do Azure Log Analytics](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) estão disponíveis sem custo adicional no plano Basic a partir de Abril de 2018.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Plano de Enterprise e elegibilidades da subscrição do Operations Management Suite

Os clientes que comprarem o Operations Management Suite E1 e E2 podem obter o Application Insights empresarial como um componente adicional sem custos adicionais, como [anunciado anteriormente](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Especificamente, cada unidade do Operations Management Suite E1 e E2 inclui a elegibilidade para um nó do plano Enterprise do Application Insights. Cada nó do Application Insights inclui até 200 MB de dados ingeridos por dia (separado da ingestão de dados do Log Analytics), com uma retenção de dados de 90 dias sem custos adicionais. O plano é descrito em mais detalhada posteriormente neste artigo. 

Uma vez que este plano é aplicável apenas para clientes com uma assinatura do Operations Management Suite, os clientes que não tem uma subscrição do Operations Management Suite não vir uma opção para selecionar este plano.

> [!NOTE]
> Para garantir que obtém esta elegibilidade, seus recursos do Application Insights tem de ser na empresa plano de preços. Esta elegibilidade aplica-se apenas como nós. Recursos do Application Insights no plano Basic não percebem algum benefício. Este direito não está visível os custos estimados mostrada os **utilização e custo estimado** painel. Além disso, se mover uma subscrição para o novo Azure monitorização o modelo de preços em Abril de 2018, o plano básico é o plano apenas disponível. Mover uma subscrição para o Azure de novo modelo de preços de monitorização não é aconselhável se tiver uma subscrição do Operations Management Suite.

Para obter mais informações sobre o plano Enterprise, consulte [os detalhes dos preços do Enterprise](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Testes Web com vários passos

[Testes web de vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests) implicar um custo adicional. Testes web de vários passos são testes web que executam uma sequência de ações.

Não existe cobrança separada para *testes de ping* de uma única página. Telemetria dos testes de ping e testes de vários passos é cobrada a mesma que outros telemetria da sua aplicação.

## <a name="review-pricing-plans-and-estimate-costs"></a>Reveja os planos de preços e estimar os custos

O Application Insights torna mais fácil de compreender os planos de preços que estão disponíveis e que os seus custos têm probabilidades de ser com base nos padrões de utilização recente. Para começar a utilizar, no portal do Azure, para o recurso do Application Insights, vá para o **utilização e custos estimados** painel:

![Escolha preços](./media/app-insights-pricing/pricing-001.png)

R. Reveja o volume dos seus dados para o mês. Isto inclui todos os dados que recebeu e mantidos (após qualquer [amostragem](app-insights-sampling.md)) de seu servidor e as aplicações de cliente e de testes de disponibilidade.  
B. Cobrado separadamente é feito [testes web de vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Isso não incluir testes de disponibilidade simples, que estão incluídos no custo de volume de dados.)  
C. Ver as tendências de volume de dados do mês passado.  
D. Ativar a ingestão de dados [amostragem](app-insights-sampling.md).   
E. Defina o limite de volume de dados diário.  

Custos do Application Insights são adicionados à sua fatura do Azure. Pode ver detalhes do seu do Azure são faturadas no **faturação** secção do portal do Azure ou no [portal de faturação do Azure](https://account.windowsazure.com/Subscriptions). 

![No menu à esquerda, selecione faturação](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Taxa de dados
O volume de dados que envia é limitado de três formas:

* **Amostragem**: Pode usar amostragem para reduzir a quantidade de telemetria é enviada a partir do seu servidor e aplicações de cliente, com o mínimo distorção de métricas. A amostragem é a principal ferramenta, que pode usar para ajustar a quantidade de dados que envia. Saiba mais sobre [funcionalidades de amostragem](app-insights-sampling.md). 
* **Limite diário**: Quando cria um recurso do Application Insights no portal do Azure, o limite diário está definido para 100 GB/dia. Quando cria um recurso do Application Insights no Visual Studio, a predefinição é pequena (apenas 32,3 MB/dia). A predefinição de limite diário está definida para facilitar os testes. Seu objetivo é que o utilizador irá elevar o limite diário antes de implementar a aplicação para produção. 

    O limite máximo é de 1000 GB/dia, a menos que solicitar um máximo de superior para uma aplicação de tráfego elevado. 

    Utilize o cuidado ao definir o limite diário. Deve ser sua intenção *nunca atingirá o limite diário*. Se tiver atingido o limite diário, perde os dados para o resto do dia, e não é possível monitorizar a sua aplicação. Para alterar o limite diário, utilize o **limite de volume diário** opção. Pode aceder a esta opção no **utilização e custos estimados** painel (descrito mais detalhadamente posteriormente neste artigo).
    Removemos a restrição em alguns tipos de subscrição que utilizam crédito não foi possível utilizar para o Application Insights. Anteriormente, se a subscrição tem um limite de gastos, a caixa de diálogo do limite diário tem instruções para remover o limite de gastos e ativar o limite diário a ser gerado para além dos 32,3 MB/dia.
* **Limitação**: Limites de conjunto a taxa de dados para 32.000 eventos por segundo, média das mais 1 minuto para cada chave de instrumentação.

*O que acontece se a minha aplicação exceder a taxa de aceleração?*

* O volume de dados que envia a sua aplicação é avaliado a cada minuto. Se exceder a taxa de por segundo média no minuto, o servidor de recusa-se alguns pedidos. O SDK coloca os dados na memória intermédia e, em seguida, tenta reenviá-lo. E espalha com um pico ao longo de vários minutos. Se a sua aplicação enviar consistentemente dados em mais do que a taxa de aceleração, serão possível remover alguns dados. (O ASP.NET, Java e JavaScript SDKs tentam reenviar dados desta forma; outros SDKs poderão simplesmente soltar limitada dados). Se ocorrer a limitação, um aviso de notificação alerta-o que ocorreu esta situação.

*Como posso saber a quantidade de dados está a enviar a minha aplicação?*

Pode utilizar uma das seguintes opções para ver a quantidade de dados a aplicação está a enviar:

* Vá para o **utilização e custo estimado** painel para ver o gráfico de volume de dados diário. 
* No Explorador de métricas, adicione um novo gráfico. Para a métrica de gráfico, selecione **volume do ponto de dados**. Ative **agrupamento**e, em seguida, agrupe por **tipo de dados**.

## <a name="reduce-your-data-rate"></a>Reduzir a taxa de dados
Aqui estão algumas coisas que pode fazer para reduzir o volume dos seus dados:

* Uso [amostragem](app-insights-sampling.md). Essa tecnologia reduz a taxa de dados sem a inclinação de suas métricas. Não perde a capacidade de navegar entre os itens relacionados na pesquisa. Em aplicativos de servidor, amostragem funciona automaticamente.
* [Limitar o número de chamadas de Ajax que podem ser comunicadas](app-insights-javascript.md#detailed-configuration) em cada vista de página ou desativar o relatório de Ajax.
* [Edite applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md) para alterar os módulos de coleção de que não precisa. Por exemplo, poderá decidir que os contadores de desempenho ou dados de dependência são inessential.
* Divida a telemetria entre chaves de instrumentação separado. 
* Métricas de agregação previamente. Se colocar chamadas para TrackMetric na sua aplicação, pode reduzir o tráfego utilizando a sobrecarga que aceita o cálculo da média e o desvio-padrão de um lote de medidas. Em alternativa, pode utilizar um [pré-agregar pacote](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Gerir o volume de dados máximo diário

Pode usar o limite de volume diário para limitar os dados recolhidos. No entanto, se o limite for cumprido, ocorre uma perda de toda a telemetria enviada a partir da sua aplicação para o resto do dia. É *não é aconselhável* para que seu aplicativo atingir o limite diário. Não é possível controlar o estado de funcionamento e o desempenho da sua aplicação depois de atingir o limite diário.

Em vez de usar o limite de volume diário, utilize [amostragem](app-insights-sampling.md) para otimizar o volume de dados para o nível desejado. Em seguida, utilize o limite diário apenas como "último recurso", no caso de seu aplicativo inesperadamente começa a enviar muito mais altos volumes de telemetria.

Para alterar o limite diário, no **configurar** secção de recurso do Application Insights, na **utilização e custos estimados** painel, selecione **limite diário**.

![Ajustar o limite de volume de telemetria diário](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Amostragem
[Amostragem](app-insights-sampling.md) é um método de reduzir a taxa a que telemetria é enviada para a sua aplicação, mantendo a capacidade para encontrar eventos relacionados durante a pesquisas de diagnóstico. Também mantém as contagens de eventos correto.

A amostragem é uma maneira eficiente de reduzir os custos e manter-se dentro da sua quota mensal. O algoritmo de amostragem retém itens relacionados de telemetria, então, por exemplo, quando usa a pesquisa, pode encontrar o pedido relacionados com uma exceção em particular. O algoritmo mantém também contagens corretas para ver os valores corretos no Explorador de métricas para taxas de pedidos, taxas de exceção e outras contagens.

Existem várias formas de amostragem.

* [Amostragem adaptável](app-insights-sampling.md) é o padrão para o SDK do ASP.NET. Amostragem adaptável se ajusta automaticamente o volume de telemetria que envia a sua aplicação. Ele funciona automaticamente no SDK na sua aplicação web para que o tráfego de telemetria na rede é reduzido. 
* *Amostragem de ingestão* é uma alternativa que opera no ponto onde a telemetria da sua aplicação insere o serviço Application Insights. Amostragem de ingestão não afeta o volume de telemetria enviada pela sua aplicação, mas reduz o volume que é mantido pelo serviço. Pode utilizar a amostragem de ingestão para reduzir a quota utilizada pelo telemetria a partir de browsers e outros SDKs.

Para definir a amostragem de ingestão, vá para o **preços** painel:

![No painel preços e Quota, selecione o mosaico de exemplos e, em seguida, selecione uma fração de amostragem](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> O **amostragem de dados** painel controla apenas o valor da amostragem de ingestão. Ele não reflete a taxa de amostragem, que é aplicada ao SDK do Application Insights na sua aplicação. Se a telemetria de entrada já amostragem no SDK, não é aplicada a amostragem de ingestão.
>

Para detetar a taxa de amostragem real, não importa de onde é foi aplicada, use um [consulta do Analytics](app-insights-analytics.md). A consulta tem o seguinte aspeto:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Em cada retidos registo, `itemCount` indica o número de registos originais que representa. É igual a 1 + o número de registos de rejeitados anteriores. 

## <a name="automation"></a>Automatização

Pode escrever um script para definir o plano de preços com a gestão de recursos do Azure. [Saiba como](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Resumo de limites

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Desativar o diário de limite de emails

Para desativar os emails de limite de volume diário, no **configurar** secção de recurso do Application Insights, na **utilização e custos estimados** painel, selecione **limite diário** . Existem definições para enviar um e-mail quando for atingido o limite máximo, bem como quando um nível de aviso ajustável foi atingido. Se pretender desativar todas as diariamente cap relacionados com o volume e-mails desmarque as caixas.

## <a name="next-steps"></a>Passos Seguintes

* [Amostragem](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/