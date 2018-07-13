---
title: Comparar Flow, Logic Apps, Functions, e WebJobs - Azure
description: 'Compare serviços cloud da Microsoft que estão otimizados para tarefas de integração:  Flow, Logic Apps, Functions e WebJobs.'
services: functions,app-service\logic
documentationcenter: na
author: tdykstra
manager: cfowler
tags: ''
keywords: microsoft flow, flow, fluxo, logic apps, aplicações lógicas, funções do azure, funções, azure webjobs, webjobs, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/09/2018
ms.author: tdykstra
ms.custom: mvc
ms.openlocfilehash: 9b9ef150a86a754d4ed328184eb02c1937008b7b
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970143"
---
# <a name="compare-flow-logic-apps-functions-and-webjobs"></a>Comparar Flow, Logic Apps, Functions e WebJobs

Este artigo compara os seguintes serviços cloud do Microsoft:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Funções do Azure](https://azure.microsoft.com/services/functions/)
* [WebJobs do Serviço de Aplicações do Azure ](../app-service/web-sites-create-web-jobs.md)

Todos estes serviços podem resolver problemas de integração e automatizar processos comerciais. Podem todos definir entradas, ações, condições e saídas. Pode executar cada um com base numa agenda ou num acionador. No entanto, cada serviço tem vantagens únicas e este artigo explica as diferenças.

## <a name="compare-microsoft-flow-and-azure-logic-apps"></a>Comparar o Microsoft Flow e o Azure Logic Apps

O Flow e o Logic Apps são serviços de integração *para designers* que podem criar fluxos de trabalho. Ambos os serviços integram-se com diversas aplicações empresariais e SaaS. 

O Flow baseia-se no Logic Apps. Partilham o mesmo estruturador de fluxo de trabalho e os mesmos [Conectores](../connectors/apis-list.md). 

O Flow capacita qualquer trabalhador de escritório para efetuar integrações simples (por exemplo, um processo de aprovação numa Biblioteca de Documentos do SharePoint) sem recorrer a programadores ou às TI. Por outro lado, o Logic Apps pode permitir integrações avançadas (por exemplo, processos B2B) que exigem práticas de DevOps e de segurança de nível empresarial. É normal um fluxo de trabalho do negócio se tornar cada vez mais complexo ao longo do tempo. Assim, pode começar primeiro com um fluxo e depois convertê-lo numa aplicação lógica, conforme necessário.

A tabela seguinte ajuda-o a determinar se o Flow ou o Logic Apps é o melhor para uma determinada integração.

|  | Fluxo | Aplicações Lógicas |
| --- | --- | --- |
| Utilizadores |Trabalhadores do escritório, utilizadores empresariais, administradores do SharePoint |Integradores e programadores profissionais, profissionais de TI |
| Cenários |Gestão personalizada |Integrações avançadas |
| Ferramenta de estruturação |Aplicação no browser e móvel, apenas IU |No browser e [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Vista de código](../logic-apps/logic-apps-author-definitions.md) disponível |
| Application Lifecycle Management (ALM) |Conceba e teste em ambientes de não produção e promova a produção quando estiver pronto. |DevOps: controlo da origem, teste, suporte, automatização e capacidade de gestão na [Gestão de Recursos do Azure](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Experiência de administrador |Gerir políticas de Prevenção de Perda de Dados (DLP) e Ambientes do Flow, controlar o licenciamento [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Gerir Grupos de Recursos, Ligações, Gestão de Acesso e Registos [https://portal.azure.com](https://portal.azure.com) |
| Segurança |Registos de auditoria de Segurança e Conformidade do Office 365, Prevenção de Perda de Dados (DLP), [encriptação inativa](https://wikipedia.org/wiki/Data_at_rest#Encryption) para dados confidenciais, etc. |Garantia de segurança do Azure: [Segurança do Azure](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity), [Centro de Segurança](https://azure.microsoft.com/services/security-center/), [registos de auditoria](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) e muito mais. |

## <a name="compare-azure-functions-and-azure-logic-apps"></a>Comparar o Azure Functions e o Azure Logic Apps

O Functions e o Logic Apps são serviços do Azure que permitem cargas de trabalho sem servidor. O Azure Functions é um serviço de computação sem servidor, ao passo que o Azure Logic Apps fornece fluxos de trabalho sem servidor. As *orquestrações* complexas podem ser criadas por ambos. Uma orquestração é uma coleção de funções ou passos, chamados *ações* no Logic Apps, que são executados para cumprir tarefas complexas. Por exemplo, para processar um lote de encomendas, pode executar muitas instâncias de uma função em paralelo, aguardar até que todas as instâncias terminem e, em seguida, executar uma função que calcule um resultado agregado.

Para o Azure Functions, desenvolva orquestrações ao escrever código e utilizar a [extensão Durable Functions](durable-functions-overview.md) (em pré-visualização). Para o Logic Apps, crie orquestrações ao utilizar um GUI ou editar os ficheiros de configuração.

Pode combinar serviços ao criar uma orquestração, chamar funções de aplicações lógicas e chamar aplicações lógicas de funções. Selecione como criar cada orquestração com base nas capacidades dos serviços ou nas suas preferências pessoais. A seguinte tabela lista algumas das principais diferenças entre estes serviços:
 
|  | Funções Duráveis | Aplicações Lógicas |
| --- | --- | --- |
| Desenvolvimento | Baseada em código (imperativo) | Baseada em designer (declarativo) |
| Conectividade | [Cerca de uma dúzia de tipos de vinculação integrados](functions-triggers-bindings.md#supported-bindings), código de escrita para vinculações personalizadas | [Grande coleção de conectores](../connectors/apis-list.md), [Enterprise Integration Pack para cenários B2B](../logic-apps/logic-apps-enterprise-integration-overview.md), [criar conectores personalizados](../logic-apps/custom-connector-overview.md) |
| Ações | Cada atividade é uma função do Azure; escrever código para funções de atividades |[Grande coleção de ações preparadas](../logic-apps/logic-apps-workflow-actions-triggers.md)|
| Monitorização | [Azure Application Insights](../application-insights/app-insights-overview.md) | [Portal do Azure](../logic-apps/quickstart-create-first-logic-app-workflow.md), [Operations Management Suite](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md), [Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md)|
| Gestão | [API REST](durable-functions-http-api.md), [Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer) | [Portal do Azure](../logic-apps/quickstart-create-first-logic-app-workflow.md), [API REST](https://docs.microsoft.com/rest/api/logic/), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp/?view=azurermps-5.6.0), [Visual Studio](https://docs.microsoft.com/azure/logic-apps/manage-logic-apps-with-visual-studio) |
| Contexto de execução | Pode executar o [localmente](functions-runtime-overview.md) ou na cloud. | Executado apenas na cloud.|

<a name="function"></a>

## <a name="compare-functions-and-webjobs"></a>Comparar o Functions e o WebJobs

Tal como as Funções do Azure, o Serviço de Aplicações do Azure WebJobs com o SDK do WebJobs é um serviço de integração *baseado em código* e concebido para programadores. Ambos são criados no [Serviço de Aplicações do Azure](../app-service/app-service-web-overview.md) e suportam funcionalidades como a [integração de controlo de origem](../app-service/app-service-continuous-deployment.md), a [autenticação](../app-service/app-service-authentication-overview.md) e a [monitorização com integração do Application Insights](functions-monitoring.md).

### <a name="webjobs-and-the-webjobs-sdk"></a>WebJobs e o SDK do WebJobs

A funcionalidade *WebJobs* do Serviço de Aplicações permite-lhe executar um script ou código no contexto de uma aplicação Web do Serviço de Aplicações. O *SDK do WebJobs* é uma arquitetura concebida para o WebJobs que simplifica o código que escreveu para responder a eventos em serviços do Azure. Por exemplo, pode responder à criação de um blob de imagem no Armazenamento do Microsoft Azure ao criar uma imagem de miniatura. O SDK do WebJobs é executado como uma aplicação da consola .NET, a qual pode implementar num WebJob. 

O WebJobs e o SDK do WebJobs funcionam melhor em conjunto, mas pode utilizar o WebJobs sem o SDK do WebJobs e vice-versa. Um WebJob pode executar qualquer programa ou script que execute na sandbox do Serviço de Aplicações. Uma aplicação de consola do SDK do WebJobs pode ser executada em qualquer local onde as aplicações de consolas se executam, como nos servidores no local.

### <a name="comparison-table"></a>Tabela de comparação

As Funções do Azure são criadas no SDK do WebJobs, pelo que partilham muitos dos acionadores de eventos e das ligações a outros serviços do Azure. Eis alguns fatores a ter em conta ao escolher entre as Funções do Azure e o WebJobs com o SDK do WebJobs:

|  | Funções | WebJobs com o SDK do WebJobs |
| --- | --- | --- |
|[Modelo de aplicação sem servidor](https://azure.microsoft.com/overview/serverless-computing/) com [dimensionamento automático](functions-scale.md#how-the-consumption-plan-works)|✔||
|[Programação e testes no browser](functions-create-first-azure-function.md) |✔||
|[Preços com pagamento por utilização](functions-scale.md#consumption-plan)|✔||
|[Integração com Logic Apps](functions-twitter-email.md)|✔||
| Eventos de acionador |[Temporizador](functions-bindings-timer.md)<br>[Filas e blobs de Armazenamento do Microsoft Azure](functions-bindings-storage-blob.md)<br>[Filas e tópicos do Azure Service Bus](functions-bindings-service-bus.md)<br>[BD do Cosmos para o Azure](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Temporizador](functions-bindings-timer.md)<br>[Filas e blobs de Armazenamento do Microsoft Azure](functions-bindings-storage-blob.md)<br>[Filas e tópicos do Azure Service Bus](functions-bindings-service-bus.md)<br>[BD do Cosmos para o Azure](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Sistema de Ficheiros](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Linguagens suportadas  |C#<br>F#<br>JavaScript<br>Java (pré-visualização) |C#<sup>1</sup>|
|Gestores de pacotes|NPM e NuGet|NuGet<sup>2</sup>|

<sup>1</sup> O WebJobs (sem SDK do WebJobs) suporta C#, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python e mais. Não se trata de uma lista exaustiva; um WebJob pode executar qualquer programa ou script que possa ser executado na sandbox do Serviço de Aplicações.

<sup>2</sup> O WebJobs (sem SDK do WebJobs) suporta NPM e NuGet.

### <a name="summary"></a>Resumo

As Funções do Azure oferecem maior produtividade para programadores, mais opções de linguagem, mais opções de ambientes de desenvolvimento, mais opções de integração de serviços do Azure e mais opções de preços. Para a maioria dos cenários, é a melhor escolha.

Eis dois cenários para os quais o WebJobs pode ser a melhor escolha:

* Precisa de ter mais controlo sobre o código que escuta eventos, o objeto `JobHost`. As Funções oferecem um número limitado de formas de personalizar o comportamento `JobHost` no ficheiro [host.json](functions-host-json.md). Por vezes, precisa de fazer coisas que não podem ser especificadas por uma cadeia num ficheiro JSON. Por exemplo, apenas o SDK do WebJobs permite configurar uma política de repetição personalizada para o Armazenamento do Microsoft Azure.
* Tem uma aplicação do Serviço de Aplicações para a qual pretende executar fragmentos de código e geri-los em conjunto no mesmo ambiente DevOps.

Para outros cenários nos quais pretenda executar fragmentos de código para integrar serviços do Azure ou de terceiros, selecione as Funções do Azure sobre o WebJobs com o SDK do WebJobs.

<a name="together"></a>

## <a name="flow-logic-apps-functions-and-webjobs-together"></a>Flow, Logic Apps, Funções e WebJobs em conjunto

Não precisa de escolher apenas um destes serviços; os mesmos integram-se entre si, tal como fazem com serviços externos.

Um fluxo pode chamar uma aplicação lógica. Uma aplicação lógica pode chamar uma função e uma função pode chamar uma aplicação lógica. Consulte, por exemplo, [Criar uma função que se integra no Azure Logic Apps](functions-twitter-email.md).

A integração entre o Flow, o Logic Apps e as Funções continua a melhorar ao longo do tempo. Pode criar algo num serviço e utilizar noutros serviços.

## <a name="next-steps"></a>Passos seguintes

Comece por criar o seu primeiro fluxo, aplicação lógica ou aplicação de funções. Clique em qualquer uma das seguintes ligações:

* [Introdução ao Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Criar a sua primeira Função do Azure](functions-create-first-azure-function.md)

Em alternativa, obtenha mais informações sobre estes serviços de integração com as seguintes ligações:

* [Tirar partido das Funções do Azure e do Serviço de Aplicações do Azure para cenários de integração por Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrações Simplificadas por Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Webcast em Direto sobre Aplicações Lógicas](http://aka.ms/logicappslive)
* [Perguntas mais frequentes do Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)
