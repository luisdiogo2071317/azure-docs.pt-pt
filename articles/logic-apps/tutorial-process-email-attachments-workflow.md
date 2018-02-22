---
title: Criar fluxos de trabalho para processar e-mails e anexos - Azure Logic Apps | Microsoft Docs
description: "Este tutorial mostra como criar fluxos de trabalho automatizados para processar e-mails e anexos com o Azure Logic Apps, o Armazenamento do Azure e as Funções do Azure"
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
ms.openlocfilehash: 16db5bc948b1eac0ef996b449d89211fb210a91e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>Processar e-mails e anexos com uma aplicação lógica

O Azure Logic Apps ajuda-o a automatizar fluxos de trabalho e a integrar dados nos serviços do Azure, nos serviços Microsoft, noutras aplicações de software como serviço (Saas) e em sistemas no local. Este tutorial mostra como pode criar uma [aplicação lógica](../logic-apps/logic-apps-overview.md) que processa e-mails recebidos e anexos. Esta aplicação lógica processa esses conteúdos, guarda-os no armazenamento do Azure e envia notificações para revê-los. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o [Armazenamento do Azure](../storage/common/storage-introduction.md) e o Explorador de Armazenamento para verificar e-mails e anexos guardados.
> * Criar uma [função do Azure](../azure-functions/functions-overview.md) que remove o HTML dos e-mails. Este tutorial inclui o código que pode utilizar para esta função.
> * Criar uma aplicação lógica em branco.
> * Adicionar um acionador que monitoriza os e-mails para verificar se há anexos.
> * Adicionar uma condição que verifica se os e-mails têm anexos.
> * Adicionar uma ação que chama a função do Azure quando um e-mail tem anexos.
> * Adicionar uma ação que cria blobs de armazenamento para e-mails e anexos.
> * Adicionar uma ação que envia notificações de e-mail.

Quando terminar, a aplicação lógica é semelhante a este fluxo de trabalho a alto nível:

