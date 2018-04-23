---
title: Registar conectores personalizados - Azure Logic Apps | Microsoft Docs
description: Configurar conectores personalizados para utilização em Logic Apps do Azure
author: ecfan
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 722b82aabe796367d906e9338355b83a1a8b1e1c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/26/2018
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Registar conectores personalizados no Azure Logic Apps

Depois de criar a API REST, configurar a autenticação e obter o ficheiro de definição de OpenAPI ou uma coleção de Postman, está pronto para registar o conector. 

## <a name="prerequisites"></a>Pré-requisitos

Para registar o conector personalizado, terá destes itens:

* Detalhes para registar o conector no Azure, tais como o nome, a subscrição do Azure, o grupo de recursos do Azure e a localização que pretende utilizar

* Um ficheiro de OpenAPI (Swagger) ou uma coleção de Postman que descrevem a API

  Para este tutorial, pode utilizar o [ficheiro OpenAPI de Gestor de recursos do Azure de exemplo](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Um ícone que representa o conector

* Uma descrição breve para o conector

* A localização de anfitrião para a API

## <a name="1-create-your-connector"></a>1. Criar o conector

1. No portal do Azure, no menu principal do Azure, escolha **crie um recurso**. Na caixa de pesquisa, introduza "conector do logic apps" como o filtro e prima Enter.

   ![Novo, procurar “conector do logic apps”](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Na lista de resultados, escolha **Conector do Logic Apps** > **Criar**.

   ![Criar conector do Logic Apps](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Indique os detalhes para registar o conector, conforme descrito na tabela. Quando tiver terminado, escolha **Afixar ao dashboard** > **Criar**.

   ![Detalhes do conector personalizado da Aplicação Lógica](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Propriedade | Valor sugerido | Descrição | 
   | -------- | --------------- | ----------- | 
   | **Nome** | *nome-do-conector-personalizado* | Indique um nome para o conector. | 
   | **Subscrição** | *nome-da-subscrição-do-Azure* | Selecione a sua subscrição do Azure. | 
   | **Grupo de recursos** | *nome-do-grupo-de-recursos-do-Azure* | Crie ou selecione um grupo do Azure para organizar os seus recursos do Azure. | 
   | **Localização** | *região-da-implementação* | Selecione uma região de implementação para o conector. | 
   |||| 

   Quando o Azure implementar o conector, o menu do conector personalizado abre-se automaticamente. 
   Se não, escolha o seu conector personalizado a partir do dashboard do Azure.

## <a name="2-define-your-connector"></a>2. Definir o conector

Agora pode Especifica o ficheiro de OpenAPI ou coleção de Postman para criar o conector, a autenticação, que utiliza o conector, ações e acionadores que fornece o conetor personalizado e os parâmetros que pode utilizar as ações e é acionado.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. Especifique o ficheiro de OpenAPI ou coleção de Postman para o conector

1. No menu do seu conector, se ainda não estiver selecionado, escolha **Conector do Logic Apps**. Na barra de ferramentas, escolha **Editar**.

   ![Editar conector personalizado](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Escolha **geral** para que possa fornecer os detalhes nestas tabelas para a criação, proteger e definir as ações e acionadores para o conetor personalizado.

   1. Para **conectores personalizada**, selecione uma opção para que possam fornecer o ficheiro de OpenAPI (Swagger) que descrevem a API.

      ![Disponibilize o ficheiro de OpenAPI para a API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Opção | Formato |Descrição | 
      | ------ | ------ | ----------- | 
      | **Carregar um ficheiro de OpenAPI** | *OpenAPI (Swagger)-json-file* | Navegue para a localização para o ficheiro OpenAPI e selecione esse ficheiro. | 
      | **Utilizar um URL de OpenAPI** | http://*path-to-swagger-json-file* | Forneça o URL para o ficheiro de OpenAPI da sua API. | 
      | **Carregar a coleção de Postman V1** | *exported-Postman-collection-V1-file* | Navegue para a localização para uma coleção de Postman exportada no formato V1. | 
      |||| 

   2. Para **Informações gerais**, carregue um ícone para o conector. 
   Normalmente, o **Descrição**, **anfitrião**, e **Base URL** campos serão preenchidos automaticamente a partir do seu ficheiro OpenAPI. 
   Contudo, se isso não acontecer, adicione estas informações, conforme descrito na tabela, e escolha **Continuar**. 

      ![Detalhes do conector](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Opção ou definição | Formato | Descrição | 
      | ----------------- | ------ | ----------- | 
      | **Carregar Ícone** | *ficheiro-png-ou-jpg-com-menos-de-1-MB* | Um ícone que representa o conector <p>Cor: preferencialmente, um logótipo branco num fundo colorido. <p>Dimensões: logótipo com cerca de 160 pixels num quadrado com 230 pixels | 
      | **Cor de fundo do ícone** | *código-hexadecimal-da-cor-da-marca-do-ícone* | <p>A cor por trás do ícone que corresponde à cor de fundo no ficheiro do ícone. <p>Formato: hexadecimal. Por exemplo, #007ee5 representa a cor azul. | 
      | **Descrição** | *descrição-do-conector* | Indique uma descrição breve do conector. | 
      | **Anfitrião** | *anfitrião-do-conector* | Forneça o domínio do anfitrião para a API Web. | 
      | **URL Base** | *URL-base-do-conector* | Forneça o URL de base para a API Web. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Descrever a autenticação que o conector utiliza

1. Agora, escolha **Segurança**, para rever ou descrever a autenticação que o seu conector utiliza. A autenticação certifica-se de que as identidades dos seus utilizadores fluem adequadamente entre o seu serviço e os clientes.

   ![Tipo de autenticação](./media/logic-apps-custom-connector-register/security.png)

   * Se carregar um ficheiro de OpenAPI, o Assistente de registo Deteta automaticamente o tipo de autenticação que utiliza a API Web, e preenche automaticamente a **segurança** secção no assistente, com base no `securityDefinitions` objeto que ficheiro. Por exemplo, aqui está uma secção que especifica OAuth2.0 a utilizar:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * Se o ficheiro OpenAPI não preencher as propriedades e o tipo de autenticação, escolha **editar** pelo que pode adicionar esta informação. 
   
     Por exemplo, se anteriormente [configurar a autenticação do Azure AD neste tutorial](../logic-apps/custom-connector-azure-active-directory-authentication.md), criar aplicações do Azure AD para proteger a sua API Web e o conector. 
     Por isso, agora pode fornecer a chave de cliente e o ID de aplicação que guardou anteriormente:
    
     | Definição | Valor sugerido | Descrição | 
     | ------- | --------------- | ----------- | 
     | **Tipo de autenticação** | OAuth 2.0 | | 
     | **Fornecedor de identidade** | Azure Active Directory | | 
     | **id de cliente** | *application-ID-for-connector-Azure-AD-app* | O ID da aplicação que guardou anteriormente para a aplicação do seu conector do Azure AD | 
     | **Segredo do cliente** | *client-key-for-connector-Azure-AD-app* | A chave de cliente para a aplicação do seu conector Azure AD | 
     | **URL de início de sessão** | `https://login.windows.net` | | 
     | **URL de recurso** | `https://management.core.windows.net/` | Certifique-se de que adiciona o URL exatamente conforme especificado, incluindo a barra no final. | 
     |||| 

   * Uma coleção de Postman, o que gera automaticamente um ficheiro de OpenAPI para si, preenche automaticamente o tipo de autenticação *apenas* ao utilizar os tipos de autenticação suportadas, tais como o OAuth 2.0 ou básico.

2. Para guardar o conector depois de introduzir as informações de segurança, na parte superior da página, escolha **Atualizar conector** e **Continuar**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Rever, atualizar ou definir ações e acionadores para o conector

1. Agora, escolha **Definição**, para rever, editar ou definir ações e acionadores novos que os utilizadores podem adicionar aos respetivos fluxos de trabalho.

   Ações e acionadores baseiam-se nas operações definidas no seu ficheiro OpenAPI ou coleção de Postman preencher automaticamente o **definição** página e incluir os valores de pedido e resposta. Assim, se as operações necessárias já aparecerem aqui, pode avançar para o passo seguinte do processo de registo sem fazer alterações nesta página.

   ![Definição do conector](./media/logic-apps-custom-connector-register/definition.png)

2. Opcionalmente, se pretender editar ações existentes e acionadores ou adicionar novos, continue com estes passos.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Editar ou adicionar ações para o conector

1. Para editar uma ação existente, escolha o número de ação. Para adicionar uma ação que não existe no seu ficheiro OpenAPI ou coleção de Postman em **ações**, escolha **nova ação**.

2. Em **geral**, fornecer ou editar estas informações para a ação:
   
   | Definição | Valor sugerido | Descrição | 
   | ------- | --------------- | ----------- | 
   | **Resumo** | *operation-name* | O título para esta ação | 
   | **Descrição** | *operation-description* | A descrição para esta ação. <p>**Sugestão**: Certifique-se de que a descrição do seu termina com um período. |
   | **Operation ID** (ID da operação) | *operation-identifier* | Um nome exclusivo para identificar esta ação. Utilizar camel maiúsculas e minúsculas, por exemplo: "GetPullRequest" | 
   |**Visibility** (Visibilidade)| **nenhum**, **avançadas**, **interno**, ou **importantes** | A visibilidade destinada ao utilizador para esta ação. Para obter mais informações, consulte [OpenAPI extensões](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Agora defina o pedido para a ação.
  
   1. No **pedido** secção, escolha **importação da amostra**. 

   2. No **importação da amostra** página, cole um exemplo de pedido. 

      Normalmente, pedidos de exemplo estão disponíveis na documentação de API, onde pode obter informações para o **verbo**, **URL**, **cabeçalhos**, e **corpo**campos. Por exemplo, consulte o [documentação da API de análise de texto](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Exemplo de pedido de importação](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Se criar um conector de uma coleção de Postman, certifique-se de que remove o `Content-type` cabeçalho de ações e é acionado. As Logic Apps adiciona automaticamente este cabeçalho. Além disso, remover os cabeçalhos de autenticação que definiu no `Security` secção de ações e é acionado.

      Para a funcionalidade de OpenAPI avançada, consulte [OpenAPI extensões para conectores personalizados](../logic-apps/custom-connector-openapi-extensions.md).

   3. Para concluir a definição de pedido, escolha **importação**.

4. Agora, defina a resposta para a ação.

   1. Em **resposta**, pode especificar uma resposta de predefinição. 
   Escolha **Add default response** (Adicionar resposta predefinida).

   2. No **importação da amostra** página, cole uma resposta de amostra, em seguida, escolha **importação**.

5. Por fim, em **validação**, reveja e corrija quaisquer problemas potenciais identificados para a ação.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Editar ou adicionar acionadores para o conector

1. Para editar um acionador existente, escolha o número de para este acionador. Para adicionar um acionador que não existe no seu ficheiro OpenAPI ou coleção de Postman em **Acionadores**, escolha **novo acionador**.

2. Em **geral**, fornecer ou editar estas informações para o acionador:

   | Definição | Valor sugerido | Descrição | 
   | ------- | --------------- | ----------- | 
   | **Resumo** | *operation-name* | O título para este acionador | 
   | **Descrição** | *operation-description* | A descrição para este acionador. <p>**Sugestão**: Certifique-se de que a descrição do seu termina com um período. | 
   | **Operation ID** (ID da operação) | *operation-identifier* | Um nome exclusivo para identificar este acionador. Utilizar camel maiúsculas e minúsculas, por exemplo: "TriggerOnGitHubPushEvent" | 
   |**Visibility** (Visibilidade)| **nenhum**, **avançadas**, **interno**, ou **importantes** | A visibilidade destinada ao utilizador para este acionador. Para obter mais informações, consulte [OpenAPI extensões](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Trigger type** (Tipo de acionador) | **Webhook** ou **de consulta** | O tipo para este acionador. Por exemplo, um acionador de webhook aguarda que um evento específico ocorrer antes de acionadas. Um acionador de consulta verifica regularmente um ponto final de serviço com base num intervalo especificado e a frequência. <p>Para obter mais informações sobre o webhook e padrões de Acionador de consulta, consulte [criar APIs personalizadas](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. Agora defina o pedido que cria o acionador. 

   1. No **pedido** secção, escolha **importação da amostra**.

   2. No **importação da amostra** página, cole um exemplo de pedido. 

      Normalmente, pedidos de exemplo estão disponíveis na documentação de API, onde pode obter informações para o **verbo**, **URL**, **cabeçalhos**, e **corpo**campos. Por exemplo, consulte o [documentação da API de análise de texto](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Exemplo de pedido de importação](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Se criar um conector de uma coleção de Postman, certifique-se de que remove o `Content-type` cabeçalho de ações e é acionado. As Logic Apps adiciona automaticamente este cabeçalho. Além disso, remover os cabeçalhos de autenticação que definiu no `Security` secção de ações e é acionado.

      Para a funcionalidade de OpenAPI avançada, consulte [OpenAPI extensões para conectores personalizados](../logic-apps/custom-connector-openapi-extensions.md).

   3. Para concluir a definição de pedido, escolha **importação**. 

4. Agora defina resposta o acionador. No **resposta** secção, com base no tipo o acionador, siga estes passos:

   **Webhook acionador**
   1. No **Webhook resposta**, escolha **importação da amostra**. 

   2. No **importação da amostra** página, cole uma resposta de amostra, em seguida, escolha **importação**. Para ver um exemplo de resposta, consulte o [referência da API do GitHub para criar um webhook](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. Em **configuração acionador**, selecione um parâmetro a utilizar para o pedido de criação do webhook. As Logic Apps utiliza este valor de parâmetro para povoar o URL de chamada de retorno utilizado pelo seu serviço para comunicar com o acionador.

   **Acionador da consulta**
   1. Em **resposta**, pode especificar uma resposta de predefinição. 
   Escolha **Add default response** (Adicionar resposta predefinida).

   2. No **importação da amostra** página, cole uma resposta de amostra, em seguida, escolha **importação**.

   3. Em **configuração acionador**, especifique o parâmetro de consulta, o valor para passar para o parâmetro e uma *sugestão de Acionador* que especifica um intervalo de consulta adequada para o pedido seguinte.

5. Por fim, em **validação**, reveja e corrija quaisquer problemas potenciais identificados para o acionador.

#### <a name="review-reference-definitions-for-your-connector"></a>Reveja as definições de referência para o conector

O **referências** secção automaticamente preenche da descrição do seu API e descreve os campos de parâmetro que pode referenciar ações e é acionado. 

1. Em **referências**, escolha o número para a definição de referência que pretende rever.

2. Em **nome**, analisar ou atualizar o nome de definição de referência.

3. Em **validação**, reveja e corrija quaisquer problemas potenciais identificados para a definição de referência.

## <a name="3-finish-creating-your-connector"></a>3. Concluir a criação do conector

Quando estiver pronto, selecione **criar** pelo que pode implementar o conector. Se estiver a atualizar um conetor existente, escolha **atualizar conector**.

Parabéns! Agora, quando criar uma aplicação lógica, pode localizar o conector no Estruturador de Aplicações Lógicas e adicioná-lo à sua aplicação lógica.

![No Estruturador de Aplicações Lógicas, localize o seu conector](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Partilhar o seu conector com outros utilizadores do Logic Apps

Os conectores personalizados registados, mas não certificados, funcionam como os conectores geridos pela Microsoft, mas estão visíveis e disponíveis *apenas* para os respetivos autores e para os utilizadores que têm o mesmo inquilino do Azure Active Directory e a mesma subscrição do Azure para as aplicações lógicas na região onde estas estão implementadas. Embora a partilha seja opcional, poderá deparar-se com cenários em que queira partilhar os seus conectores com outros utilizadores. 

> [!IMPORTANT]
> Se partilhar um conector, as outras pessoas poderão começar a depender do mesmo. 
> ***A eliminação do conector elimina todas as ligações ao mesmo.***
 
Para partilhar o seu conector com utilizadores externos fora estes limites, por exemplo, com as Logic Apps, fluxo e PowerApps todos os utilizadores, [submeter o seu conector para certificação Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>FAQ

**P:** Existem limites para os conectores personalizados? </br>
**R:** Sim, veja os [limites dos conectores personalizados aqui](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Obter suporte

* Para obter suporte com o desenvolvimento e a integração ou para pedir funcionalidades que não estão disponíveis no assistente de registo, contacte [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). A Microsoft monitoriza esta conta quanto a perguntas e problemas com que os programadores se deparam e encaminha-as para a equipa adequada.

* Para fazer ou responder a perguntas ou para ver o que os outros utilizadores do Azure Logic Apps estão a fazer, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Para ajudar a melhorar o Logic Apps, vote ou submeta ideais no [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Passos Seguintes

* [Opcional: Confirme do seu conector](../logic-apps/custom-connector-submit-certification.md)
* [Custom connector FAQ](../logic-apps/custom-connector-faq.md) (FAQ dos conectores personalizados)