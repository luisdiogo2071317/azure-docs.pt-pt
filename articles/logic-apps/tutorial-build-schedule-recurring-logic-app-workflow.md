---
title: Criar baseado no programador de fluxos de trabalho automatizados - Azure Logic Apps | Microsoft Docs
description: Este tutorial mostra como criar um fluxo de trabalho com base no programador recorrente, automatizado com Azure Logic Apps
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: deb2572de363ca5d0dec0f78f2e30ad648e9b5f8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="check-traffic-with-a-scheduler-based-logic-app"></a>Tráfego de verificação com uma aplicação lógica baseada no programador

Aplicações lógicas do Azure ajuda a automatizar fluxos de trabalho com base numa agenda. Este tutorial mostra como pode criar um [aplicação lógica](../logic-apps/logic-apps-overview.md) com um acionador de programador, que é executada todas as manhãs dia da semana e verifica o levar tempo, incluindo o tráfego, entre duas coloca. Se o tempo exceder um limite específico, a aplicação lógica envia correio eletrónico com a hora de levar e o tempo adicional necessário para o destino.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma aplicação lógica em branco. 
> * Adicione um acionador que funciona como um programador para a sua aplicação lógica.
> * Adicione uma ação que obtém o tempo de levar para uma rota.
> * Adicione uma ação que cria uma variável, converte o levar tempo de segundos para minutos e guarda que resultam na variável.
> * Adicione uma condição que compara a hora de levar contra um limite especificado.
> * Adicione uma ação que envia a mensagem de e-mail, se o tempo de levar excede o limite.

Quando tiver terminado, a aplicação lógica aspeto este fluxo de trabalho a um alto nível:

