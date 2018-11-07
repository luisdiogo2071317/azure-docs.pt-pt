---
title: Monitorizar aplicações no serviço de aplicações do Azure | Documentos da Microsoft
description: Saiba como monitorizar aplicações no serviço de aplicações do Azure com o portal do Azure.
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: byvinyal
ms.openlocfilehash: 9c58e5c64ea3689634d7afb4c5fef08c9b21798c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244377"
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Como: monitorizar aplicações no serviço de aplicações do Azure
[Serviço de aplicações](https://go.microsoft.com/fwlink/?LinkId=529714) fornece a funcionalidade interna de monitorização no [portal do Azure](https://portal.azure.com).
O portal do Azure inclui a possibilidade de revisar **quotas** e **métricas** para uma aplicação, bem como o plano do serviço de aplicações, configurar **alertas** e, até mesmo **dimensionamento**  automaticamente com base nessas métricas.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Noções básicas sobre Quotas e métricas
### <a name="quotas"></a>Quotas
As aplicações alojadas no serviço de aplicações estão sujeitas a determinados *limites* nos recursos que podem utilizar. Os limites são definidos pelos **plano do App Service** associado à aplicação.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Se o aplicativo é hospedado num **gratuito** ou **partilhado** planear, em seguida, os limites para os recursos que pode utilizar a aplicação são definidos pelo **Quotas**.

Se o aplicativo é hospedado num **básica**, **padrão** ou **Premium** planear, em seguida, os limites nos recursos que podem utilizar são definidos pelo **tamanho**(Pequena, média, grande) e **contagem de instâncias** (1, 2, 3,...) da **plano do App Service**.

**Quotas** para **gratuito** ou **partilhado** as aplicações são:

* **CPU(short)**
  * Quantidade de CPU permitida para esta aplicação num intervalo de 5 minutos. Esta quota repõe a cada cinco minutos.
* **CPU(Day)**
  * Quantidade total de CPU permitida para esta aplicação num dia. Esta quota repõe a cada 24 horas à meia-noite UTC.
* **Memória**
  * Quantidade total de memória permitida para esta aplicação.
* **Largura de banda**
  * Quantidade total de largura de banda de saída permitida para esta aplicação num dia.
    Esta quota repõe a cada 24 horas à meia-noite UTC.
* **Sistema de ficheiros**
  * Quantidade total de armazenamento permitida.

A quota apenas aplicável para aplicações alojadas nos **básica**, **padrão**, e **Premium** planos é **sistema de ficheiros**.

Obter mais informações sobre as quotas específicas, limites e recursos disponíveis para os SKUs de serviço de aplicação diferente podem ser encontradas aqui: [limites de serviço de subscrição do Azure](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Imposição de quota
Se um aplicativo exceder os **CPU (curto)**, **CPU (dia)**, ou **largura de banda** quota, em seguida, o aplicativo está parado até que repõe a quota. Durante este período, todos os pedidos recebidos resultam numa **HTTP 403**.
![][http403]

Se o aplicativo **memória** das quotas for excedida, em seguida, o aplicativo for reiniciado.

Se o **sistema de ficheiros** das quotas for excedida, em seguida, qualquer escrever operação falhar, que inclui qualquer escritas nos registos.

As quotas podem ser aumentadas ou removidas da aplicação ao atualizar o seu plano do serviço de aplicações.

### <a name="metrics"></a>Métricas
**Métricas** fornecem informações sobre a aplicação ou o comportamento do plano do serviço de aplicações.

Para uma **aplicativo**, as métricas disponíveis são:

* **Tempo médio de resposta**
  * O tempo médio despendido para a aplicação atender a solicitações em ms.
* **Conjunto de trabalho de memória média**
  * A quantidade média de memória em MiBs utilizados pela aplicação.
* **Tempo de CPU**
  * A quantidade de CPU em segundos consumidos pela aplicação. Para obter mais informações sobre esta métrica, consulte: [percentagem de CPU do vs de tempo de CPU](#cpu-time-vs-cpu-percentage)
* **Dados em**
  * A quantidade de largura de banda de entrada consumida pela aplicação no MiBs.
* **Saída de dados**
  * A quantidade de largura de banda de saída consumida pela aplicação no MiBs.
* **2xx de HTTP**
  * Contagem dos pedidos, resultando num código de estado HTTP de > = 200 mas < 300.
* **Http 3xx**
  * Contagem dos pedidos, resultando num código de estado HTTP de > = 300 mas < 400.
* **HTTP 401**
  * Contagem de pedidos, resultando em código de estado HTTP 401.
* **HTTP 403**
  * Contagem de pedidos, resultando em código de estado HTTP 403.
* **HTTP 404**
  * Contagem de pedidos, resultando em código de estado de HTTP 404.
* **HTTP 406**
  * Contagem de pedidos, resultando em código de estado HTTP 406.
* **Http 4xx**
  * Contagem dos pedidos, resultando num código de estado HTTP de > = 400 mas < 500.
* **Erros de servidor HTTP**
  * Contagem dos pedidos, resultando num código de estado HTTP de > = 500 mas < 600.
* **Conjunto de trabalho de memória**
  * Quantidade atual de memória utilizada pela aplicação no MiBs.
* **Pedidos**
  * Número total de pedidos independentemente do seu código de estado HTTP resultante.

Para uma **plano do App Service**, as métricas disponíveis são:

> [!NOTE]
> Métricas do plano de serviço de aplicações só estão disponíveis para planos **básica**, **padrão**, e **Premium** escalões.
> 
> 

* **Percentagem de CPU**
  * A CPU média utilizada em todas as instâncias do plano.
* **Percentagem de memória**
  * Média de memória utilizada em todas as instâncias do plano.
* **Dados em**
  * A média entrada largura de banda utilizada em todas as instâncias do plano.
* **Saída de dados**
  * A média de largura de banda utilizada em todas as instâncias do plano de saída.
* **Comprimento da fila de disco**
  * O número médio de leitura e pedidos que foram colocados em fila de escrita no armazenamento. Um comprimento de fila de disco elevados é uma indicação de um aplicativo que pode ser mais lento devido a e/s de disco excessiva.
* **Comprimento da fila de HTTP**
  * O número médio de pedidos HTTP que tinha sentar-se na fila antes de a ser concluído. Um comprimento de fila de HTTP alto ou crescente é um sintoma de um plano com muita carga.

### <a name="cpu-time-vs-cpu-percentage"></a>Percentagem de CPU do vs de tempo de CPU
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Existem duas métricas que refletem a utilização da CPU. **Tempo de CPU** e **percentagem de CPU**

**Tempo de CPU** é útil para as aplicações alojadas no **gratuito** ou **partilhado** planos, uma vez que uma das suas quotas é definida em minutos de CPU utilizados pela aplicação.

**Percentagem de CPU** é útil para as aplicações alojadas no **básica**, **padrão**, e **premium** planos, uma vez que podem ser aumentados horizontalmente. Percentagem de CPU é uma boa indicação de que o uso geral em todas as instâncias.

## <a name="metrics-granularity-and-retention-policy"></a>Granularidade de métricas e política de retenção
As métricas para uma aplicação e o plano do serviço de aplicações são registadas e agregadas pelo serviço com as políticas de retenção e nas duas granularidades seguintes:

* **Minuto** métricas de granularidade são mantidas durante **30 horas**
* **Hora** métricas de granularidade são mantidas durante **30 dias**
* **Dia** métricas de granularidade são mantidas durante **30 dias**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Monitorizar Quotas e as métricas no portal do Azure.
Pode rever o estado dos diferentes **quotas** e **métricas** afetar um aplicativo no [portal do Azure](https://portal.azure.com).

![][quotas]
**Quotas** pode ser encontrado em Definições >**Quotas**. A experiência do Usuário permite-lhe rever: (1) o nome de quotas, (2) o intervalo de reposição, (3) o limite atual e o valor (4) atual.

![][metrics]
**Métricas** podem ser acedidos diretamente a partir da página de recursos. Também pode personalizar o gráfico por: (1) **clique em** e selecione (2) **editar gráfico**.
Aqui pode alterar (3) **intervalo de tempo**, (4) **tipo de gráfico**e (5) **métricas** para apresentar.  

Pode saber mais sobre as métricas aqui: [monitorizar as métricas de serviço](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Alertas e o dimensionamento automático
As métricas para um plano de aplicação ou serviço de aplicações podem ser conectadas a alertas. Para saber mais sobre esse assunto, veja [receber notificações de alerta](../monitoring-and-diagnostics/insights-alerts-portal.md).

Aplicações de serviço de aplicações alojadas no básico, standard ou premium suporte de planos de serviço de aplicações **dimensionamento automático**. Dimensionamento automático permite-lhe configurar regras que monitoram as métricas de plano de serviço de aplicações. Regras podem aumentar ou diminuir a contagem de instâncias fornecem recursos adicionais, conforme necessário. As regras também podem ajudar a poupar dinheiro quando o aplicativo é provisionado excessiva. Pode saber mais sobre aqui a escala automática: [como dimensionar](../monitoring-and-diagnostics/insights-how-to-scale.md) e, aqui [melhores práticas para o dimensionamento automático do Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Se pretender começar a utilizar o App Service do Azure antes de se inscrever numa conta do Azure, aceda a [Experimentar o App Service](https://azure.microsoft.com/try/app-service/), onde pode criar de imediato uma aplicação Web de arranque de curta duração no App Service. Sem cartões de crédito; sem compromissos.
> 
> 

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
