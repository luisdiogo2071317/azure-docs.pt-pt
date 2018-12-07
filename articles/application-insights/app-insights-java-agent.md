---
title: Monitorização de desempenho para aplicações web de Java no Azure Application Insights | Documentos da Microsoft
description: Estendidas de desempenho e monitorização da utilização do seu site de Java com o Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: a62c6cbb6594aaf7c2c57d7efee1832d7b1430b7
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "53000733"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Monitorizar dependências, exceções recebidas e tempos de execução do método em aplicações web Java


Se tiver [instrumentado a sua aplicação web de Java com o Application Insights][java], pode utilizar o agente Java para obter informações mais aprofundadas, sem quaisquer alterações de código:

* **Dependências:** dados sobre chamadas de que a aplicação faz para outros componentes, incluindo:
  * **Chamadas REST** feita por meio do HttpClient, OkHttp e RestTemplate (Spring) são capturadas.
  * **Redis** chamadas efetuadas através do cliente de Jedis são capturadas.
  * **[Chamadas JDBC](https://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -comandos do MySQL, SQL Server e Oracle DB são automaticamente capturados. Para o MySQL, se a chamada demora mais tempo do que 10s, o agente reporta o plano de consulta.
* **Pego exceções:** informações sobre as exceções que são processados pelo seu código.
* **Tempo de execução do método:** informações sobre o tempo que demora a executar métodos específicos.

Para utilizar o agente Java, instalá-lo no seu servidor. As aplicações web tem de ser equipadas com o [SDK de Java do Application Insights][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Instalar o agente do Application Insights para Java
1. No computador a executar o seu servidor de Java [transferir o agente](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Certifique-se para baixar o verson mesmo do agente de Java como pacotes de núcleo e web de Java SDK do Application Insights.
2. Edite o script de inicialização do servidor de aplicação e adicione o JVM seguinte:
   
    `javaagent:`*caminho completo para o ficheiro JAR do agente*
   
    Por exemplo, no Tomcat numa máquina Linux:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Reinicie o servidor de aplicação.

## <a name="configure-the-agent"></a>Configurar o agente
Crie um ficheiro denominado `AI-Agent.xml` e colocá-lo na mesma pasta que o ficheiro JAR do agente.

Defina o conteúdo do arquivo xml. Edite o exemplo seguinte para incluir ou omitir os recursos que pretende.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Tem de ativar a exceção de relatórios e o tempo de método para métodos individuais.

Por predefinição, `reportExecutionTime` é verdadeiro e `reportCaughtExceptions` é false.

## <a name="view-the-data"></a>Ver os dados
O recurso do Application Insights, agregados remotos dependência e o método tempos de execução aparece [sob o mosaico de desempenho][metrics].

Para procurar instâncias individuais de dependência, a exceção e o método de relatórios, abra [pesquisa][diagnostic].

[Diagnóstico de dependência de problemas - Saiba mais](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Tem dúvidas? Problemas?
* Não existem dados? [Exceções de firewall de conjunto](app-insights-ip-addresses.md)
* [Resolução de problemas de Java](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
