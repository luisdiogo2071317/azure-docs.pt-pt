---
title: "Escolher entre Flow, Logic Apps, Funções e WebJobs | Microsoft Docs"
description: "Comparar e diferenciar os serviços de integração da cloud da Microsoft, e decidir que serviços deve utilizar."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: "microsoft flow, flow, fluxo, logic apps, aplicações lógicas, funções do azure, funções, azure webjobs, webjobs, processamento de eventos, computação dinâmica, arquitetura sem servidor"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7ffe44828735a5687008ebc5a7d8d9f017f49daa
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Escolher entre Fluxo, Aplicações Lógicas, Funções e WebJobs
Este artigo compara e diferencia os seguintes serviços de cloud da Microsoft que podem resolver todos os problemas de integração e automatizar processos de negócio:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Funções do Azure](https://azure.microsoft.com/services/functions/)
* [WebJobs do Serviço de Aplicações do Azure ](../app-service/web-sites-create-web-jobs.md)

Todos estes serviços são úteis quando "combina" sistemas dispares. Podem todos definir entradas, ações, condições e saídas. Pode executar cada um com base numa agenda ou num acionador. No entanto, cada serviço tem vantagens exclusivas e para os comparar não baste perguntar "Qual é o melhor serviço?", mas antes perguntar "Que serviço melhor se adequa a esta situação?" Muitas vezes, uma combinação destes serviços é a melhor forma para criar rapidamente uma solução de integração dimensionável e completa.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flow vs. Aplicações Lógicas
Podemos analisar o Microsoft Flow e o Azure Logic Apps em conjunto por serem ambos serviços de integração baseados na *configuração*. Facilitam a criação de processos e fluxos de trabalho, e permitem a integração com várias aplicações SaaS e empresariais. 

* O Flow baseia-se no Logic Apps
* Têm o mesmo estruturador de fluxo de trabalho
* Os [conectores](../connectors/apis-list.md) de um funcionam no outro

O Flow capacita qualquer trabalhador de escritório para efetuar integrações simples (por exemplo, um processo de aprovação numa Biblioteca de Documentos do SharePoint) sem recorrer a programadores ou às TI. Por outro lado, o Logic Apps pode permitir integrações avançadas (por exemplo, processos B2B) que exigem práticas de DevOps e de segurança de nível empresarial. É normal um fluxo de trabalho do negócio se tornar cada vez mais complexo ao longo do tempo. Assim, pode começar primeiro com um fluxo e depois convertê-lo numa aplicação lógica, conforme necessário.

A tabela seguinte ajuda-o a determinar se o Flow ou o Logic Apps é o melhor para uma determinada integração.

|  | Fluxo | Aplicações Lógicas |
| --- | --- | --- |
| Audiência |Trabalhadores do escritório, utilizadores empresariais, administradores do SharePoint |Integradores e programadores profissionais, profissionais de TI |
| Cenários |Gestão personalizada |Integrações avançadas |
| Ferramenta de estruturação |Aplicação no browser e móvel, apenas IU |No browser e [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Vista de código](../logic-apps/logic-apps-author-definitions.md) disponível |
| Application Lifecycle Management (ALM) |Conceba e teste em ambientes de não produção e promova a produção quando estiver pronto. |DevOps: controlo da origem, teste, suporte, automatização e capacidade de gestão na [Gestão de Recursos do Azure](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Experiência de administrador |Gerir políticas de Prevenção de Perda de Dados (DLP) e Ambientes do Flow, controlar o licenciamento [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Gerir Grupos de Recursos, Ligações, Gestão de Acesso e Registos [https://portal.azure.com](https://portal.azure.com) |
| Segurança |Registos de auditoria de Segurança e Conformidade do Office 365, Prevenção de Perda de Dados (DLP), [encriptação inativa](https://wikipedia.org/wiki/Data_at_rest#Encryption) para dados confidenciais, etc. |Garantia de segurança do Azure: [Segurança do Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centro de Segurança](https://azure.microsoft.com/services/security-center/), [registos de auditoria](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) e muito mais. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Funções versus Trabalhos Web
Podemos analisar as Funções do Azure e o WebJobs do Serviço de Aplicações do Azure em conjunto por serem ambos serviços de integração baseados no *código* e se destinarem a programadores. Permitem executar um script ou um fragmento de código em resposta a vários eventos, tais como [novos Blobs de Armazenamento](functions-bindings-storage.md) ou [um Pedido de WebHook](functions-bindings-http-webhook.md). Seguem-se as suas semelhanças: 

* Ambos baseiam-se no [Serviço de Aplicações do Azure](../app-service/app-service-web-overview.md) e dispõem de funcionalidades como [controlo da origem](../app-service/app-service-continuous-deployment.md), [autenticação](../app-service/app-service-authentication-overview.md) e [monitorização](../app-service/web-sites-monitor.md).
* São ambos serviços destinados a programadores.
* Ambos suportam linguagens padrão de script e programação.
* Ambos têm suporte para NuGet e NPM.

As Funções são a evolução natural dos WebJobs na medida em que melhoram as melhores características dos WebJobs. Os melhoramentos incluem: 

* Modelo de aplicação [sem servidor](https://azure.microsoft.com/overview/serverless-computing/).
* Desenvolvimento, teste e execução simplificados do código, diretamente no browser.
* Integração incorporada com mais serviços do Azure e serviços de terceiros como [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
* Pagamento por utilização, não é necessário pagar um [plano de Serviço de Aplicações](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* [Dimensionamento dinâmico](functions-scale.md) automático.
* Para clientes existentes do Serviço de Aplicações, utilizar um plano de Serviço de Aplicações pode continuar a ser possível (para tirar partido dos recursos subutilizados).
* Integração com Logic Apps.

A tabela seguinte resume as diferenças entre Funções e WebJobs:

|  | Funções | Trabalhos Web |
| --- | --- | --- |
| Dimensionamento |Dimensionamento sem configuração |Dimensionamento com plano de Serviço de Aplicações |
| Preços |Pagamento por utilização ou parte do plano de Serviço de Aplicações |Parte do plano de Serviço de Aplicações |
| Tipo de execução |Acionada, agendada (por acionador de temporizador) |Acionada, contínua, agendada |
| Eventos de acionador |[Temporizador](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-cosmosdb.md), [Hubs de Eventos do Azure](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Aplicações Móveis do Serviço de Aplicações do Azure](functions-bindings-mobile-apps.md), [Hubs de Eventos do Azure](functions-bindings-event-hubs.md), [Filas e blobs de Armazenamento do Azure](functions-bindings-storage-blob.md), [Filas e tópicos do Azure Service Bus](functions-bindings-service-bus.md) |[Filas e blobs de armazenamento do Azure](functions-bindings-storage-blob.md), [Filas e tópicos do Azure Service Bus](functions-bindings-service-bus.md) |
| Desenvolvimento no browser |Suportado |Não suportado |
| C# |Suportado |Suportado |
| F# |Suportado |Não suportado |
| JavaScript |Suportado |Suportado |
| Java |Pré-visualização | Não suportado |
| Bash |Experimental |Suportado |
| Windows Scripting (. cmd,. bat) |Experimental |Suportado |
| PowerShell |Experimental |Suportado |
| PHP |Experimental |Suportado |
| Python |Experimental |Suportado |
| TypeScript |Experimental |Não suportado |

Em última análise, utilizar as Funções ou os WebJobs depende da forma como já utiliza o Serviço de Aplicações. Se tiver uma aplicação do Serviço de Aplicações para o qual pretende executar fragmentos de código e geri-los em conjunto no mesmo ambiente DevOps, utilize os WebJobs. Nos seguintes cenários, utilize as Funções.

* Pretende executar fragmentos de código para outros serviços do Azure ou aplicações de terceiros.
* Pretende gerir o seu código de integração em separado das suas aplicações de Serviço de Aplicações.
* Pretende chamar os fragmentos de código a partir de uma Aplicação lógica. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Flow, Logic Apps e Funções em conjunto
Como mencionado anteriormente, o serviço mais adequado depende da situação existente. 

* Para uma otimização empresarial simples, utilize o Flow.
* Se o seu cenário de integração for demasiado avançado para o Flow ou precisar de capacidades DevOps, utilize o Logic Apps.
* Se uma fase do seu cenário de integração exigir uma transformação altamente personalizada ou um código especializado, escreva uma função e acione-a como uma ação na sua aplicação lógica.

Pode chamar uma aplicação lógica num fluxo. Também pode chamar uma função numa aplicação lógica e uma aplicação lógica numa função. A integração entre o Flow, o Logic Apps e as Funções continua a melhorar ao longo do tempo. Pode criar algo num serviço e utilizar noutros serviços. Assim, qualquer investimento nestas três tecnologias será sempre uma mais valia.

## <a name="next-steps"></a>Passos seguintes
Comece com cada um dos serviços ao criar o seu primeiro fluxo, aplicação lógica, aplicação de função ou WebJob. Clique em qualquer uma das seguintes ligações:

* [Introdução ao Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Criar a sua primeira Função do Azure](functions-create-first-azure-function.md)
* [Implementar o WebJobs com o Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Em alternativa, obtenha mais informações sobre estes serviços de integração com as seguintes ligações:

* [Tirar partido das Funções do Azure e do Serviço de Aplicações do Azure para cenários de integração por Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrações Simplificadas por Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Webcast em Direto sobre Aplicações Lógicas](http://aka.ms/logicappslive)
* [Perguntas mais frequentes do Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)

