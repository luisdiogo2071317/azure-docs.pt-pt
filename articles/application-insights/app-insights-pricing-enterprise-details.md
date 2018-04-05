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
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 6e7591ccf0f21099474a08dda088422c377135f6
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="enterprise-plan-details"></a>Detalhes do plano de Enterprise

O Application Insights tem dois planos de preços. O plano predefinido é chamado [básico](app-insights-pricing.md), que inclui todas as funcionalidades como o plano de Enterprise, sem custos de adição e faturas principalmente no volume de dados ingeridos mesmas. Se estiver a utilizar o Operations Management Suite, deve escolher para o plano, de que tem um por nó cobram juntamente com diariamente allowances de dados e, em seguida, cobrará um encargo para ingeridos acima o allowance incluído os dados da empresa.

Consulte o [página de preços do Application Insights](http://azure.microsoft.com/pricing/details/application-insights/) para os preços atuais na sua região e moeda.

## <a name="heres-how-the-enterprise-plan-works"></a>Eis como funciona o plano de Enterprise

* Paga por nó que está a enviar telemetria para todas as aplicações no plano de empresa.
 * A *nó* é um computador do servidor físico ou virtual ou uma instância de função de plataforma-como-um-serviço, o que aloja a aplicação.
 * Máquinas de desenvolvimento, os browsers cliente e dispositivos móveis não são contabilizados como nós.
 * Se a aplicação tiver vários componentes que enviam telemetria, como um serviço web e de um trabalho de back-end, estes são contadas separadamente.
 * [Transmissão de velocidade de métricas](app-insights-live-stream.md) dados não estão contados para os preços purposes.* através de uma subscrição, os encargos são por nó, não por aplicação. Se tiver cinco nós que envia a telemetria para 12 aplicações, em seguida, os encargos destina-se a cinco nós.
* Embora os encargos são quoted por mês, está a cobrados apenas para qualquer hora em que um nó envia telemetria a partir de uma aplicação. A taxa de hora a hora é a taxa mensal delimitado por aspas / 744 (o número de horas num mês de 31 dias).
* Para cada nó detetado (com granularidade horária) recebe uma atribuição de volume de dados de 200 MB por dia. Alocação de dados não utilizados não é executada de um dia para o seguinte.
 * Se escolher a opção de preços do Enterprise, cada subscrição recebe um allowance diária de dados com base no número de nós que envia a telemetria para os recursos do Application Insights nessa subscrição. Por isso, se tiver 5 nós enviar dados de todos os dias, terá um allowance agrupado de 1 GB aplicada a todos os recursos do Application Insights nessa subscrição. É irrelevante se determinados nós estão a enviar mais dados dos outros nós porque os dados incluídos são partilhados entre todos os nós. Se, num determinado dia, os recursos do Application Insights recebem mais dados do que está incluído na atribuição de dados diárias para esta subscrição, aplicam-se os encargos de dados excedido por GB. 
 * O diário allowance de dados é calculada como o número de horas do dia (utilizando a UTC) que cada nó está a enviar telemetria dividida pelo 200 MB de 24 horas. Para que o se tiver de 4 nós que envia a telemetria durante 15 de 24 horas do dia, os dados incluídos para esse dia seria ((4 x 15) / 24) x 200 MB = 500 MB. O preço 2.30 EUR por GB para excesso de dados, os encargos seria EUR 1.15 se os nós de envio de 1 GB de dados nesse dia.
 * Enterprise allowance diárias do plano não está partilhado com aplicações para o qual tiver escolhido a opção básica e allowance não utilizada não transportada diárias do. 

## <a name="here-are-some-examples-of-determining-distinct-node-count"></a>Seguem-se alguns exemplos de determinar o número de nós distintos

| Cenário                               | Contagem total de nó diária |
|:---------------------------------------|:----------------:|
| 1 aplicação está a utilizar 3 instâncias do App Service do Azure e o servidor virtual 1 | 4 |
| 3 aplicações em execução no 2 VMs e os recursos do Application Insights para estas aplicações estão na mesma subscrição e no plano de Enterprise | 2 | 
| 4 aplicações cujos recursos de informações de aplicações estão na mesma subscrição. Cada aplicação é executada 2 instâncias 16 horas de ponta e 4 instâncias durante 8 horas de pico. | 13.33 | 
| Serviços em nuvem com a função de trabalho 1 e 1 função da Web, cada 2 instâncias em execução | 4 | 
| executar 50 microserviços, cada serviço de micro 3 instâncias em execução de Cluster do nó de 5 Service Fabric | 5|

* O nó preciso contando comportamento depende em que a aplicação do Application Insights SDK está a utilizar. 
  * Em versões do SDK 2.2 e em diante, tanto o Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) ou [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) será relatório cada anfitrião de aplicações como um nó, por exemplo, o nome do computador para o servidor físico e anfitriões de VM ou o nome da instância no caso de serviços em nuvem.  A única exceção é aplicações utilizando apenas [.NET Core](https://dotnet.github.io/) e o Application Insights Core SDK, na qual maiúsculas apenas um nó será reportado para todos os anfitriões porque o nome do anfitrião não está disponível. 
  * Para versões anteriores do SDK, o [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) irão comportar-se tal como as versões mais recentes do SDK, no entanto, a [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) comunicará apenas um nó, independentemente do número de anfitriões de aplicação real. 
  * Se a aplicação estiver a utilizar o SDK ao definir roleInstance para um valor personalizado, por predefinição esse mesmo valor será utilizado para determinar o número de nós. 
  * Se estiver a utilizar uma nova versão do SDK com uma aplicação que é executada a partir de dispositivos móveis ou computadores cliente, é possível que a contagem de nós pode devolver um número que é muito grande (a partir de elevado número de dispositivos móveis ou computadores cliente). 
