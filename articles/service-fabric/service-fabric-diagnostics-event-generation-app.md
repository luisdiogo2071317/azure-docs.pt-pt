---
title: Nível de aplicação de recursos de infraestrutura de serviço do Azure monitorização | Microsoft Docs
description: Saiba mais sobre os registos de aplicações e eventos de nível de serviço e utilizados para monitorizar e diagnosticar os clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: 258aac722aa1c94ecf2cbf0524a3e4b53b8a788c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="application-and-service-level-logging"></a>Aplicação e o registo de nível de serviço

Instrumentação o código é a base para a maioria dos outros aspetos dos seus serviços de monitorização. Instrumentação é a única forma que pode saber que algo está errado e para diagnosticar o que tem de ser corrigidos. Embora seja tecnicamente possível ligar um depurador a um serviço de produção, não é uma prática comum. Por isso, ter detalhadas dados de instrumentação é importante.

Alguns produtos instrumentem automaticamente o seu código. Apesar destas soluções podem funcionar bem, instrumentação manual é quase sempre necessária. No final, tem de ter informações suficientes para forensically depurar a aplicação. Este documento descreve diferentes abordagens para instrumentar o seu código e escolher uma abordagem em vez de outro.

## <a name="eventsource"></a>EventSource

Quando cria uma solução de Service Fabric a partir de um modelo no Visual Studio, uma **EventSource**-classe derivada (**ServiceEventSource** ou **ActorEventSource**) é gerado . Um modelo é criado, na qual pode adicionar eventos para a aplicação ou serviço. O **EventSource** nome **tem** de ser exclusivo e deve ser alterado de cadeia de modelo predefinido aminhaempresa -&lt;solução&gt;-&lt;projeto &gt;. Ter vários **EventSource** definições que utilizam o mesmo nome provocar um problema em tempo de execução. Cada evento definido tem de ter um identificador exclusivo. Se um identificador não for exclusivo, ocorre uma falha de tempo de execução. Algumas organizações preassign intervalos de valores para os identificadores evitar conflitos entre as equipas de desenvolvimento separados. Para obter mais informações, consulte [blogue de Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) ou [documentação MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>O registo do ASP.NET Core

É importante planear cuidadosamente como irá instrumentar o seu código. O plano de instrumentação direita pode ajudar a evitar potencialmente destabilizing a base de código e, em seguida, necessitar de reinstrument o código. Para reduzir o risco, pode escolher uma biblioteca de instrumentação como [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), que faz parte do Microsoft ASP.NET Core. ASP.NET Core tem um [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface que pode utilizar com o fornecedor da sua escolha, para minimizar o efeito no código existente. Pode utilizar o código ASP.NET Core no Windows e Linux e, no .NET Framework completa, por isso, o código de instrumentação está padronizado.

## <a name="choosing-a-logging-provider"></a>Escolher um fornecedor de registo

Se a aplicação depende de elevado desempenho, **EventSource** é normalmente uma boa abordagem. **EventSource** *geralmente* utiliza menos recursos e efetua melhor do que o registo do ASP.NET Core ou qualquer uma das soluções de terceiros disponíveis.  Esta operação não é um problema para vários serviços, mas se o seu serviço é desempenho orientados, utilizando **EventSource** poderá ser uma melhor opção. No entanto obter estas vantagens do estruturados registo, **EventSource** requer um investimento superior da sua equipa de engenharia. Se for possível, não um protótipo rápido de algumas opções de registo e, em seguida, escolha a que melhor se adeque às suas necessidades.

## <a name="next-steps"></a>Passos Seguintes

Depois de escolher o fornecedor de registo instrumentem os seus serviços e aplicações, os registos e eventos precisam para serem agregados antes que podem ser enviadas para qualquer plataforma de análise. Leia sobre [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) e [WAD](service-fabric-diagnostics-event-aggregation-wad.md) para melhor compreender algumas das opções recomendadas.
