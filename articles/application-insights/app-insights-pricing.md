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
ms.openlocfilehash: 42c54509cedf48e9ebabb5b50865ed56e54bee05
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Gerir o volume de preços e os dados no Application Insights

Preços para [Azure Application Insights] [ start] baseia-se no volume de dados por aplicação. Cada recurso do Application Insights é cobrado como um serviço separado e contribui para a fatura para a sua subscrição para o Azure.

Existem dois planos de preços. O plano predefinido é denominado básico que inclui todas as funcionalidades como o plano de Enterprise, sem custos de adição e faturas principalmente no volume de dados ingeridos mesmas. Se estiver a utilizar o Operations Management Suite, deve escolher para o plano, de que tem um por nó cobram juntamente com diariamente allowances de dados e, em seguida, cobrará um encargo para ingeridos acima o allowance incluído os dados da empresa.

Se tiver dúvidas sobre como preços funciona para o Application Insights, não hesite em publicar uma pergunta nos nossos [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="the-price-plans"></a>Os planos de preços

Consulte o [página de preços do Application Insights] [ pricing] para os preços atuais na sua região e moeda.

### <a name="basic-plan"></a>Plano básico

O plano básico é o predefinido quando é criado um novo recurso do Application Insights e é o ideal para todos os clientes, exceto aqueles com uma subscrição do Operations Management Suite.

* No plano básico, são-lhe cobrados pelo volume de dados: número de bytes de telemetria recebida pelo Application Insights. Volume de dados é medido como o tamanho do pacote de dados JSON descomprimido recebido pelo Application Insights da sua aplicação.
Para [importados para análise de dados de tabela](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), o volume de dados é medido como o tamanho descomprimido de ficheiros enviados para o Application Insights.
* [Transmissão de velocidade de métricas](app-insights-live-stream.md) dados não estão contabilizados para fins de preço.
* [A exportação contínua](app-insights-export-telemetry.md) e [conector de análise de registos](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) no plano básico a partir de Abril de 2018 estão disponíveis sem qualquer custo adicional.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Plano de empresa e a elegibilidade de subscrição do Operations Management Suite

Os clientes que comprar E1 do Microsoft Operations Management Suite e E2 conseguem obter o Application Insights empresarial como um componente sem custos adicionais como [anteriormente anunciada](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Especificamente, cada unidade de Operations Management Suite E1 e E2 inclui uma elegibilidade para 1 nó do plano empresarial do Application Insights. Conforme descrito abaixo, mais detalhadamente, cada nó do Application Insights inclui até 200 MB de dados ingeridos por dia (separado da ingestão de dados de análise de registos), com retenção de dados de 90 dias sem custos adicionais. Uma vez que isso apenas se aplica aos clientes com uma subscrição do Operations Management Suite, os clientes sem uma subscrição não irão ver uma opção para selecionar este plano.

> [!NOTE]
> Para garantir que recebem esta elegibilidade, tem de ter os recursos do Application Insights na empresa plano de preços. Esta elegibilidade aplica-se apenas como nós de, pelo que os recursos do Application Insights no plano básico não serão tenha em consideração qualquer benefício em. Tenha em atenção que esta elegibilidade não ficará visível nos custos estimados apresentados no *utilização e o custo estimado* página. Além disso, se mover uma subscrição para o Azure novo modelo de preços de 2018 de Abril de monitorização, o plano básico será o plano apenas disponível para que não é aconselhada se tiver uma subscrição do Operations Management Suite.

Para obter mais detalhes sobre o plano de Enterprise visitam o [página de detalhes de preços Enterprise.](app-insights-pricing-enterprise-details.md)

### <a name="multi-step-web-tests"></a>Testes Web com vários passos

Há um custo adicional para [testes web com vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Isto refere-se para testes web que execute uma sequência de ações.

Não há sem qualquer encargo separado para 'os testes de ping' de uma única página. A telemetria de testes de ping e testes de vários passos é cobrada juntamente com outra telemetria da sua aplicação.

## <a name="review-pricing-plans-and-estimate-costs"></a>Reveja os planos de preços e estimar os custos

Application Insights torna mais fácil de compreender os planos de preços disponíveis e que os custos são provável que ser com base nos padrões de utilização recente. Comece por abrir o **utilização e os custos estimados** página no recurso do Application Insights no portal do Azure:

![Escolha o preço.](./media/app-insights-pricing/pricing-001.png)

**a.** Reveja o volume de dados para o mês. Isto inclui todos os dados recebidos e mantidos (após qualquer [amostragem](app-insights-sampling.md) do seu servidor e as aplicações cliente e de testes de disponibilidade.

**b.** É efetuada cobrada uma taxa separada [testes web com vários passos](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Isto não incluir testes de disponibilidade simples, que estão incluídos na taxa de volume de dados.)

**c.** Ver tendências de volume de dados para o último mês.

**d.** Ativar a ingestão de dados [amostragem.](app-insights-sampling.md) 

**e.** Configure a extremidade de volume de dados diária.

Custos do Application Insights são adicionados à fatura do Azure. Pode ver os detalhes do seu Azure cobrar na secção de faturação do portal do Azure ou no [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![No menu do lado, escolha a faturação.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Taxa de dados
Existem três formas em que o volume de enviar dados está limitado:

* **A amostragem:** pode ser utilizado este mecanismo de reduzir a quantidade de telemetria enviada das suas aplicações de servidor e cliente, com distortion mínima de métricas. Esta é a principal ferramenta que terá de ajustar a quantidade de dados. Saiba mais sobre [amostragem funcionalidades](app-insights-sampling.md). 
* **Cap diária:** quando criar um recurso do Application Insights do portal do Azure está definido para 100 GB diário. A predefinição quando criar um recurso do Application Insights do Visual Studio, é pequeno (apenas 32.3 MB/dia), que é dirigido apenas para o facilitar a testar. Neste caso, destina-se que o utilizador irá elevar o limite diário antes de implementar a aplicação em produção. O limite máximo é diário 1000 GB, a menos que pediu um máximo superior para uma aplicação de tráfego elevado. Utilizar cuidado ao definir a cap diária, como deve ser a sua intenção **nunca para atingiu o limite diário**, porque, em seguida, irá perder dados para o resto do dia e não conseguir monitorizar a sua aplicação. Para alterá-lo, utilize a opção de extremidade de volume diária, ligada da utilização e a página de custos estimados (ver abaixo). Removemos a restrição em alguns tipos de subscrição que tem crédito que não pode ser utilizado para o Application Insights. Anteriormente, se a subscrição tem um limite de gastos, a caixa de diálogo de extremidade diária tem instruções como removê-lo e ativar a cap diária gerado para além do diário 32.3 MB.
* **Limitação:** Isto limita a taxa de dados para 32,000 eventos por segundo, média ao longo de 1 minuto.

*O que acontece se a taxa de limitação de exceder a minha aplicação?*

* O volume de dados que a aplicação envia é avaliado a cada minuto. Se exceder a taxa de por segundo média o minuto, o servidor recusa-se alguns pedidos. O SDK coloca os dados e, em seguida, tenta reenviar, propagando-se com um aumento através de alguns minutos. Se a aplicação consistentemente envia dados em acima a taxa de limitação, alguns dados serão ignorados. (O ASP.NET, Java e JavaScript SDKs tentarem Reenviar desta forma; outros SDKs poderão simplesmente largar limitada dados). Se ocorrer a limitação, verá uma notificação de aviso se isto tiver ocorrido.

*Como sei a minha aplicação está a enviar quantidade de dados?*

* Abra o **utilização e o custo estimado** página para ver o gráfico de volume de dados diária. 
* Ou, no Explorador de métricas, adicione um novo gráfico e selecione **volume do ponto de dados** como a métrica. Comutador no agrupamento e agrupar por **tipo de dados**.

## <a name="to-reduce-your-data-rate"></a>Para reduzir a velocidade de dados
Eis algumas coisas que pode fazer para reduzir o volume de dados:

* Utilize [amostragem](app-insights-sampling.md). Esta tecnologia reduz a taxa de dados sem skewing métricas e sem perturbar a capacidade de navegar entre itens relacionados na pesquisa. Nas aplicações de servidor, funciona automaticamente.
* [Limitar o número de chamadas Ajax que podem ser comunicadas](app-insights-javascript.md#detailed-configuration) em cada vista de página ou comutador desativar relatórios de Ajax.
* Desactivar módulos de coleção não precisa de por [editar Applicationinsights](app-insights-configuration-with-applicationinsights-config.md). Por exemplo, poderá decidir se os contadores de desempenho ou dados de dependência são inessential.
* Divida a telemetria para separar as chaves de instrumentação. 
* Pré-agrega as métricas. Se tiver coloca chamadas para TrackMetric na sua aplicação, pode reduzir o tráfego utilizando a sobrecarga que aceita o cálculo da média e o desvio-padrão de um lote de medidas. Ou pode utilizar um [previamente agregar pacote](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Gerir o volume de dados diária máximo

Pode utilizar a extremidade de volume diária para limitar os dados recolhidos, mas se a extremidade for cumprida, irá resultar em perda de toda a telemetria enviada a partir da sua aplicação para o resto do dia. É **não recomendado** ter a sua aplicação para atingiu o limite diário, uma vez que não é possível controlar o estado de funcionamento e desempenho da aplicação depois é atingido.

Em alternativa, utilize [amostragem](app-insights-sampling.md) para otimizar o volume de dados para o nível de seria como e utilizar a cap diária apenas como um "último recurso", no caso da aplicação começa a enviar muito superiores dos volumes de telemetria inesperadamente.

Para alterar a cap diária, na secção de configuração do recurso do Application Insights, o **utilização e os custos estimados** página, clique em **extremidade diária**.

![Ajustar a extremidade de volume de telemetria diária](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Amostragem
[Amostragem](app-insights-sampling.md) é um método de reduzir a velocidade a que a telemetria é enviada para a sua aplicação, mantendo a capacidade para encontrar eventos relacionados durante pesquisas de diagnóstico, e ainda manter evento correto contagens.

A amostragem é uma forma eficaz para reduzir os custos e permanecer dentro da sua quota mensal. O algoritmo de amostragem mantém relacionadas com itens de telemetria, para que, por exemplo, quando utilizar a pesquisa, pode encontrar o pedido relacionadas com uma exceção específica. O algoritmo mantém também contagens corretas, para que pode ver os valores corretos no Explorador de métrica de taxas de pedidos, taxas de exceção e outras contagens.

Existem várias formas de amostragem.

* [Amostragem adaptável](app-insights-sampling.md) é a predefinição para o SDK do ASP.NET, que se ajusta automaticamente para o volume de telemetria que envia a sua aplicação. Funciona automaticamente no SDK na sua aplicação web, para que o tráfego de telemetria na rede é reduzido. 
* *A amostragem de ingestão* é uma alternativa que opera no ponto onde a telemetria da sua aplicação introduz o serviço do Application Insights. Não afeta o volume de telemetria enviado a partir da sua aplicação, mas reduz o volume mantido pelo serviço. Pode utilizá-lo a reduzir a quota gastar pela telemetria de outros SDKs e browsers.

Para configurar a amostragem de ingestão, defina o controlo na caixa de diálogo de preços:

![A partir de Quota e a caixa de diálogo preço, clique no mosaico de exemplos e selecione uma fração de amostragem.](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> A caixa de diálogo de amostragem de dados controla apenas o valor da amostragem da ingestão. Esta não reflete a frequência de amostragem que está a ser aplicada pelo Application Insights SDK na sua aplicação. Se a telemetria de entrada já amostragem no SDK, amostragem de ingestão não está aplicada.
>

Para detetar a frequência de amostragem real, independentemente de onde foi aplicado, utilize um [consulta de análise](app-insights-analytics.md) como esta:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Em cada retidos registo, `itemCount` indica o número de registos originais que representa, igual a 1 + o número de registos rejeitados anteriores. 

## <a name="automation"></a>Automatização

Pode escrever um script para definir o plano de preços, utilizando a gestão de recursos do Azure. [Saiba como](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Resumo de limites

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Amostragem](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
