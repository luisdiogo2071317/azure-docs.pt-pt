---
title: Corrigir 502 gateway incorreto, 503 Serviço indisponíveis erros | Documentos da Microsoft
description: Resolva 502 gateway incorreto e 503 Serviço indisponível na sua aplicação web alojada no App Service do Azure.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: 502 gateway incorreto, 503 Serviço indisponível, o erro 503, Erro 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 1a64e40b13b05fc7f9fdb6f5aa99c8d8cc47c471
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251615"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Resolver erros HTTP "502 gateway inválido" e "503 Serviço indisponível" nas suas aplicações web do Azure
"502 gateway inválido" e "503 Serviço indisponível" são erros comuns na sua aplicação web alojada num [App Service do Azure](https://go.microsoft.com/fwlink/?LinkId=529714). Este artigo ajuda-o a resolver esses erros.

Se precisar de mais ajuda a qualquer momento neste artigo, pode contactar os especialistas do Azure no [do Azure do MSDN e os fóruns de Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, também pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e clique em **obter suporte**.

## <a name="symptom"></a>Sintoma
Ao navegar para a aplicação web, ele retorna um HTTP ou um pedido HTTP de erro de "502 Gateway incorreto" erro "503 Serviço indisponível".

## <a name="cause"></a>Causa
Esse problema normalmente é causado por problemas de nível de aplicativos, tais como:

* pedidos a demorar muito tempo
* aplicação com a memória/CPU elevada
* aplicativo falha devido a uma exceção.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Passos de resolução de problemas para resolver "502 gateway inválido" e erros de "503 Serviço indisponível"
Resolução de problemas pode ser dividida em três tarefas distintas, em ordem sequencial:

1. [Observar e monitorar o comportamento do aplicativo](#observe)
2. [Recolher dados](#collect)
3. [Mitigar o problema](#mitigate)

[Web Apps do App Service](app-service-web-overview.md) dá-lhe várias opções em cada passo.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Observar e monitorar o comportamento do aplicativo
#### <a name="track-service-health"></a>Controlar o estado de funcionamento do serviço
Microsoft Azure publicizes sempre que há uma degradação de desempenho ou interrupções de serviço. Pode acompanhar o estado de funcionamento do serviço no [Portal do Azure](https://portal.azure.com/). Para obter mais informações, consulte [controlar o estado de funcionamento do serviço](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorizar a sua aplicação web
Esta opção permite-lhe saber se a aplicação está a ter problemas. No painel da sua aplicação web, clique a **pedidos e erros** mosaico. O **métrica** painel mostrará a todas as métricas, pode adicionar.

Algumas das métricas que possa querer monitorizar para a sua aplicação web são

* Média do conjunto de trabalho de memória
* Tempo médio de resposta
* Tempo de CPU
* Conjunto de trabalho de memória
* Pedidos

![monitorizar a aplicação web na solução de erros HTTP 502 gateway incorreto e 503 Serviço indisponível](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Para obter mais informações, consulte:

* [Monitorizar aplicações da Web no serviço de aplicações do Azure](web-sites-monitor.md)
* [Receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Recolher dados
#### <a name="use-the-azure-app-service-support-portal"></a>Utilizar o Portal de suporte do serviço de aplicações do Azure
As aplicações Web fornece a capacidade de resolver problemas relacionados com a sua aplicação web ao observar a HTTP registos, registos de eventos, despejos de processo e muito mais. Pode acessar todas essas informações com o nosso portal de suporte em **http://&lt;o nome da aplicação >.scm.azurewebsites.net/Support**

O portal de suporte de serviço de aplicação do Azure fornece três separadores separados para suportar os três passos de um cenário de resolução de problemas comuns:

1. Observar o comportamento atual
2. Analisar a recolher informações de diagnóstico e executando os analisadores internos
3. Mitigar

Se o problema está acontecendo neste momento, clique em **Analyze** > **diagnósticos** > **diagnosticar agora** para criar uma sessão de diagnóstico, que recolha de registos de HTTP, registos do Visualizador de eventos, despejos, registos de erros PHP e o PHP processam o relatório de memória.

Quando os dados são recolhidos, também irá executar uma análise dos dados e fornecer-lhe um relatório HTML.

No caso de pretender transferir os dados, por padrão, ele seria armazenado na pasta D:\home\data\DaaS.

Para obter mais informações sobre o portal de suporte de serviço de aplicações do Azure, consulte [novas atualizações a extensão de Site de suporte para Web sites Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Utilizar a consola de depuração do Kudu
As aplicações Web é fornecido com uma consola de depuração que pode usar para depuração, explorar, carregar ficheiros, bem como pontos finais de JSON para obter informações sobre o seu ambiente. Isso é chamado do *consola Kudu* ou o *SCM Dashboard* para a sua aplicação web.

Pode aceder a este dashboard ao aceder à ligação **https://&lt;o nome da aplicação >.scm.azurewebsites.net/**.

Algumas das coisas que fornece o Kudu são:

* definições de ambiente para a sua aplicação
* fluxo de registo
* informação de diagnóstico
* consola na qual pode executar cmdlets do Powershell e comandos básicos de depuração.

Outro recurso útil do Kudu é que, no caso da aplicação que está a gerar exceções de primeira chance, pode utilizar o Kudu e despeja a ferramenta SysInternals Procdump para criar a memória. Esses despejos de memória são instantâneos do processo e, muitas vezes, podem ajudar a resolver problemas mais complicados, com a sua aplicação web.

Para obter mais informações sobre os recursos disponíveis no Kudu, consulte [Web sites do Azure ferramentas online, deve saber sobre](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Mitigar o problema
#### <a name="scale-the-web-app"></a>Dimensionar a aplicação web
No serviço de aplicações do Azure, para aumentar o desempenho e o débito, pode ajustar a escala a que está a executar a aplicação. Aumentar verticalmente uma aplicação web envolve duas ações relacionadas: alterar o seu plano do serviço de aplicações para um escalão de preço superior e a configuração de determinadas definições depois que tiver mudado para o escalão de preço superior.

Para obter mais informações sobre o dimensionamento, veja [dimensionar uma aplicação web no serviço de aplicações do Azure](web-sites-scale.md).

Além disso, pode optar por executar a sua aplicação em mais de uma instância. Isso não apenas fornece a mais capacidade de processamento, mas também fornece algum período de tolerância a falhas. Se o processo de ficar inativo numa instância, a outra instância irá continuar a servir pedidos.

Pode definir o dimensionamento Manual ou automático.

#### <a name="use-autoheal"></a>Utilizam o AutoHeal
AutoHeal recicla o processo de trabalho para a sua aplicação com base nas definições que escolher (como alterações de configuração, pedidos, limites baseados em memória ou o tempo necessário para executar um pedido). Na maioria das vezes, a forma mais rápida para recuperar a partir de um problema é a Reciclagem do processo. Embora sempre pode reiniciar a aplicação web a partir diretamente no Portal do Azure, AutoHeal fará automaticamente para. Tudo o que precisa fazer é adicionar alguns acionadores ao Web. config de raiz para a sua aplicação web. Tenha em atenção que estas definições funciona da mesma forma, mesmo que seu aplicativo não é um .net um.

Para obter mais informações, consulte [Autorrecuperação dos Web Sites Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Reinicie a aplicação web
Geralmente essa é a forma mais simples para recuperar rapidamente de problemas de uso individual. Sobre o [Portal do Azure](https://portal.azure.com/), no painel da sua aplicação web, tem as opções para parar ou reiniciar a aplicação.

 ![Reinicie a aplicação para resolver erros HTTP 502 gateway incorreto e 503 Serviço indisponível](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Também pode gerir a sua aplicação web com o Azure Powershell. Para obter mais informações, veja [Using Azure PowerShell with Azure Resource Manager (Utilizar o Azure PowerShell com o Azure Resource Manager)](../powershell-azure-resource-manager.md).

