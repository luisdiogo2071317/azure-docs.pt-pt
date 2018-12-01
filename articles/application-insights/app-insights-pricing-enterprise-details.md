---
title: Enterprise herdado detalhes do plano de preços do Azure Application Insights | Documentos da Microsoft
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
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 820c1baaf920dc2ada2aba1a4a8cc3b11ab2db2d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726819"
---
# <a name="enterprise-plan-details"></a>Detalhes do plano de Enterprise

O Azure Application Insights tem dois planos de preços: básico e empresarial. O [básica](app-insights-pricing.md) plano de preços é o plano predefinido. Inclui todas as funcionalidades do plano de Enterprise, sem custos adicionais. As listas de plano básico, principalmente no volume de dados que são ingeridos. 

O plano de Enterprise tem uma cobrança por nó e cada nó recebe um montante diário de dados. Na empresa preços do plano, é-lhe cobrada aos dados ingeridos acima do montante incluído. Se estiver a utilizar o Operations Management Suite, escolha o plano Enterprise. 

Para obter preços atuais na sua moeda e região, veja [os preços do Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> Em Abril de 2018, vamos [introduzida](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) um novo modelo de preços para a monitorização do Azure. Esse modelo adota um modelo simples de "pay as you go" em todo o portefólio completado dos serviços de monitorização. Saiba mais sobre o [novo modelo de preços](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), como a [avaliar o impacto da migração para esse modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) com base nos seus padrões de utilização, e [como otimizado para o novo modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>Como funciona o plano Enterprise

* Paga para cada nó que envia telemetria para todas as aplicações no plano do Enterprise.
 * R *nó* é uma máquina física ou virtual server ou uma instância de função de plataforma-como-serviço que aloja a aplicação.
 * Máquinas de desenvolvimento, os browsers cliente e dispositivos móveis não contam como nós.
 * Se a sua aplicação tem vários componentes que enviam telemetria, como um serviço da web e uma função de trabalho back-end, os componentes são contados em separado.
 * [Live Stream métricas](app-insights-live-stream.md) dados não são contabilizados para fins de preços. Numa subscrição, as cobranças são por nó, não por aplicação. Se tiver cinco nós que enviam telemetria para 12 aplicações, o custo é para cinco nós.
* Embora os encargos são apresentados por mês, é-lhe cobrado apenas para a qualquer hora em que um nó envia telemetria a partir de uma aplicação. O custo por hora é o custo mensal com aspas simples, dividido por 744 (o número de horas no mês de 31 dias).
* Recebe uma alocação de volume de dados de 200 MB por dia para cada nó detetado (com granularidade de hora a hora). Alocação de dados não utilizados não é transportada através de um dia para a próxima.
 * Se escolher a empresa plano de preços, cada subscrição recebe um montante diário de dados com base no número de nós que enviam telemetria para os recursos do Application Insights nessa subscrição. Então, se tiver cinco nós que enviam dados todo o dia, terá um volume total permitido de 1 GB aplicada a todos os recursos do Application Insights nessa subscrição. Não importa se alguns nós enviam mais dados do que outros nós, uma vez que os dados incluídos são partilhados por todos os nós. Se, num determinado dia, os recursos do Application Insights recebem mais dados do que está incluído na alocação diária de dados para esta subscrição, se aplicam os encargos de dados excedidos por GB. 
 * O volume de dados diário é calculado como o número de horas do dia (utilizando a UTC) que cada nó envia telemetria, dividido por 24 multiplicado por 200 MB. Por isso, se tiver quatro nós que enviam telemetria durante 15 das 24 horas do dia, os dados incluídos nesse dia seriam ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. O preço de USD 2.30 por GB de dados excedida, o custo seria USD 1.15 se os nós de envio de 1 GB de dados nesse dia.
 * O montante diário de plano de Enterprise não serem partilhado com aplicações para o qual optou pelo plano básico. Montante não utilizado não é herdado do dia a dia. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Exemplos de como determinar a contagem de nós distintos

| Cenário                               | Contagem total de nó diária |
|:---------------------------------------|:----------------:|
| 1 aplicação usando 3 instâncias de serviço de aplicações do Azure e 1 servidor virtual | 4 |
| aplicações de 3 em execução em 2 VMs; os recursos do Application Insights para estas aplicações estão na mesma subscrição e no plano do Enterprise | 2 | 
| 4 aplicativos cujos recursos de informações de aplicativos estão na mesma subscrição; cada aplicativo executar 2 instâncias 16 horas de ponta e 4 instâncias durante 8 horas de pico | 13.33 | 
| Serviços cloud com 1 função de trabalho e 1 função da Web, sendo que a execução de 2 instâncias | 4 | 
| Um cluster do Azure Service Fabric de 5 nós executar 50 microsserviços; cada microsserviço 3 instâncias em execução | 5|

* A contagem de nós precisa depende em qual SDK do Application Insights a aplicação está a utilizar. 
  * Nas versões SDK 2.2 e posteriores, tanto o Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) e o [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) cada anfitrião de aplicação como um nó de relatório. Os exemplos são o nome do computador para o servidor físico e anfitriões VM ou o nome da instância para serviços em nuvem.  A única exceção é uma aplicação que utiliza apenas a [.NET Core](https://dotnet.github.io/) e o SDK do Application Insights básico. Nesse caso, apenas um nó é comunicado para todos os anfitriões porque o nome do anfitrião não está disponível. 
  * Para versões anteriores do SDK, o [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) se comporta como as versões mais recentes do SDK, mas o [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) relatórios apenas um nó, independentemente do número de anfitriões de aplicação. 
  * Se o aplicativo utilizar o SDK para definir **roleInstance** para um valor personalizado, por padrão, esse mesmo valor é utilizado para determinar a contagem de nós. 
  * Se estiver a utilizar uma nova versão do SDK com uma aplicação que é executado a partir de dispositivos móveis ou computadores cliente, a contagem de nós pode devolver um número que é muito grande (devido ao elevado número de dispositivos móveis ou computadores cliente). 