![Aplicação lógica concluída de alto nível](./media/tutorial-process-email-attachments-workflow/overview.png)

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se numa conta do Azure gratuita</a> antes de começar. 

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta de e-mail de um fornecedor de e-mail que o Logic Apps suporte, como o Outlook do Office 365, o Outlook.com ou o Gmail. Para outros fornecedores, [consulte a lista de conectores aqui](https://docs.microsoft.com/connectors/).

  Esta aplicação lógica utiliza uma conta do Outlook do Office 365. 
  Se utilizar outra conta de e-mail, os passos gerais são os mesmos, mas a IU poderá ser ligeiramente diferente.

* Transferir e instalar o <a href="http://storageexplorer.com/" target="_blank">Explorador de Armazenamento do Microsoft Azure</a> gratuito. Esta ferramenta ajuda-o a verificar se o seu contentor de armazenamento está configurado corretamente.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no <a href="https://portal.azure.com" target="_blank">portal do Azure</a> com as credenciais da sua conta do Azure.

## <a name="set-up-storage-to-save-attachments"></a>Configurar o armazenamento para guardar anexos

Pode guardar os e-mails e anexos recebidos como blobs num [contentor de armazenamento do Azure](../storage/common/storage-introduction.md). 

1. Antes de criar um contentor de armazenamento, [crie uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) com estas definições:

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome** | attachmentstorageacct | O nome da sua conta de armazenamento. | 
   | **Deployment model** (Modelo de implementação) | Resource Manager | O [modelo de implementação](../azure-resource-manager/resource-manager-deployment-model.md) para gerir a implementação de recursos | 
   | **Account kind** (Tipo de conta) | Fins gerais | O [tipo de conta de armazenamento](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Performance** (Desempenho) | Standard | Esta definição especifica os tipos de dados suportados e os suportes de dados para armazenar dados. Veja [Tipos de contas de armazenamento](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replicação** | Armazenamento localmente redundante (LRS) | Esta definição especifica de que forma os seus dados são copiados, armazenados, geridos e sincronizados. Veja [Replicação](../storage/common/storage-introduction.md#replication). | 
   | **Secure transfer required** (Transferência segura necessária) | Desativado | Esta definição especifica a segurança necessária para pedidos provenientes de ligações. Veja [Require secure transfer](../storage/common/storage-require-secure-transfer.md) (Transferência segura necessária). | 
   | **Subscrição** | <*your-Azure-subscription-name*> | O nome para a subscrição do Azure | 
   | **Grupo de recursos** | LA-Tutorial-RG | O nome do [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) utilizado para organizar e gerir os recursos relacionados. <p>**Nota:** os grupos de recursos existem dentro de regiões específicas. Embora os itens neste tutorial possam não estar disponíveis em todas as regiões, tente utilizar a mesma região sempre que possível. | 
   | **Localização** | EUA Leste 2 | A região onde pretende armazenar as informações da sua conta de armazenamento | 
   | **Configurar redes virtuais** | Desativado | Neste tutorial, mantenha a definição **Desativado**. | 
   |||| 

   Também pode utilizar o [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) ou a [CLI do Azure](../storage/common/storage-quickstart-create-storage-account-cli.md).
  
2. Após o Azure implementar a sua conta de armazenamento, obtenha a chave de acesso da mesma:

   1. No menu da conta de armazenamento, em **Definições**, escolha **Chaves de acesso**. 
   2. Localize **key1** em **Chaves predefinidas** e o nome da sua conta.

      ![Copie e cole o nome e a chave da conta de armazenamento](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Também pode utilizar o [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) ou a [CLI do Azure](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list). 

3. Crie um contentor de armazenamento para os anexos de e-mails.
   
   1. No menu da conta do armazenamento, no painel **Descrição Geral**, escolha **Blobs**, em **Serviços**, e escolha **+ Contentor**.

   2. Introduza "attachments" como o nome do contentor. Em **Nível de acesso público**, selecione **Contentor (acesso de leitura anónimo para contentores e blobs)**e escolha **OK**.

   Também pode utilizar o [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) ou a [CLI do Azure](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create). 
   Quando terminar, pode encontrar o contentor de armazenamento na sua conta de armazenamento aqui no portal do Azure:

   ![Contentor de armazenamento terminado](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Depois, ligue o Explorador de Armazenamento à conta de armazenamento.

## <a name="set-up-storage-explorer"></a>Configurar o Explorador de Armazenamento

Agora, ligue o Explorador de Armazenamento à conta de armazenamento, de modo a verificar que a sua aplicação lógica guarda corretamente os anexos como blobs no contentor.

1. Abra o Explorador de Armazenamento do Microsoft Azure. Quando o Explorador de Armazenamento lhe pedir uma ligação ao armazenamento do Azure, escolha **Use a storage account name and key** (Utilizar o nome e a chave de uma conta de armazenamento) > **Next** (Seguinte).
Não se aparecer nenhum pedido, escolha **Add account** (Adicionar conta) na barra de ferramentas do Explorador.

2. Em **Attach using Name and Key** (Anexar com o Nome e a Chave), introduza o nome e a chave de acesso da conta de armazenamento que guardou anteriormente. Escolha **Next** (Seguinte)  > **Connect** (Ligar).

3. Verifique se a conta de armazenamento e os contentores aparecem corretamente no Explorador de Armazenamento:

   1. Em **Explorer** (Explorador), expanda **(Local and Attached)** (Locais e Anexados) > 
    **Storage Accounts** (Contas de Armazenamento) > **attachmentstorageaccount** > 
    **Blob Containers** (Contentores de Blobs).

   2. Confirme que o contentor "attachments" aparece agora. 
   Por exemplo:

      ![Explorador de Armazenamento - confirmar o contentor de armazenamento](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Depois, crie uma [função do Azure](../azure-functions/functions-overview.md) que remove o HTML dos e-mails recebidos.

## <a name="create-a-function-to-clean-html"></a>Criar uma função para limpar o HTML

Agora, utilize o fragmento de código fornecido nestes passos para criar uma função do Azure que remove o HTML de cada e-mail recebido. Desta forma, o conteúdo dos e-mails fica mais limpo e é processado mais facilmente. Depois, pode chamar esta função a partir da sua aplicação lógica.

1. Antes de poder criar uma função, [crie uma aplicação de funções](../azure-functions/functions-create-function-app-portal.md) com estas definições:

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome da aplicação**  | CleanTextFunctionApp | Um nome globalmente exclusivo e descritivo para a sua aplicação de funções | 
   | **Subscrição** | <*your-Azure-subscription-name*> | A mesma subscrição do Azure que utilizou anteriormente | 
   | **Grupo de Recursos** | LA-Tutorial-RG | O mesmo grupo de recursos do Azure que utilizou anteriormente | 
   | **Plano de Alojamento** | Plano de Consumo | Esta definição determina como alocar e dimensionar os recursos, como o poder de computação, para executar a aplicação de funções. Veja [Hosting plans comparison](../azure-functions/functions-scale.md) (Comparação dos planos de alojamento). | 
   | **Localização** | EUA Leste 2 | A mesma região que utilizou anteriormente | 
   | **Armazenamento** | cleantextfunctionstorageacct | Crie uma conta de armazenamento para a sua aplicação de funções. Utilize apenas letras minúsculas e números. <p>**Nota:** esta conta de armazenamento contém as suas aplicações de funções e é diferente da conta de armazenamento criada anteriormente para os anexos de e-mails. | 
   | **Application Insights** | Desativado | Ativa a monitorização de aplicações com o [Application Insights](../application-insights/app-insights-overview.md); contudo, neste tutorial, mantenha a definição **Desativado**. | 
   |||| 

   Se a sua aplicação de funções não se abrir automaticamente após a implementação, localiz-a no <a href="https://portal.azure.com" target="_blank">portal do Azure</a>. No menu principal do Azure, escolha **App Services** (Serviços de Aplicações) e selecione a sua aplicação de funções.

   ![Aplicação de funções criada](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Se **App Services** não aparecer no menu do Azure, aceda a **More services** (Mais serviços). Na caixa de pesquisa, localize e selecione **Function Apps** (Aplicações de Funções). Para obter mais informações, veja [Criar a sua função](../azure-functions/functions-create-first-azure-function.md).

   Também pode utilizar a [CLI do Azure](../azure-functions/functions-create-first-azure-function-azure-cli.md) ou [modelos do PowerShell e do Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

2. Em **Function Apps**, expanda **CleanTextFunctionApp** e selecione **Functions** (Funções). Na barra de ferramentas das funções, escolha **+ New function** (+ Nova Função).

   ![Criar função nova](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. Em **Choose a template below or go to the quickstart** (Escolher um modelo abaixo ou aceder ao início rápido), selecione o modelo de função **HttpTrigger - C#**.

   ![Selecionar o modelo de função](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. Em **Name your function** (Dar nome à função), introduza ```RemoveHTMLFunction```. Em **HTTP trigger** (Acionador HTTP)  > **Authorization level** (Nível de autorização), mantenha o valor **Function** (Função) predefinido e escolha **Create** (Criar).

   ![Dê um nome à sua função](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Quando o editor estiver aberto, substitua o código do modelo por este código, o que remove o HTML e devolve resultados ao chamador.

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Quando tiver terminado, escolha **Save** (Guardar). Para testar a sua função, escolha **Testar** no ícone de seta (**<**), no lado direito do editor. 

   ![Abrir o painel "Test" ("Teste")](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. No painel **Test** (Teste), em **Request Body** (Corpo do Pedido), introduza esta linha e escolha **Run** (Executar).

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Testar a função](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   A janela **Saída** mostra este resultado da função:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Depois de confirmar que a função está a funcionar, crie a aplicação lógica. Embora este tutorial mostre como criar uma função que remove o HTML dos e-mails, o Logic Apps também tem um conector **HTML to Text** (HTML para Texto).

## <a name="create-your-logic-app"></a>Criar uma aplicação lógica

1. No menu principal do Azure, escolha **Nova** > **Integração Empresarial** > **Aplicação Lógica**.

   ![Criar uma aplicação lógica](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. Em **Criar aplicação lógica**, indique estas informações sobre a sua aplicação lógica, conforme mostrado e descrito. Quando tiver terminado, escolha **Afixar ao dashboard** > **Criar**.

   ![Indicar as informações da aplicação lógica](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome** | LA-ProcessAttachment | O nome para a aplicação lógica | 
   | **Subscrição** | <*your-Azure-subscription-name*> | A mesma subscrição do Azure que utilizou anteriormente | 
   | **Grupo de recursos** | LA-Tutorial-RG | O mesmo grupo de recursos do Azure que utilizou anteriormente |
   | **Localização** | EUA Leste 2 | A mesma região que utilizou anteriormente | 
   | **Log Analytics** | Desativado | Neste tutorial, mantenha a definição **Desativado**. | 
   |||| 

3. Depois de o Azure implementar a aplicação, o Estruturador de Aplicações Lógicas abre-se e mostra uma página com um vídeo de introdução e modelos de padrões de aplicações lógicas comuns. Em **Modelos**, escolha **Aplicação Lógica em Branco**.

   ![Escolher o modelo de aplicação lógica em branco](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Em seguida, adicione um [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts) que escuta os e-mails recebidos que têm anexos. Todas as aplicações lógicas têm de ser iniciadas com um acionador, que é acionado quando um evento específico acontece ou quando dados novos satisfazem uma condição específica. Para obter mais informações, veja [Criar a sua primeira aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Monitorizar os e-mails recebidos

1. No estruturador, introduza "when email arrives" (quando chegarem e-mails) na caixa de pesquisa. Selecione este acionador para o seu fornecedor de e-mail: **<*fornecedor_de_e-mail*> - When a new email arrives**, como, por exemplo:

   ![Selecione este acionador para o fornecedor de e-mail: "When a new email arrives"](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Relativamente a contas escolares ou profissionais do Azure, selecione Office 365 Outlook (Outlook do Office 365). 
   * Quanto a contas Microsoft pessoais, selecione Outlook.com. 

2. Se lhe forem pedidas credenciais, inicie sessão na sua conta de e-mail para que o Logic Apps se possa ligar à mesma.

3. Agora, indique os critérios que o acionador utiliza para filtrar os e-mails novos.

   1. Especifique a pasta, o intervalo e a frequência para a verificação dos e-mails.

      ![Especifique a pasta, o intervalo e a frequência para a verificação dos e-mails](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Definição | Valor | Descrição | 
      | ------- | ----- | ----------- | 
      | **Pasta** | A Receber | A pasta de e-mail a verificar | 
      | **Intervalo** | 1 | O número de intervalos de espera entre verificações | 
      | **Frequência** | Minuto | A unidade de tempo para cada intervalo entre verificações | 
      |  |  |  | 
  
   2. Escolha **Mostrar opções avançadas** e especifique estas definições:

      | Definição | Valor | Descrição | 
      | ------- | ----- | ----------- | 
      | **Tem Anexo** | Sim | Obter apenas os e-mails com anexos. <p>**Nota:** o acionador não remove e-mails da sua conta; verifica apenas as mensagens novas e só processa os e-mails que correspondam ao filtro do assunto. | 
      | **Incluir Anexos** | Sim | Obter os anexos como entrada para o seu fluxo de trabalho em vez de verificar apenas os anexos. | 
      | **Filtro de Assunto** | ```Business Analyst 2 #423501``` | O texto a localizar no assunto do e-mail | 
      |  |  |  | 

4. Para ocultar os detalhes do acionador por agora, clique no interior da barra de título do mesmo.

   ![Fechar forma para ocultar os detalhes](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Guarde a aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**.

   A sua aplicação lógica está agora ativa, mas não faz mais nada que não verificar os e-mails. 
   Em seguida, adicione uma condição que especifique critérios para continuar o fluxo de trabalho.

## <a name="check-for-attachments"></a>Verificar a existência de anexos

1. No acionador, escolha **+ New step** Novo passo)  > **Add a condition** (Adicionar uma condição).

   Quando a forma da condição aparecer, é apresentada, por predefinição, a lista de parâmetros ou a lista de conteúdo dinâmico, que mostra todos os parâmetros do passo anterior que podem ser incluídos como entradas do fluxo de trabalho. 
   A largura do browser determina a lista que é apresentada.

2. Mude o nome da condição com uma descrição melhor.

   1. Na barra de título da condição, escolha o botão **reticências** (**...** ) > **Rename** (Mudar o nome).

      Por exemplo, se o browser estiver na vista estreita:

      ![Mudar o nome da condição](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Se o browser estiver na vista alargada e a lista de conteúdo dinâmico bloquear o acesso ao botão de reticências, escolha **Add dynamic content** (Adicionar conteúdo dinâmico), dentro da condição, para fechar a lista. 
      
      ![Fechar a lista de conteúdo dinâmico](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Mude o nome da condição com a descrição ```If email has attachments and key subject phrase```

3. Indique uma expressão para descrever a condição. 

   1. Dentro da forma da condição, escolha **Edit in advanced mode** (Editar no modo avançado).

      ![Editar condição no modo avançado](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. Na caixa de texto, introduza esta expressão:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      Esta expressão compara o valor da propriedade **HasAttachment** do corpo do acionador, que é o e-mail neste tutorial, com o objeto booleano ```True```. 
      Se ambos os valores forem iguais, o e-mail tem, pelo menos, um anexo, a condição é transmitida e o fluxo de trabalho continua.

      A sua condição terá agora este aspeto:

      ![Expressão da condição](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Escoolha **Edit in basic mode** (Editar no modo básico). A sua expressão é agora resolvida da seguinte forma:

      ![Expressão resolvida](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Para criar uma expressão manualmente, tem de trabalhar no modo básico e ter aberta a lista a dinâmica, para poder funcionar com o estruturador de expressões. Em **Expression** (Expressão), pode selecionar as funções. Em **Dynamic content** (Conteúdo dinâmico), pode selecionar os campos dos parâmetros que vão ser utilizados nessas funções.
      > Este tutorial mostra como criar as expressões manualmente mais à frente.

4. Guarde a aplicação lógica.

### <a name="test-your-condition"></a>Testar a condição

Agora, teste para verificar se a condição funciona corretamente:

1. Se a sua aplicação lógica ainda não estiver em execução, escolha **Run** (Executar), na barra de ferramentas do estruturador.

   Este passo inicia manualmente a aplicação lógica sem ter de esperar que o intervalo que especificou passe. 
   No entanto, nada acontece enquanto o e-mail de teste não chegar à sua caixa de entrada. 

2. Envie um e-mail para si próprio que cumpra estes critérios:

   * O assunto do e-mail tem o texto que especificou no **filtro de assunto** do acionador, ```Business Analyst 2 #423501```

   * O e-mail tem um anexo. 
   Por agora, crie apenas um ficheiro de texto vazia e anexe-o ao e-mail.

   Quando o e-mail chegar, a aplicação lógica verifica a existência de anexos e o texto do assunto especificado.
   Se a condição for aprovada, o acionador é acionado e faz com que o motor do Logic Apps crie uma instância da aplicação lógica e inicie o fluxo de trabalho. 

3. Para verificar se o acionador foi acionado e a aplicação lógica executada com êxito, no menu da aplicação lógica, escolha **Overview** (Descrição Geral).

   ![Verificar o acionador e o histórico de execuções](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Se a sua aplicação lógica não for acionada ou executada, apesar de o acionador ter sido bem-sucedido, veja [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Resolver problemas da sua aplicação lógica).

Em seguida, defina as ações a realizar para o ramo **Se verdadeiro**. Para guardar o e-mail juntamente com eventuais anexos, remova todo o HTML do corpo do e-mail e crie blobs no contentor de armazenamento para a mensagem e os anexos.

> [!NOTE]
> A aplicação lógica não tem de fazer nada no ramo **Se falso** se os e-mails não tiverem anexos. Como exercício de bónus após concluir este tutorial, pode adicionar qualquer ação adequada que queira realizar no ramo **Se falso**.

## <a name="call-the-removehtmlfunction"></a>Chamar RemoveHTMLFunction

1. No menu da aplicação lógica, selecione **Logic App Designer** (Estruturador da Aplicação Lógica). No ramo **Se verdadeiro**, escolha **Add an action** (Adicionar uma ação).

2. Procure "azure functions" e selecione a ação **Azure Functions – Choose an Azure function**

   ![Selecionar a ação "Azure Functions - Choose an Azure function"](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Selecione a aplicação de função criada anteriormente, **CleanTextFunctionApp**

   ![Selecionar a aplicação de funções do Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Agora, selecione a função **RemoveHTMLFunction**

   ![Selecionar a função do Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Mude o nome da forma da função com a descrição ```Call RemoveHTMLFunction to clean email body``` 

6. Na forma da função, introduza a entrada que a função vai processar. Especifique o corpo do e-mail conforme mostrado e descrito aqui:

   ![Especificar o corpo do pedido que a função vai esperar](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. Em **Request Body** (Corpo do Pedido), introduza este texto: 
   
      ```{ "emailBody": ``` 

      Enquanto não concluir esta entrada nos passos seguintes, é apresentado um erro relativo a JSON inválido.
      Quando testou esta função anteriormente, a entrada especificada para a mesma utilizou JavaScript Object Notation (JSON). 
      Por isso, o corpo do pedido também tem de utilizar o mesmo formato. 

   2. Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione o campo **Body** em **When a new email arrives**.
   Após o campo **Body** (Corpo), adicione a chaveta de fecho, ```}```

      ![Especificar o corpo do pedido que vai ser transmitido à função](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      Na definição da aplicação lógica, esta entrada é apresentada no formato:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Guarde a aplicação lógica.

Em seguida, adicione uma ação que cria um blob no contentor de armazenamento para guardar o corpo do e-mail.

## <a name="create-blob-for-email-body"></a>Criar blob para o corpo do e-mail

1. Na forma da função do Azure, escolha **Add an action**. 

2. Em **Choose an action** (Escolher uma ação), procure por "blob e selecione a ação **Azure Blob Storage – Create blob**

   ![Adicionar ação para criar blob para o corpo do e-mail](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Se não tiver uma ligação a uma conta de armazenamento do Azure, crie-a com estas definições, conforme mostrado e descrito aqui. Quando tiver terminado, escolha **Create** (Criar).

   ![Criar ligação à conta de armazenamento](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Nome da Ligação** | AttachmentStorageConnection | Um nome descritivo para a ligação | 
   | **Storage Account** | attachmentstorageacct | O nome da conta de armazenamento que criou anteriormente para guardar anexos | 
   |||| 

4. Mude o nome da ação **Create blob** pela descrição ```Create blob for email body```

5. Na ação **Create blob**, indique estas informações e selecione estes parâmetros para criar o blob, conforme mostrado e descrito:

   ![Indicar as informações do blob para o corpo do e-mail](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Folder path** | /attachments | O caminho e o nome do contentor que criou anteriormente. Também pode procurar e selecionar um contentor. | 
   | **Blob name** | Campo **From** | Transmita o nome do remetente do e-mail como o nome do blob. Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione **From** (De) em **When a new email arrives**. | 
   | **Blob content** | Campo **Content** | Transmita o corpo do e-mail sem HTML como o conteúdo do blob. Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione **Body** em **Call RemoveHTMLFunction to clean email body**. |
   |||| 

6. Guarde a aplicação lógica. 

### <a name="check-attachment-handling"></a>Verificar o processamento de anexos

Agora, teste se a sua aplicação lógica processa os e-mails da forma que especificou:

1. Se a sua aplicação lógica ainda não estiver em execução, escolha **Run** (Executar), na barra de ferramentas do estruturador.

2. Envie um e-mail para si próprio que cumpra estes critérios:

   * O assunto do e-mail tem o texto que especificou no **filtro de assunto** do acionador, ```Business Analyst 2 #423501```

   * O e-mail tem, pelo menos, um anexo. 
   Por agora, crie apenas um ficheiro de texto vazia e anexe-o ao e-mail.

   * O e-mail tem algum conteúdo de teste no corpo, como, por exemplo: 

     ```
     Testing my logic app
     ```

   Se a sua aplicação lógica não for acionada ou executada, apesar de o acionador ter sido bem-sucedido, veja [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Resolver problemas da sua aplicação lógica).

3. Confirme que a aplicação lógica guardou o e-mail no contentor de armazenamento correto. 

   1. No Explorador de Armazenamento, expanda **(Local and Attached)** > 
   **Storage Accounts** > **attachmentstorageacct (External)** > 
   **Blob Containers** > **attachments** (anexos).

   2. Verifique o contentor **attachments** relativamente ao e-mail. 

      Neste momento, apenas o e-mail aparece no contentor, porque a aplicação lógica ainda não processa anexos.

      ![Verificar o e-mail guardado no Explorador de Armazenamento](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Quando tiver terminado, elimine o e-mail no Explorador de Armazenamento.

4. Opcionalmente, para testar o ramo **Se falso**, que não faz nada nesta fase, pode enviar um e-mail que não cumpra os critérios.

Em seguida, adicione um ciclo para processar todos os anexos de e-mail.

## <a name="process-attachments"></a>Processar anexos

Esta aplicação lógica utiliza um ciclo **for each** para processar cada anexo no e-ail.

1. Na forma **Create blob for email body**, escolha **… More** (Mais) e selecione o comando **Add a for each**

   ![Adicionar ciclo "for each"](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Mude o nome do ciclo com a descrição ```For each email attachment```

3. Agora, especifique os dados que o ciclo vai processar. Clique no interior da caixa **Select an output from previous steps** (Selecionar uma saída dos passos anteriores). Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione **Attachments**. 

   ![Selecionar "Attachments"](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   O campo **Attachments** transmite uma matriz que contém todos os anexos incluídos num e-mail. 
   O ciclo **For each** repete as ações em cada item que é transmitido com a matriz.

4. Guarde a aplicação lógica.

Em seguida, adicione a ação que guarda cada anexo como um blob no seu contentor de armazenamento **attachments**.

## <a name="create-blobs-for-attachments"></a>Criar blobs para anexos

1. No ciclo **For each**, escolha **Add an action**, para poder especificar a tarefa a realizar em cada anexo encontrado.

   ![Adicionar ação ao ciclo](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. Em **Choose an action**, procure por "blob" e selecione a ação **Azure Blob Storage – Create blob**

   ![Adicionar ação para criar blob](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Mude o nome da ação **Create blob 2** pela descrição ```Create blob for each email attachment```

4. Na ação **Create blob for each email attachment**, indique estas informações e selecione os parâmetros para criar cada blob, conforme mostrado e descrito:

   ![Indicar as informações dos blobs](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Definição | Valor | Descrição | 
   | ------- | ----- | ----------- | 
   | **Folder path** | /attachments | O caminho e o nome do contentor que criou anteriormente. Também pode procurar e selecionar um contentor. | 
   | **Blob name** | Campo **Name** | Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione **Name** (Nome) para transmitir o nome do anexo ao nome do blob. | 
   | **Blob content** | Campo **Content** | Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione **Content** (Conteúdo) para transmitir o conteúdo do anexo ao conteúdo do blob. |
   |||| 

5. Guarde a aplicação lógica. 

### <a name="check-attachment-handling"></a>Verificar o processamento de anexos

Em seguida, teste se a sua aplicação lógica processa os anexos da forma que especificou:

1. Se a sua aplicação lógica ainda não estiver em execução, escolha **Run** (Executar), na barra de ferramentas do estruturador.

2. Envie um e-mail para si próprio que cumpra estes critérios:

   * O assunto do e-mail tem o texto que especificou no **filtro de assunto** do acionador, ```Business Analyst 2 #423501```

   * O seu e-mail tem, pelo menos, dois anexos. 
   Por agora, crie apenas dois ficheiros de texto vazios e anexe-os ao e-mail.

   Se a sua aplicação lógica não for acionada ou executada, apesar de o acionador ter sido bem-sucedido, veja [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Resolver problemas da sua aplicação lógica).

3. Confirme que a aplicação lógica guardou o e-mail e os anexos no contentor de armazenamento correto. 

   1. No Explorador de Armazenamento, expanda **(Local and Attached)** > 
   **Storage Accounts** > **attachmentstorageacct (External)** > 
   **Blob Containers** > **attachments** (anexos).

   2. Verifique o contentor **attachments** relativamente ao e-mail e aos anexos.

      ![Verificar o e-mail e os anexos guardados](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Quando tiver terminado, elimine o e-mail e os anexos no Explorador de Armazenamento.

Em seguida, adicione uma ação para que a sua aplicação lógica envia um e-mail para rever os anexos.

## <a name="send-email-notifications"></a>Enviar notificações por e-mail

1. No ramo **Se verdadeiro**, no ciclo **For each email attachment**, escolha **Add an action**. 

   ![Adicionar ação no ciclo "for each"](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. Em **Escolher uma ação**, procure "enviar e-mail, selecione a ação "enviar e-mail" para o fornecedor de e-mail que quiser. Para filtrar a lista de ações para um serviço específico, pode selecionar primeiro o conector em **Conectores**.

   ![Selecionar a ação "send email" relativa ao seu fornecedor de e-mail](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Relativamente a contas escolares ou profissionais do Azure, selecione Office 365 Outlook (Outlook do Office 365). 
   * Quanto a contas Microsoft pessoais, selecione Outlook.com. 

3. Se lhe forem pedidas credenciais, inicie sessão na sua conta de e-mail para que o Logic Apps crie uma ligação para a sua conta de e-mail.

4. Mude o nome da ação **Send an email** pela descrição ```Send email for review```

5. Indique as informações desta ação e selecione os campos que pretende incluir no e-mail, conforme mostrado e descrito. Para adicionar linhas em branco a uma caixa de edição, prima Shift + Enter.  

   Por exemplo, se estiver a trabalhar com a lista de conteúdo dinâmico:

   ![Enviar notificação por e-mail](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Se não encontrar um campo esperado na lista, selecione **See more** (Ver mais), junto a **When a new email arrives**, na lista de conteúdo dinâmico ou no final da lista de parâmetros.

   | Definição | Valor | Notas | 
   | ------- | ----- | ----- | 
   | **Para** | <*recipient-email-address*> | Para fins de teste, pode utilizar o seu próprio endereço de e-mail. | 
   | **Assunto**  | ```ASAP - Review applicant for position: ``` **Subject** | O assunto do e-mail que pretende incluir. Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione o campo **Subject** (Assunto) em **When a new email arrives**. | 
   | **Corpo** | ```Please review new applicant:``` <p>```Applicant name: ``` **From** <p>```Application file location: ``` **Path** <p>```Application email content: ``` **Body** | O conteúdo do corpo do e-mail. Na lista de parâmetros ou na lista de conteúdo dinâmico, selecione os campos: <p>- **From**, em **When a new email arrives** </br>- **Path**, em **Create blob for email body** </br>- **Body**, em **Call RemoveHTMLFunction to clean email body** | 
   |||| 

   Se selecionar um campo que contém uma matriz, como **Content**, que é uma matriz que contém anexos, o estruturador adiciona automaticamente um ciclo “For each” em torno da ação que referencia esse campo. 
   Desta forma, a sua aplicação lógica pode realizar essa ação em cada item da matriz. 
   Para remover o ciclo, remova o campo da matriz, mova a ação de referenciação para fora do ciclo, selecione as reticências (**...** ) na barra de título do ciclo e escolha **Delete** (Eliminar).
     
6. Guarde a aplicação lógica. 

Em seguida, teste a sua aplicação lógica, que é agora semelhante a este exemplo:

![Aplicação lógica concluída](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Executar a aplicação lógica

1. Envie um e-mail para si próprio que cumpra estes critérios:

   * O assunto do e-mail tem o texto que especificou no **filtro de assunto** do acionador, ```Business Analyst 2 #423501```

   * O seu e-mail tem um ou mais anexos. 
   Pode reutilizar um ficheiro de texto vazio do teste anterior. 
   Para um cenário realista, anexe um ficheiro de currículo.

   * O corpo do e-mail tem este texto, que pode copiar e colar:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Execute a aplicação lógica. Se tiver êxito, a aplicação lógica envia-lhe um e-mail semelhante ao deste exemplo:

   ![Notificação de e-mail enviada pela aplicação lógica](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Se não receber nenhuma mensagem de e-mail, verifique a pasta de lixo do e-mail. 
   O filtro de lixo de e-mail poderá redirecionar estes tipos de mensagem de e-mail. 
   Caso contrário, se não tiver a certeza de que a aplicação lógica foi executada corretamente, veja [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md) (Resolver problemas da sua aplicação lógica).

Parabéns! Criou e executou uma aplicação lógica que automatiza tarefas em diferentes serviços do Azure e que chama um código personalizado.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos que contém a aplicação lógica e os recursos relacionados. No menu principal do Azure, aceda a **Grupos de recursos** e selecione o grupo de recursos para a sua aplicação lógica. Escolha **Eliminar grupo de recursos**. Introduza o nome do grupo de recursos como confirmação e escolha **Eliminar**.

![Eliminar grupo de recursos da aplicação lógica](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma aplicação lógica que processa e armazena anexos de e-mail através da integração de serviços do Azure, como o Armazenamento do Azure e as Funções do Azure. Agora, saiba mais sobre outros conectores que pode utilizar para criar aplicações lógicas.

> [!div class="nextstepaction"]
> [Saiba mais sobre os conectores do Logic Apps](../connectors/apis-list.md)
