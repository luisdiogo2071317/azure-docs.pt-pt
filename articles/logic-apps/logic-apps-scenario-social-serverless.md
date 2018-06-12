---
title: Cenário sem servidor – criar um dashboard de conhecimentos de cliente com o Azure | Microsoft Docs
description: Saiba como pode gerir comentários de clientes, dados de redes sociais e muito mais ao criar um dashboard de cliente com Azure Logic Apps e as funções do Azure
keywords: ''
services: logic-apps
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jehollan; LADocs
ms.openlocfilehash: 3ee3ec3107cf8aad834e8201405c9aa833d838af
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299965"
---
# <a name="create-a-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Criar um dashboard de conhecimentos de cliente transmissão em fluxo com Azure Logic Apps e as funções do Azure

O Azure disponibiliza sem servidor ferramentas que ajudam a rapidamente compilação e o anfitrião de aplicações na nuvem, sem ter de refletir sobre a infraestrutura. Neste tutorial, pode criar um dashboard que aciona nos comentários de clientes, analisa os comentários com machine learning e publica insights a uma origem, tais como o Power BI ou do Azure Data Lake.

Para esta solução, utilize estes componentes chave do Azure para aplicações sem servidor: [das funções do Azure](https://azure.microsoft.com/services/functions/) e [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Aplicações lógicas do Azure fornece um motor de fluxo de trabalho sem servidor na nuvem, para que possa criar orchestrations sem servidor componentes e ligar ao 200 + serviços e APIs. As funções do Azure fornece sem servidor computação na nuvem. Esta solução utiliza as funções do Azure para sinalizar tweets de cliente com base em palavras-chave predefinidas.

Neste cenário, crie uma aplicação lógica que aciona em localizar comentários de clientes. Alguns conectores que ajuda a responder a comentários de clientes inclui Outlook.com, Office 365, Monkey de inquérito, Twitter e um [pedido de HTTP a partir de um formulário web](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/). O fluxo de trabalho que criar monitoriza um hashtag no Twitter.

Pode [compilar a solução completa no Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) e [implementar a solução com o modelo Azure Resource Manager](../logic-apps/logic-apps-create-deploy-template.md). Para instruções vídeo que mostra como criar esta solução, [veja este vídeo de Channel 9](http://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Acionar nos dados de cliente

1. No portal do Azure ou Visual Studio, crie uma aplicação lógica em branco. 

   Se estiver familiarizado com as logic apps, reveja o [início rápido para o portal do Azure](../logic-apps/quickstart-create-first-logic-app-workflow.md) ou [início rápido para o Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

2. No Designer de aplicação lógica, localizar e adicione o acionador do Twitter com esta ação: **quando é registado um tweet novo**

3. Defina o acionador para escutar os tweets com base em palavra-chave ou hashtag.

   No acionadores baseada em consulta, tais como o acionador do Twitter, a propriedade de periodicidade determina a frequência a aplicação lógica verifica a existência de novos itens.

   ![Exemplo de Acionador do Twitter][1]

Esta aplicação lógica agora desencadeado em todos os novos tweets. Em seguida, pode demorar e analisar os dados de tweet para que pode compreender melhor os sentiments expressados. 

## <a name="analyze-tweet-text"></a>Analisar texto tweet

Para detetar o sentimento atrás algum texto, pode utilizar [serviços cognitivos Azure](https://azure.microsoft.com/services/cognitive-services/).

1. No Designer de aplicação lógica, sob o acionador, escolha **novo passo**.

2. Localizar o **análise de texto** conector.

3. Selecione o **detetar sentimento** ação.

4. Se lhe for solicitado, forneça uma chave de serviços cognitivos válida para o serviço de análise de texto.

5. Em **corpo do pedido**, selecione o **Tweet texto** campo, o que fornece o texto de tweet como entrada para análise.

Depois de obter os dados de tweet e informações sobre o tweet, agora, pode utilizar vários outros conectores relevantes e as respetivas ações:

* **Power BI - adicionar linhas ao conjunto de dados de transmissão em fluxo**: ver tweets recebidos no dashboard do Power BI.
* **Azure Data Lake - acrescentar o ficheiro**: adicionar dados de cliente para um conjunto de dados do Azure Data Lake para incluir nas tarefas de análise.
* **SQL Server - adicionar linhas**: armazenar dados numa base de dados para obtenção posterior.
* **Slack - enviar mensagem**: notificar um canal Slack sobre os comentários negativos que possam exigir ação.

Também pode criar e do Azure funcionar para que possam executar processamento personalizado nos seus dados. 

## <a name="process-data-with-azure-functions"></a>Processamento de dados com as funções do Azure

Antes de criar uma função, crie uma aplicação de função na sua subscrição do Azure. Além disso, para a sua aplicação lógica chamar diretamente uma função, a função tem de ter um HTTP acionar enlace, por exemplo, utilize o **HttpTrigger** modelo. Saiba [como criar a sua primeira aplicação de função e a função no portal do Azure](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Para este cenário, utilize o texto de tweet como o corpo do pedido para a função do Azure. No código da função, defina a lógica que determina se o texto de tweet contém uma palavra-chave ou uma expressão. Manter a função de simples ou complexas, conforme necessário para o cenário.
No final da função, devolver uma resposta para a aplicação lógica com alguns dados, por exemplo, um simples booleano como valor `containsKeyword` ou um objeto complexo.

> [!TIP]
> Para aceder a uma resposta complexa de uma função de uma aplicação lógica, utilize o **analisar JSON** ação.

Quando tiver terminado, guarde a função e, em seguida, adicione a função como uma ação na aplicação lógica que está a criar.

## <a name="add-azure-function-to-logic-app"></a>Adicionar a função do Azure para a aplicação lógica

1. No Designer de aplicação lógica, sob o **detetar sentimento** ação, escolha **novo passo**.

2. Localizar o **das funções do Azure** conector e, em seguida, selecione a função que criou.

3. Em **corpo do pedido**, selecione **Tweet texto**.

![Passo de função do Azure configurado][2]

## <a name="run-and-monitor-your-logic-app"></a>Executar e monitorizar a sua aplicação lógica

Para rever as execuções atuais ou anteriores para a sua aplicação lógica, pode utilizar a avançada de depuração e capacidades do Azure Logic Apps fornece no portal do Azure, o Visual Studio, ou pelas APIs REST do Azure e dos SDKs de monitorização.

Para testar facilmente a sua aplicação lógica, no Designer de aplicação lógica, escolha **executar acionador**. O acionador consulta tweets com base na sua agenda especificada até é encontrado um tweet que cumpra os critérios. Durante a execução avança de ser, no designer mostra uma vista em direto para essa execução.

A vista anterior execute histories no Visual Studio ou no portal do Azure: 

* Abra o Explorador de nuvem do Visual Studio. Localizar a sua aplicação lógica, abra o menu de atalho da aplicação. Selecione **histórico de execução Open**.

* No portal do Azure, localize a sua aplicação lógica. No menu da sua aplicação lógica, escolha **descrição geral**. 

## <a name="create-automated-deployment-templates"></a>Criar modelos de implementação automatizada

Depois de criar uma solução de aplicação lógica, pode capturar e implementar a aplicação como um [modelo Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#template-deployment) a qualquer região do Azure no mundo. Pode utilizar esta capacidade tanto para modificar os parâmetros para a criação de versões diferentes da sua aplicação e para integrar a solução em compilação e versão pipeline. Também pode incluir funções do Azure no seu modelo de implementação, para que possa gerir a solução completa com todas as dependências, como um único modelo. Saiba [como criar modelos de implementação de aplicação de lógica](../logic-apps/logic-apps-create-deploy-template.md).

Para um modelo de implementação de exemplo com uma função do Azure, verifique o [repositório de modelo de início rápido do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Passos Seguintes

* [Localizar outros exemplos e cenários para o Azure Logic Apps](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png