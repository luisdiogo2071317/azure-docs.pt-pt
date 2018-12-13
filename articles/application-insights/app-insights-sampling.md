---
title: Amostragem de telemetria no Application Insights do Azure | Documentos da Microsoft
description: Como manter o volume de telemetria sob controle.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/02/2018
ms.reviewer: vitalyg
ms.author: mbullwin
ms.openlocfilehash: 103f4b10d5fbb7fbcf9c3721a82fe4075abe0dc4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877620"
---
# <a name="sampling-in-application-insights"></a>Amostragem no Application Insights


A amostragem é uma funcionalidade do [do Azure Application Insights](app-insights-overview.md). É a forma recomendada para reduzir o tráfego de telemetria e de armazenamento, preservando uma análise estatística correta dos dados de aplicação. O filtro seleciona itens relacionados, para que pode navegar entre os itens quando estão a fazer investigações de diagnóstico.
Quando contagens de métricas são apresentadas a no portal, eles são renormalized para levar em conta a amostragem, para minimizar qualquer impacto nas estatísticas.

Amostragem reduz os custos de tráfego e os dados e ajuda a evitar a limitação.

## <a name="in-brief"></a>Em Resumo:
* Amostragem mantém 1 na *n* regista e descarta o resto. Por exemplo, ele poderá reter os eventos de 1 de 5, uma taxa de amostragem de 20%. 
* Amostragem ocorre automaticamente se a sua aplicação envia uma grande quantidade de telemetria, nas aplicações de servidor web do ASP.NET.
* Também pode definir manualmente de amostragem, no portal sobre a utilização e estimativa de custos de página; ou, no SDK do ASP.NET no arquivo. config, ou do SDK de Java no ficheiro applicationinsights. XML, também reduzir o tráfego de rede.
* Se iniciar a sessão de eventos personalizados e pretender certificar-se de que um conjunto de eventos é mantido ou descartado em conjunto, certifique-se de que têm o mesmo valor de OperationId.
* O divisor de amostragem *n* é comunicada em cada registo na propriedade `itemCount`, que na pesquisa aparece sob o nome amigável "contagem de pedidos" ou "contagem de eventos". Quando a amostragem não está em funcionamento, `itemCount==1`.
* Se escrever consultas de análise, deve [levar em conta amostragem](../azure-monitor/log-query/aggregations.md). Em particular, em vez de simplesmente contagem de registos, deve usar `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Tipos de amostragem
Existem três métodos alternativos de amostragem:

* **Amostragem adaptável** se ajusta automaticamente o volume de telemetria enviada do SDK na sua aplicação ASP.NET. A partir 2.0.0-Beta3 do SDK v isso é o método de amostragem padrão. Amostragem adaptável está atualmente disponível apenas para a telemetria de lado do servidor do ASP.NET. Para aplicativos do Asp.NET Core direcionamento Framework completo, a amostragem adaptável está disponível de versão 1.0.0 do SDK de Microsoft.ApplicationInsights.AspNetCore. Para aplicativos do Asp.NET Core direcionamento NetCore, a amostragem adaptável está disponível a partir 2.2.0-beta1 do Microsoft.ApplicationInsights.AspNetCore SDK.

* **Amostragem de taxa fixa** reduz o volume de telemetria enviada a partir de ambos os seu servidor ASP.NET ou Java e de browsers dos seus utilizadores. Definir a taxa. O cliente e o servidor irão sincronizar os seus amostragem, de modo que, a pesquisa em, pode navegar entre as vistas de página relacionado e pedidos.
* **Amostragem de ingestão** funciona no portal do Azure. Ele descartará alguns da telemetria que chega pela sua aplicação, a uma taxa de amostragem que definir. Ele não reduz o tráfego de telemetria enviado pela sua aplicação, mas ajuda a manter-se dentro da sua quota mensal. A principal vantagem de amostragem de ingestão é que pode definir a frequência de amostragem sem Reimplementar a sua aplicação e uniformemente funciona para todos os servidores e clientes. 

Se forem Adaptive ou fixo amostragem de taxa numa operação, a amostragem de ingestão está desativada.

## <a name="ingestion-sampling"></a>Amostragem de ingestão
Essa forma de amostragem opera no ponto onde a telemetria do seu servidor web, navegadores e dispositivos atinge o ponto de extremidade de serviço do Application Insights. Embora ele não reduz o tráfego de telemetria enviado pela sua aplicação, a reduzir a quantidade processados e mantidos (e é cobrada) pelo Application Insights.

Utilize este tipo de amostragem, se a sua aplicação, muitas vezes, são enviados pela sua quota mensal e não tem a opção de utilizar qualquer um dos tipos com base no SDK de amostragem. 

Defina a frequência de amostragem na utilização e a página de custos estimados:

![No painel de descrição geral da aplicação, clique em definições, Quota, amostras, em seguida, em seguida, selecione uma frequência de amostragem e clique em Atualizar.](./media/app-insights-sampling/04.png)

Como outros tipos de amostragem, o algoritmo mantém os itens de telemetria relacionados. Por exemplo, quando está analisando a telemetria na pesquisa, poderá encontrar o pedido relacionados com uma exceção em particular. Contagens de métrica, tais como a taxa de pedidos e taxa de exceções são mantidos corretamente.

Pontos de dados que são rejeitados pelo amostragem como não estão disponíveis em qualquer recurso do Application Insights [exportação contínua](app-insights-export-telemetry.md).

Amostragem de ingestão não funciona enquanto amostragem adaptável ou tarifa fixa baseada no SDK está em operação. Tenha em atenção que a amostragem adaptável está ativada por predefinição, quando o ASP.NET SDK está ativado no Visual Studio ou com o Monitor de estado e amostragem de ingestão está desativada. Se a taxa de amostragem, o SDK for inferior a 100%, em seguida, a taxa de amostragem de ingestão que definir é ignorada.

> [!WARNING]
> O valor mostrado no mosaico indica o valor que definiu para a amostragem de ingestão. Ele não representar a frequência de amostragem real se amostragem do SDK está em operação.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Amostragem adaptável em seu servidor web
Amostragem adaptável está disponível para o Application Insights SDK para 2.0.0-Beta3 v do ASP.NET e versões posteriores e está ativada por predefinição. 

Amostragem adaptável afeta o volume de telemetria enviada pela sua aplicação de servidor web para o ponto de final de serviço do Application Insights. O volume é ajustado automaticamente de manter uma taxa máxima especificada de tráfego.

Ele não funciona em baixos volumes de telemetria, portanto, uma aplicação na depuração ou um Web site com baixa utilização não será afetado.

Para obter o volume de destino, alguns da telemetria gerada é descartado. Mas, como outros tipos de amostragem, o algoritmo mantém os itens de telemetria relacionados. Por exemplo, quando está analisando a telemetria na pesquisa, poderá encontrar o pedido relacionados com uma exceção em particular. 

Contagens de métrica, tais como a taxa de pedidos e taxa de exceções são ajustados para compensar a taxa de amostragem, para que mostrem aproximadamente os valores corretos no Explorador de métricas.

### <a name="update-nuget-packages"></a>Atualizar pacotes NuGet ###

Atualizar pacotes de NuGet do seu projeto para o versão mais recente *pré-lançamento* versão do Application Insights. No Visual Studio, clique com botão direito do rato no Explorador de soluções, selecione gerir pacotes NuGet, verifique **incluir pré-lançamento** e procure o applicationinsights. 

### <a name="configuring-adaptive-sampling"></a>Configuração de amostragem adaptável ###

Na [applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md), pode ajustar vários parâmetros nos `AdaptiveSamplingTelemetryProcessor` nó. Os números mostrados são os valores predefinidos:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    A taxa de destino que o algoritmo do estímulo Objetiva **em cada anfitrião do servidor**. Se a sua aplicação web é executado em vários anfitriões, reduza este valor de modo a permanecer no seu ritmo de destino de tráfego no portal do Application Insights.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    O intervalo a que a taxa actual de telemetria é reavaliada. Avaliação é executada como uma média móvel. Pode querer diminuir este intervalo, se a telemetria é considerada como picos repentinos.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Quando as alterações de valor de percentagem de amostragem, como em breve afinal são nós permitidos para reduzir a percentagem de amostragem novamente para capturar dados com menos.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Quando as alterações de valor de percentagem de amostragem, como brevemente após são nós permitidos para aumentar a percentagem de amostragem novamente para capturar mais dados.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Como a percentagem de amostragem varia, o que é o valor mínimo que têm permissão para definir.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Como a percentagem de amostragem varia, o que é o que têm permissão para definir o valor máximo.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    O cálculo da média móvel, o peso atribuído para o valor mais recente. Utilize um valor igual ou inferior a 1. Valores menores efetuar o algoritmo de alterações de menos reativas para repentino.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    O valor atribuído quando acabou de iniciar a aplicação. Não reduzi-lo enquanto estiver a depurar. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Uma lista delimitada ponto e vírgula de tipos que não pretende ser objeto de amostragem. Reconhecido tipos são: dependência, eventos, exceções, visualização de página, Request, rastreio. Todas as instâncias dos tipos especificados são transmitidas; os tipos que não forem especificados são objeto de amostragem.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Uma lista delimitada ponto e vírgula de tipos que deseja ser objeto de amostragem. Reconhecido tipos são: dependência, eventos, exceções, visualização de página, Request, rastreio. Tipos especificados são objeto de amostragem; todas as instâncias dos outros tipos de sempre serão transmitidas.


**Para optar por desativar** adaptável de amostragem, a remover o nó AdaptiveSamplingTelemetryProcessor do Application insights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternativa: Configurar amostragem adaptável no código
Em vez de definir o parâmetro de amostragem no arquivo. config, pode definir programaticamente estes valores. Isto permite-lhe especificar uma função de retorno de chamada invocado sempre que a taxa de amostragem é reavaliada. Poderia usar isso, por exemplo, para descobrir a que taxa de amostragem está a ser utilizada.

Remover o `AdaptiveSamplingTelemetryProcessor` nó a partir do arquivo. config.

*C#*

```csharp

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Saiba mais sobre os processadores de telemetria](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Amostragem para páginas da web com JavaScript
Pode configurar páginas da web para a amostragem de taxa fixa a partir de qualquer servidor. 

Quando [configurar as páginas da web do Application Insights](app-insights-javascript.md), modificar o fragmento do JavaScript que a partir do portal do Application Insights. (Em aplicativos do ASP.NET, o fragmento normalmente vai no layout. cshtml.)  Insira uma linha, como `samplingPercentage: 10,` antes da chave de instrumentação:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Para a percentagem de amostragem, escolha uma percentagem que esteja próxima 100/N, onde N é um número inteiro.  Amostragem atualmente não suporta outros valores.

Se também ativar a amostragem de taxa fixa no servidor, os clientes e o servidor irão sincronizar, de modo que, a pesquisa em, pode navegar entre as vistas de página relacionado e pedidos.

## <a name="fixed-rate-sampling-for-aspnet-and-java-web-sites"></a>Amostragem de taxa fixa para web sites ASP.NET e Java
Taxa fixa amostragem reduz o tráfego enviado a partir do servidor web e navegadores da web. Ao contrário de amostragem adaptável, reduz a telemetria a um preço fixo decidido por. Ele também sincroniza o cliente e amostragem de servidor para que os itens relacionados são retidos - por exemplo, quando examinar uma vista de página na pesquisa, pode encontrar a solicitação relacionada.

O algoritmo de amostragem mantém os itens relacionados. Para cada solicitação HTTP eventos, o pedido e seus eventos relacionados são eliminados ou transmitidos em conjunto. 

No Explorador de métricas, tarifas, tais como contagens de pedido e exceção são multiplicadas por um fator para compensar a taxa de amostragem, para que fiquem aproximadamente corretos.

### <a name="configuring-fixed-rate-sampling-in-aspnet"></a>Configurar taxa fixa amostragem no ASP.NET ###

1. **Pacotes de NuGet do seu projeto de atualização** para o versão mais recente *pré-lançamento* versão do Application Insights. No Visual Studio, clique com botão direito do rato no Explorador de soluções, selecione gerir pacotes NuGet, verifique **incluir pré-lançamento** e procure o applicationinsights. 
2. **Desativar a amostragem adaptável**: na [applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md), remover ou comentar o `AdaptiveSamplingTelemetryProcessor` nó.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

3. **Ative o módulo de amostragem de taxa fixa.** Adicione este fragmento de código para [applicationinsights. config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

### <a name="configuring-fixed-rate-sampling-in-java"></a>Configurar a amostragem de taxa fixa em JAVA ###

1. Transferir e configurar a sua aplicação web com a versão mais recente [SDK de java do application insights](app-insights-java-get-started.md)

2. **Ativar o módulo de amostragem de taxa fixa** adicionando o fragmento seguinte ao ficheiro Applicationinsights XML.

```XML
    <TelemetryProcessors>
        <BuiltInProcessors>
            <Processor type = "FixedRateSamplingTelemetryProcessor">
                <!-- Set a percentage close to 100/N where N is an integer. -->
                <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                <Add name = "SamplingPercentage" value = "50" />
            </Processor>
        </BuiltInProcessors>
    <TelemetryProcessors/>
```

3. Pode incluir ou excluir determinados tipos de telemetria de amostragem usando as seguintes etiquetas dentro da marca de processador "FixedRateSamplingTelemetryProcessor"
```XML
    <ExcludedTypes>
        <ExcludedType>Request</ExcludedType>
    </ExcludedTypes>

    <IncludedTypes>
        <IncludedType>Exception</IncludedType>
    </IncludedTypes>
```
São os tipos de telemetria que podem ser incluídos ou excluídos da amostragem: dependência, eventos, exceções, visualização de página, pedido e rastreio.

> [!NOTE]
> Para a percentagem de amostragem, escolha uma percentagem que esteja próxima 100/N, onde N é um número inteiro.  Amostragem atualmente não suporta outros valores.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternativa: Ativar a amostragem de taxa fixa no código de servidor
Em vez de definir o parâmetro de amostragem no arquivo. config, pode definir programaticamente estes valores. 

*C#*

```csharp

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Saiba mais sobre os processadores de telemetria](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-to-use-sampling"></a>Quando utilizar amostragem?
Amostragem adaptável é ativada automaticamente se usar o ASP.NET SDK versão 2.0.0-Beta3 ou posterior. Independentemente de qual versão do SDK do que utilizar, pode ativar a amostragem de ingestão permitir que o Application Insights para os dados recolhidos de exemplo.

Por predefinição, nenhuma amostragem está ativada no SDK de Java. Atualmente, apenas suporta fixo de taxa de amostragem. Amostragem adaptável não é suportada no SDK de Java.

Em geral, para a maioria dos aplicativos de tamanho de pequeno e médio não precisa amostragem. As informações de diagnóstico mais úteis e estatísticas mais precisas são obtidas através da recolha de dados em todas as suas atividades de utilizador. 

São as principais vantagens da amostragem:

* Intervalo de tempo do Application Insights serviço cai ("limitadores") pontos de dados quando a aplicação envia uma taxa muito elevada de telemetria em Resumo. 
* Para manter o [quota](app-insights-pricing.md) de pontos de dados para o escalão de preço. 
* Para reduzir o tráfego de rede da coleção de telemetria. 

### <a name="which-type-of-sampling-should-i-use"></a>Que tipo de amostragem devo utilizar?
**Utilize a amostragem de ingestão se:**

* Muitas vezes passam por meio da sua quota mensal de telemetria.
* Está a utilizar uma versão do SDK que não suporta a amostragem - por exemplo, ASP.NET versões anteriores ao 2.
* Esteja tirando uma grande quantidade de telemetria a partir de browsers dos seus utilizadores.

**Utilize a tarifa fixa de amostragem se:**

* Está a utilizar o Application Insights SDK para a versão 2.0.0 do ASP.NET web services ou posterior ou o SDK de Java v2.0.1 ou posterior, e
* Pretende que a amostragem sincronizados entre cliente e servidor, para que, quando está investigando eventos na [pesquisa](app-insights-diagnostic-search.md), pode navegar entre os eventos relacionados no cliente e servidor, como vistas de página e solicitações http.
* Tiver a certeza de que a percentagem de amostragem apropriado para a sua aplicação. Deve ser alto o suficiente para obter uma métrica precisa, mas abaixo a taxa que excede a quota de preços e os limites de limitação. 

**Utilize a amostragem adaptável:**

Se não aplicar as condições para utilizar as outras formas de amostragem, recomendamos que amostragem adaptável. Esta opção estiver ativada por predefinição no servidor do ASP.NET SDK versão 2.0.0-Beta3 ou posterior. Não reduzir o tráfego até que uma taxa mínima é atingida, por conseguinte, sites de baixa utilização não serão afetados.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Como posso saber se a amostragem está em operação?
Para detetar a frequência de amostragem real, independentemente de onde ele foi aplicado, use um [consulta do Analytics](app-insights-analytics.md) como este:

```
union * 
| where timestamp > ago(1d)
| summarize 100/avg(itemCount) by bin(timestamp, 1h), itemType
| render timechart 
```

Em cada retidos registo, `itemCount` indica o número de registos originais que ele representa, igual a 1 + o número de registos de rejeitados anteriores. 

## <a name="how-does-sampling-work"></a>Como funciona a amostragem?
Recurso de amostragem de taxa fixa do SDK em versões do ASP.NET do 2.0.0 e o SDK de Java versão 2.0.1 e e posteriores. Amostragem adaptável é uma funcionalidade do SDK em versões do ASP.NET do 2.0.0 e posteriores. Amostragem de ingestão é uma funcionalidade do serviço do Application Insights e pode ser numa operação, se o SDK não está a efetuar amostragem. 

O algoritmo de amostragem decide quais itens de telemetria ao largar e quais podem manter (seja no SDK ou no serviço do Application Insights). A decisão de amostragem baseia-se a várias regras que têm como objetivo para preservar a todos os pontos de dados inter-relacionados intactos, manter uma experiência de diagnóstico no Application Insights, que é fiável, mesmo com um conjunto de dados reduzido e acionáveis. Por exemplo, se de um pedido falhado a sua aplicação enviar itens de telemetria adicional (como exceções e rastreios com sessão iniciados neste pedido), amostragem não divide este pedido e outra telemetria. Ele mantém ou remove-las todas as ferramentas. Como resultado, quando examinar os detalhes de pedido no Application Insights, pode sempre ver o pedido, juntamente com seus itens de telemetria associados. 

A decisão de amostragem baseia-se o id da operação de solicitação, o que significa que todos os itens de telemetria que pertencem a uma determinada operação é preservado ou removida. Para os itens de telemetria que não têm operação amostragem de conjunto (para itens de telemetria de exemplo reportada a partir de threads assíncronas com nenhum contexto de http) do id simplesmente captura uma percentagem de itens de telemetria de cada tipo. Antes de 2.5.0-beta2 do SDK do .NET e 2.2.0-beta3 do ASP.NET Core SDK, a decisão de amostragem tiveram como base o hash do id de utilizador para aplicações que definir "user" (ou seja, aplicativos de web mais comuns). Para os tipos de aplicativos que não definem utilizadores (tais como serviços da web) a decisão de amostragem foi com base no id da operação do pedido.

Ao apresentar a telemetria é com, o serviço Application Insights ajusta as métricas pela percentagem de amostragem mesmo que foi utilizada no momento da coleção, para compensar os pontos de dados em falta. Por conseguinte, ao ver a telemetria no Application Insights, os utilizadores estão a ver estatística corretas aproximações estão muito perto os números de real.

A precisão da aproximação depende em grande parte a percentagem de amostragem configurado. Além disso, a precisão aumenta para aplicações que processam um grande volume de pedidos em geral semelhantes de muitos usuários. Por outro lado, para aplicações que não funcionam com uma carga significativa, amostragem não é necessária como esses aplicativos geralmente podem enviar toda a sua telemetria, permanecendo dentro da quota, sem causar perda de dados de limitação. 

> [!WARNING]
> O Application Insights não de exemplo tipos de telemetria de métricas e sessões. Redução na precisão pode ser altamente desejável para estes tipos de telemetria.
> 

### <a name="adaptive-sampling"></a>Amostragem adaptável
Amostragem adaptável adiciona um componente que monitoriza a taxa actual de transmissão do SDK e ajusta a percentagem de amostragem para experimentar para se manterem dentro a velocidade máxima de destino. O ajuste é recalculado em intervalos regulares e se baseia numa média móvel da taxa de transmissão de saída.

## <a name="sampling-and-the-javascript-sdk"></a>Amostragem e o JavaScript SDK
O lado do cliente (JavaScript) SDK participa de amostragem de taxa fixa em conjunto com o SDK do lado do servidor. As páginas instrumentadas só irão enviar telemetria do lado do cliente dos mesmos utilizadores para o qual o servidor feitas a sua decisão de "exemplo em". Essa lógica foi concebida para manter a integridade da sessão de utilizador nos lados cliente e servidor. Como resultado, a partir de qualquer item de telemetria específico no Application Insights pode encontrar todos os outros itens de telemetria para este utilizador ou a sessão. 

*Meu cliente e a telemetria do lado do servidor não mostram exemplos coordenados como descrito acima.*

* Certifique-se de que ativou a amostragem de taxa fixa no servidor e cliente.
* Certifique-se de que a versão do SDK 2.0 ou superior.
* Verifique que defina a percentagem de amostragem mesmo no cliente e servidor.

## <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes
*Por que não é de amostragem um simples "recolher X por cento de cada tipo de telemetria"?*

* Embora essa abordagem de amostragem forneceria com uma precisão muito alta em aproximações métrica, iria quebrar a capacidade para correlacionar dados de diagnóstico por usuário, sessão e pedido, o que é essencial para obter um diagnóstico. Por conseguinte, a amostragem de funciona melhor com "recolher todos os telemetria itens para X por cento dos utilizadores da aplicação" ou "recolher toda a telemetria para X por cento dos pedidos de aplicação" lógica. Para os itens de telemetria não associados a pedidos (por exemplo, o processamento assíncrono em segundo plano), Outono volta é "recolher X por cento de todos os itens para cada tipo de telemetria." 

*Pode alterar a percentagem de amostragem ao longo do tempo?*

* Sim, amostragem adaptável altera gradualmente a percentagem de amostragem, com base no volume de telemetria enfrentado atualmente.

*Se eu utilizar amostragem de taxa fixa, como posso saber qual amostragem percentagem funcionará melhor para a minha aplicação?*

* Uma forma é começar com amostragem, Descubra o que classificá-la liquida (consulte a pergunta acima) e, em seguida, mude para a taxa fixa de amostragem com essa taxa. 
  
    Caso contrário, é preciso adivinhar. Analisar a utilização atual de telemetria no Application Insights, observar qualquer limitação que está a ocorrer e estimar o volume de telemetria recolhido. Estes três entradas, juntamente com o escalão de preço selecionado, sugerem quanto poderá reduzir o volume de telemetria recolhido. No entanto, um aumento no número de utilizadores ou alguns outro shift do volume de telemetria pode invalidar sua estimativa.

*O que acontece se eu configurar a percentagem de amostragem demasiado baixa?*

* Percentagem de amostragem excessivamente baixa (demasiado agressivas amostragem) reduz a precisão das aproximações, quando tenta do Application Insights compensar a visualização dos dados para a redução de volume de dados. Além disso, experiência de diagnóstico pode ser afetada negativamente, como alguns dos pedidos com pouca frequência falhar ou lentos podem ser objeto de amostragem horizontalmente.

*O que acontece se eu configurar a percentagem de amostragem muito alta?*

* Configurar a percentagem de amostragem demasiado elevada (não agressiva suficiente) resulta numa redução de insuficiente no volume de telemetria recolhido. Ainda pode ocorrer perda de dados de telemetria relacionados com a limitação e o custo de utilização do Application Insights pode ser maior do que planeada devido a custos de utilização excedida.

*Em que plataformas posso utilizar amostragem?*

* Amostragem de ingestão pode ocorrer automaticamente para qualquer telemetria acima de um determinado volume, se o SDK não está a efetuar amostragem. Isso funciona, por exemplo, se estiver a utilizar uma versão mais antiga do SDK do ASP.NET ou versão anterior do Java SDK(1.0.10 or before).
* Se estiver a utilizar versões do ASP.NET SDK 2.0.0 e superior (alojada no Azure ou no seu próprio servidor), obterá adaptável de amostragem por predefinição, mas pode mudar para a taxa fixa, tal como descrito acima. Com a amostragem de taxa fixa, o navegador SDK sincroniza automaticamente para a amostragem de eventos relacionados. 
* Se estiver a utilizar o SDK de Java versão 2.0.1 ou acima pode configurar o applicationinsights. XML para ativar a amostragem de taxa fixa. Amostragem está desativada por predefinição. Com a amostragem de taxa fixa, o navegador SDK sincroniza automaticamente para a amostragem de eventos relacionados.

*Existem determinados eventos raros, que quero ver sempre. Como posso obtê-los após o módulo de amostragem?*

* Inicialize uma instância separada do TelemetryClient com um novo TelemetryConfiguration (não a predefinição ativa). Usá-lo para enviar seus eventos raros.

## <a name="next-steps"></a>Passos Seguintes
* [Filtragem](app-insights-api-filtering-sampling.md) pode fornecer mais rigoroso controle de que o SDK envia.

