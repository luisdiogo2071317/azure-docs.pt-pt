---
title: Enterprise legado detalhes de plano de preços para o Azure Application Insights | Microsoft Docs
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
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 65307eab0bf1b5f502f11c14c369826cd12e0966
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309835"
---
# <a name="enterprise-plan-details"></a>Detalhes do plano de Enterprise

As informações de aplicação do Azure tem dois planos de preços: Basic e Enterprise. O [básico](app-insights-pricing.md) plano de preços é o plano predefinido. Inclui todas as funcionalidades do plano de Enterprise, sem custos adicionais. Faturas o plano básico principalmente no volume de dados é ingeridos. 

O plano de Enterprise tem cobrada uma taxa por nó, e cada nó recebe um allowance dados diários. A empresa preços plano, os dados é cobrados ingeridos acima o allowance incluído. Se estiver a utilizar o Operations Management Suite, deve escolher o plano de empresa. 

Para obter preços atuais na sua moeda e região, consulte [preços do Application Insights](http://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> Em Abril de 2018, iremos [introduzida](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) um novo modelo de preços para monitorização do Azure. Este modelo adopts um modelo simples "pay as you go" entre o portefólio completado de monitorização de serviços. Saiba mais sobre o [novo modelo de preços](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), como a [avaliar o impacto de mover para este modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) com base nos seus padrões de utilização, e [como optar ativamente por participar no modelo de novo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>Como funciona o plano de Enterprise

* Pagar para cada nó que envia a telemetria para todas as aplicações no plano de empresa.
 * A *nó* é um computador do servidor físico ou virtual ou uma instância de função de plataforma-como-um-serviço que aloja a aplicação.
 * Máquinas de desenvolvimento, os browsers cliente e dispositivos móveis não contam como nós.
 * Se a aplicação tiver vários componentes que enviam telemetria, como um serviço web e de um trabalho de back-end, os componentes são contados separadamente.
 * [Transmissão de velocidade de métricas](app-insights-live-stream.md) dados não estão contabilizados para fins de preço. Numa subscrição, os encargos são por nó, não por aplicação. Se tiver cinco nós que enviar a telemetria 12 aplicações, os encargos destina-se cinco nós.
* Embora os encargos são quoted por mês, está a cobrados apenas para qualquer hora em que um nó envia telemetria a partir de uma aplicação. A taxa de hora a hora é a taxa mensal delimitado por aspas dividida pelo 744 (o número de horas num mês de 31 dias).
* Para cada nó que é detetado (com granularidade horária) recebe uma atribuição de volume de dados de 200 MB por dia. Alocação de dados não utilizadas não está transportada através de um dia para o seguinte.
 * Se optar por empresa plano de preços, cada subscrição recebe um allowance diária de dados com base no número de nós que enviar a telemetria para os recursos do Application Insights nessa subscrição. Por isso, se tiver cinco nós que enviam dados todos os dias, terá um allowance agrupado de 1 GB aplicada a todos os recursos do Application Insights nessa subscrição. É irrelevante se determinados nós enviar mais dados dos outros nós, porque os dados incluídos são partilhados entre todos os nós. Se, num determinado dia, os recursos do Application Insights recebem mais dados do que está incluído na atribuição de dados diárias para esta subscrição, aplicam-se os encargos de dados excedido por GB. 
 * O diário allowance de dados é calculada como o número de horas do dia (utilizando a UTC) que cada nó envia a telemetria dividida pelo 24 multiplicado pelo 200 MB. Por isso, se tiver de quatro nós que enviam telemetria durante 15 de 24 horas do dia, os dados incluídos para esse dia seria ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. O preço 2.30 EUR por GB para excesso de dados, os encargos seria EUR 1.15 se os nós de envio de 1 GB de dados nesse dia.
 * O allowance diária do plano de empresa não está partilhado com aplicações para o qual tiver escolhido o plano básico. Allowance não utilizada não está transportada do quotidianas. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Exemplos de como determinar o número de nós distintos

| Cenário                               | Contagem total de nó diária |
|:---------------------------------------|:----------------:|
| 1 aplicação com 3 instâncias do App Service do Azure e o servidor virtual 1 | 4 |
| 3 aplicações em execução em 2 VMs; os recursos do Application Insights para estas aplicações estão na mesma subscrição e no plano de Enterprise | 2 | 
| 4 aplicações cujos recursos de informações de aplicações estão na mesma subscrição; cada aplicação em execução 2 instâncias 16 horas de ponta e 4 instâncias durante 8 horas de pico | 13.33 | 
| Serviços em nuvem com a função de trabalho 1 e 1 função da Web, cada 2 instâncias em execução | 4 | 
| Um cluster do Service Fabric do Azure de 5-nós com 50 micro-serviços; cada microsserviço 3 instâncias em execução | 5|

* A contagem de nó preciso depende em que o Application Insights SDK da aplicação está a utilizar. 
  * Em versões SDK 2.2 e posteriores, tanto o Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) e [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) cada anfitrião de aplicações como um nó de relatório. Os exemplos são o nome do computador para o servidor físico e anfitriões de VM ou o nome de instância para serviços em nuvem.  A única exceção é uma aplicação que utiliza apenas a [.NET Core](https://dotnet.github.io/) e o Application Insights Core SDK. Nesse caso, apenas um nó é comunicado para todos os anfitriões porque o nome do anfitrião não está disponível. 
  * Para versões anteriores do SDK, o [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) comportamento semelhante as versões mais recentes do SDK, mas o [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) relatórios apenas um nó, independentemente do número de anfitriões de aplicação. 
  * Se a sua aplicação utiliza o SDK ao definir **roleInstance** para um valor personalizado, por predefinição, esse mesmo valor é utilizado para determinar o número de nós. 
  * Se estiver a utilizar uma nova versão do SDK com uma aplicação que executa a partir de dispositivos móveis ou computadores cliente, a contagem de nós poderá devolver um número que é muito grande (devido ao elevado número de dispositivos móveis ou computadores cliente). 