![Aplicação de lógica de alto nível](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se numa conta do Azure gratuita</a> antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta de e-mail de um fornecedor de correio eletrónico suportado pelo Logic Apps, tais como o Outlook do Office 365, Outlook.com ou Gmail. Para outros fornecedores, [rever os conectores listam aqui](https://docs.microsoft.com/connectors/). Este guia de introdução utiliza uma conta do Outlook.com. Se utilizar uma conta de correio eletrónico diferente, os passos gerais permanecem o mesmo, mas a sua IU pode aparecer ligeiramente diferente.

* Para obter a hora de levar para uma rota, terá uma chave de acesso a API para Bing Maps. Para obter esta chave, siga os passos para <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">como obter uma chave do Bing Maps</a>. 

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Iniciar sessão para o <a href="https://portal.azure.com" target="_blank">portal do Azure</a> com as suas credenciais de conta do Azure.

## <a name="create-your-logic-app"></a>Criar a sua aplicação lógica

1. No menu principal do Azure, escolha **Nova** > **Integração Empresarial** > **Aplicação Lógica**.

   ![Criar uma aplicação lógica](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. Em **Criar aplicação de lógica**, forneça estas informações sobre a sua aplicação lógica, como mostrado e descrito. Quando tiver terminado, escolha **Afixar ao dashboard** > **Criar**.

   ![Fornecer informações da aplicação lógica](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome** | LA-TravelTime | O nome para a sua aplicação lógica | 
   | **Subscrição** | <*your-Azure-subscription-name*> | O nome da sua subscrição do Azure | 
   | **Grupo de recursos** | TravelTime-LA-RG | O nome para o [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) utilizados para organizar os recursos relacionados | 
   | **Localização** | EUA Leste 2 | A região onde pretende armazenar informações sobre a sua aplicação lógica | 
   | **Log Analytics** | Desativado | Manter o **desativar** a definição de registo de diagnóstico. | 
   |||| 

3. Depois de Azure implementa a aplicação, o Designer de aplicações lógica abre e apresenta uma página com uma introdução em vídeo e modelos para padrões comuns da aplicação lógica. Em **Modelos**, escolha **Aplicação Lógica em Branco**.

   ![Escolher o modelo de aplicação lógica em branco](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Em seguida, adicione a periodicidade [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado com base numa agenda especificada. Cada aplicação lógica tem de começar com um acionador, o que acontece é desencadeado quando um evento específico ou quando novos dados cumprem uma condição específica. Para obter mais informações, consulte [criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Adicionar o acionador de programador

1. No estruturador, introduza "recurrence" na caixa de pesquisa. Selecione este acionador: **agenda - periodicidade**

   ![Localizar e adicione o acionador "Recurrence agenda"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. No **periodicidade** formam, escolha o **reticências** (**...** ) botão e escolha **mudar o nome**. Mudar o nome do acionador com esta descrição:```Check travel time every weekday morning```

   ![Mudar o nome do acionador](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Dentro de acionamento, escolha **Mostrar opções avançadas**.

4. Forneça os detalhes de agenda e periodicidade para o acionador como mostrado e descritas:

   ![Forneça detalhes de agenda e periodicidade](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Interval** | 1 | O número de intervalos de espera entre as verificações | 
   | **Frequência** | Semana | A unidade de tempo a utilizar para a periodicidade | 
   | **Fuso horário** | Nenhum | Aplica-se apenas quando especifica uma hora de início. É útil para especificar um não-fuso horário local. | 
   | **Hora de início** | Nenhuma | Atrase a periodicidade até uma data e hora específicas. Para obter mais informações, consulte [agendar tarefas e fluxos de trabalho que execute regularmente](../connectors/connectors-native-recurrence.md). | 
   | **Nestes dias** | Segunda, Terça, quarta-feira, Quinta-feira, sexta-feira | Disponível apenas quando **frequência** está definido como "Semana" | 
   | **Nestas horas** | 7,8,9 | Disponível apenas quando **frequência** está definida como "Semana" ou "Dia". Selecione as horas do dia para executar esta recorrência tem. Neste exemplo é executada à 7, 8 e marcas de escala 9 horas. | 
   | **A estes minutos** | 0,15,30,45 | Disponível apenas quando **frequência** está definida como "Semana" ou "Dia". Selecione os minutos do dia para executar esta recorrência tem. Neste exemplo é executado a cada 15 minutos, começando na marca de hora de zero. | 
   ||||

   Este acionador desencadeado cada dia da semana, a cada 15 minutos, começando 7 horas da Manhã e termina às 9:45. 
   O **pré-visualização** caixa mostra a agenda de periodicidade. 
   Para obter mais informações, consulte [agendar tarefas e fluxos de trabalho](../connectors/connectors-native-recurrence.md) e [ações de fluxo de trabalho e é acionado](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Para ocultar os detalhes do accionador por agora, clique no interior da barra de título da forma.

   ![Fechar de forma para ocultar os detalhes](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Guarde a sua aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**. 

A aplicação lógica está agora em direto, mas não faz nada de outros repetir. Por isso, adicione uma ação que responde quando o acionador é acionado.

## <a name="get-the-travel-time-for-a-route"></a>Obter a hora de levar para uma rota

Agora que tem um acionador, adicione um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) que obtém o levar tempo entre dois locais. As Logic Apps fornece um conector de API para Bing Maps para que pode facilmente obter estas informações. Antes de começar esta tarefa, certifique-se de que tem uma chave de API do Bing Maps, conforme descrito em pré-requisitos do tutorial.

1. No Designer de aplicação lógica, sob o acionador, escolha **+ novo passo** > **adicionar uma ação**.

2. Procure "maps" e selecionar esta ação: **Bing Maps - Get rota**

3. Se não tiver uma ligação do Bing Maps, está a pedido para criar uma ligação. Forneça estes detalhes de ligação e escolha **criar**.

   ![Selecione "Bing Maps - Get rota" ação](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Definição | Valor | Descrição |
   | ------- | ----- | ----------- |
   | **Nome da Ligação** | BingMapsConnection | Forneça um nome para a ligação. | 
   | **Chave de API** | <*your-Bing-Maps-key*> | Introduza a chave de mapas Bing anteriormente recebido. Se não tiver uma chave do Bing Maps, saiba <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">como obter uma chave</a>. | 
   | | | |  

4. Mudar o nome de ação com esta descrição:```Get route and travel time with traffic```

5. Forneça detalhes para a **Get rota** ação conforme mostrado e descrito aqui, por exemplo:

   ![Fornecer informações de "Bing Maps - Get rota" ação](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Definição | Valor | Descrição |
   | ------- | ----- | ----------- |
   | **Waypoint 1** | <*start-location*> | Origem da rota | 
   | **Waypoint 2** | <*end-location*> | Destino da rota | 
   | **Evitar** | Nenhum | Quaisquer itens para evitar no seu rota, como highways, tolls e assim sucessivamente | 
   | **Otimizar** | timeWithTraffic | Um parâmetro para otimizar a rota, por exemplo, a distância, viajam tempo com tráfego atual e assim sucessivamente. Selecione este parâmetro: "timeWithTraffic" | 
   | **Unidade de distância** | <*your-preference*> | A unidade de distância da rota. Este artigo utiliza esta unidade: "Mile"  | 
   | **Modo de levar** | Impulsionar | O modo de levar para a rota. Selecione este modo: "A ocasionar" | 
   | **Trânsito data / hora** | Nenhum | Aplica-se ao modo de trânsito apenas | 
   | **Tipo de tipo de data de trânsito** | Nenhuma | Aplica-se ao modo de trânsito apenas | 
   |||| 

   Para obter mais informações sobre estes parâmetros, consulte [calcular uma rota](https://msdn.microsoft.com/library/ff701717.aspx).

6. Guarde a sua aplicação lógica.

Em seguida, crie uma variável para que possam converter e armazene-a hora atual do levar minutos, em vez de segundos. Dessa forma, pode evitar a repetição de conversão e utilize o valor mais facilmente em passos posteriores. 

## <a name="create-variable-to-store-travel-time"></a>Crie a variável para armazenar levar tempo

Por vezes, pode querer executam operações nos dados do seu fluxo de trabalho e utilize os resultados das ações posteriores. Para guardar estes resultados, para que possa facilmente reutilizar ou referenciá-los, pode criar variáveis para armazenar esses resultados após o processamento-los. Pode criar variáveis apenas no nível superior na sua aplicação lógica.

Por predefinição, anterior **Get rota** ação devolve a hora atual do levar com tráfego em segundos, através de **viajam tráfego de duração** campo. Ao converter e armazenar em vez disso, este valor como minutos, efetua o valor mais fácil reutilizar mais tarde sem conversão novamente.

1. Sob o **Get rota** ação, escolha **+ novo passo** > **adicionar uma ação**.

2. Procure "variáveis" e selecionar esta ação: **variáveis - inicializar variável**

   ![Selecione "Variáveis - inicializar variável" ação](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Esta ação com esta descrição de mudar o nome:```Create variable to store travel time```

4. Forneça os detalhes para a variável, conforme descrito aqui:

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome** | travelTime | O nome para a variável | 
   | **Tipo** | Número inteiro | O tipo de dados para a variável | 
   | **Valor** | Uma expressão que converte a hora atual do levar de segundos para minutos (ver os passos nesta tabela). | O valor inicial para a variável | 
   |||| 

   1. Para criar a expressão para o **valor** campo, clique dentro do campo de modo a que é apresentada a lista de conteúdo dinâmica. 
   Se necessário, alargar o seu browser até que a lista aparece. 
   Na lista de conteúdo dinâmica, escolha **expressão**. 

      ![Fornecer informações para "Variáveis - inicializar variável" ação](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Ao clicar no interior de algumas caixas de edição, aparece uma lista de conteúdo dinâmica ou uma lista de parâmetros inline. Esta lista mostra todos os parâmetros do anteriores ações que pode utilizar como entradas no fluxo de trabalho. 
      A lista de conteúdo dinâmica tem um editor de expressão onde pode selecionar funções para efetuar operações. 
      O editor de expressão é apresentado apenas na lista de conteúdo dinâmica.

      A largura do seu browser determina que lista aparece. 
      Se o seu browser for grande, é apresentada a lista de conteúdo dinâmica. 
      Se o seu browser estreito, aparece uma lista de parâmetro que inline sob a caixa de edição tem actualmente o foco.

   2. No editor de expressão, introduza esta expressão:```div(,60)```

      ![Introduza esta expressão: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. Coloque o cursor no interior da expressão entre o parêntesis esquerdo (**(**) e a vírgula (**,**). 
   Escolha **conteúdo dinâmico**.

      ![Posicione o cursor, escolha "Conteúdo dinâmico"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Na lista de conteúdo dinâmica, selecione **levar duração tráfego**.

      ![Selecione o campo "Tráfego de duração levar"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. Depois do campo é resolvido dentro da expressão, escolha **OK**.

      ![Escolha "OK"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      O **valor** campo aparece conforme mostrado aqui:

      ![Campo de "Valor" com a expressão resolvido](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Guarde a sua aplicação lógica.

Em seguida, adicione uma condição que verifica se a hora atual do levar é superior a um limite específico.

## <a name="compare-travel-time-with-limit"></a>Comparar levar tempo com limite

1. Sob a ação anterior, escolha **+ novo passo** > **adicionar uma condição**. 

2. Mudar o nome da condição com esta descrição:```If travel time exceeds limit```

3. Criar uma condição que verifica se **travelTime** excede o limite especificado como descrito e apresentadas aqui:

   1. Dentro de condição, clique dentro da **escolha um valor** caixa, que está à esquerda (vista largas) ou na parte superior (vista estreitas).

   2. Na lista de conteúdo dinâmica ou a lista de parâmetros, selecione o **travelTime** campo em **variáveis**.

   3. Na caixa de comparação, selecione este operador: **é maior do que**

   4. No **escolha um valor** caixa no lado direito (wide vista) ou inferior (vista estreito), introduza este limite:```15```

   Por exemplo, se estiver a trabalhar na vista estreito, eis como criar esta condição:

   ![Criar a condição na vista estreito](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Guarde a sua aplicação lógica.

Em seguida, adicione a ação a executar quando o tempo de levar excede o limite.

## <a name="send-email-when-limit-exceeded"></a>Enviar correio eletrónico quando o limite excedido

Agora, adicione uma ação que e-mails quando o tempo de levar excede o limite. Este e-mail inclui a hora de levar atual e o tempo adicional necessário viajam a rota especificada. 

1. A condição **se for verdadeiro** sucursal, escolha **adicionar uma ação**.

2. Procure "enviar e-mail" e selecione o conector de correio eletrónico e a "a ação de e-mail de envio" que pretende utilizar.

   ![Localize e selecione a ação "enviar correio eletrónico"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Para contas Microsoft pessoais, selecione **Outlook.com**. 
   * Para o Azure de trabalho ou escola contas, selecione **Outlook do Office 365**.

3. Se ainda não tiver uma ligação, está a pedido para iniciar sessão sua conta de e-mail.

   O Logic Apps cria uma ligação para a sua conta de e-mail.

4. Mudar o nome de ação com esta descrição:```Send email with travel time```

5. Na caixa **Para**, introduza o endereço de e-mail do destinatário. Para fins de teste, utilize o seu endereço de e-mail.

6. No **requerente** caixa, especifique o assunto do e-mail e incluir o **travelTime** variável.

   1. Introduza o texto ```Current travel time (minutes): ``` com um espaço à direita. 
   
   2. Na lista de parâmetros ou a lista de conteúdo dinâmica, selecione **travelTime** em **variáveis**. 
   
      Por exemplo, se o seu browser está na vista estreito:

      ![Introduza o texto do requerente e expressão que devolve a hora de levar](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. No **corpo** caixa, especifique o conteúdo do corpo da mensagem. 

   1. Introduza o texto ```Add extra travel time (minutes): ``` com um espaço à direita. 
   
   2. Se necessário, alargar o seu browser até que é apresentada a lista de conteúdo dinâmica. 
   Na lista de conteúdo dinâmica, escolha **expressão**.

      ![Criar expressão de corpo de e-mail](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. No editor de expressão, introduza esta expressão para que pode calcular o número de minutos que excedem o limite:```sub(,15)```

      ![Introduza a expressão para calcular minutos adicionais viajam tempo](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. Coloque o cursor no interior da expressão entre o parêntesis esquerdo (**(**) e a vírgula (**,**). Escolha **conteúdo dinâmico**.

      ![Continuar a criação de expressão para calcular minutos adicionais viajam tempo](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. Em **variáveis**, selecione **travelTime**.

      ![Selecione o campo "travelTime" para utilizar na expressão](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. Depois do campo é resolvido dentro da expressão, escolha **OK**.

      ![Campo de "Corpo" com a expressão resolvido](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      O **corpo** campo aparece conforme mostrado aqui:

      ![Campo de "Corpo" com a expressão resolvido](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Guarde a sua aplicação lógica.

Em seguida, teste a sua aplicação lógica, que agora semelhante para este exemplo:

![Aplicação lógica concluído](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Execute a sua aplicação lógica

Para iniciar manualmente a sua aplicação lógica, na barra de ferramentas do estruturador, escolha **Executar**. Se atual viajam tempo permanece no seu limite, a sua aplicação lógica faz mais nada e aguarda que o próximo intervalo antes de a verificar novamente.
Mas se a hora de levar atual excede o limite, receberá um e-mail com a hora de levar atual e o número de minutos acima do limite. Eis um e-mail de exemplo que envia a sua aplicação lógica:

![E-mail enviado com levar tempo](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

Se não obtiver quaisquer mensagens de correio eletrónico, verifique a pasta lixo do seu e-mail. O filtro de lixo de correio eletrónico pode redirecionar estes tipos de mensagens de correio eletrónico. Caso contrário, se não tiver a certeza de que a aplicação lógica foi executada corretamente, veja [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Resolver problemas da sua aplicação lógica).

Parabéns, ter agora criado e executar uma aplicação lógica baseadas na agenda periódica. 

Para outras aplicações lógicas que utilizam o **agenda - periodicidade** acionar, veja estes modelos, que disponível depois de criar uma aplicação lógica:

* Obter lembretes diárias enviados.
* Elimine blobs do Azure mais antigos.
* Adicione uma mensagem para uma fila de armazenamento do Azure.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos que contém a sua aplicação lógica e os recursos relacionados. No menu principal do Azure, aceda a **grupos de recursos**e selecione o grupo de recursos para a sua aplicação lógica. Escolha **eliminar grupo de recursos**. Introduza o nome do grupo de recursos como confirmação e escolha **eliminar**.

!["Descrição geral" > "Eliminar grupo de recursos"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma aplicação lógica que verifica o tráfego com base numa agenda especificada (no mornings dia da semana) e ação demora (envia correio eletrónico) quando o levar tempo excede um limite especificado. Agora, saiba como compilar uma aplicação lógica que envia pedidos de lista de mailing aprovação através da integração dos serviços do Azure, os serviços Microsoft e outras aplicações de SaaS.

> [!div class="nextstepaction"]
> [Gerir pedidos de lista de mailing](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)