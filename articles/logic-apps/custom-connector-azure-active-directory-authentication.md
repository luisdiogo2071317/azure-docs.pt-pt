---
title: Proteger conectores personalizados com o Azure AD - Azure Logic Apps | Microsoft Docs
description: "Adicionar segurança e autenticação à sua API e ao seu conector com o Azure Active Directory (Azure AD)"
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
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: dae76a4230a1cfbe8970bbb1015041fde7765f49
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/26/2018
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Adicionar segurança à sua API e um conector com o Azure Active Directory (Azure AD)

Para proteger as chamadas entre a sua PAI e o seu conector personalizado, adicione a autenticação do Azure AD à API Web e ao conector. Neste cenário, vai criar duas aplicações do Azure Active Directory (Azure AD). Uma aplicação do Azure AD protege a API Web e a outra protege o registo do conector e acrescenta o acesso delegado. 

Este tutorial utiliza a API do Azure Resource Manager como exemplo. O Azure Resource Manager ajuda-o a gerir os componentes de uma solução que criou no Azure, como bases de dados, máquinas virtuais e aplicações Web. Os conectores personalizados para o Azure Resource Manager podem ser úteis se quiser gerir recursos do Azure a partir dos seus fluxos de trabalho. Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, pode começar com uma [conta do Azure gratuita](https://azure.microsoft.com/free/). Caso contrário, inscreva-se [numa subscrição Pay As You Go](https://azure.microsoft.com/pricing/purchase-options/).

* Neste tutorial, o [ficheiro OpenAPI de exemplo para o Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > O ficheiro OpenAPI de exemplo não define todas as operações do Azure Resource Manager e, atualmente, contém apenas a operação para [Listar todas as subscrições](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Pode utilizar o [editor OpenAPI online](http://editor.swagger.io/) para editar esta OpenAPI ou criar outro ficheiro OpenAPI.
  >
  > Pode aplicar este tutorial a qualquer API RESTful em que pretenda utilizar a autenticação do Azure AD.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Criar a sua primeira aplicação do Azure AD para proteger a sua API Web

A primeira aplicação do Azure AD realiza a autenticação quando o conector personalizado chama a API, que é a API do Azure Resource Manager, neste exemplo.

1. Inicie sessão no [portal do Azure](https://portal.azure.com). Se tiver mais de um inquilino do Azure Active Directory, confirme que tem sessão iniciada no diretório correto, verificando o diretório abaixo do seu nome de utilizador. 

   ![Confirmar o diretório](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Para mudar de diretório, escolha o seu nome de utilizador e selecione aquele que pretende.

2. No menu principal do Azure, escolha **Azure Active Directory**, para ver o diretório atual.

   ![Escolher o "Azure Active Directory"](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Se o menu principal do Azure não mostra **do Azure Active Directory**, escolha **todos os serviços**. Na caixa **Filtrar**, escreva "Azure Active Directory" como o filtro e escolha **Azure Active Directory**.

3. No menu do diretório, em **Gerir**, escolha **Registos de aplicações**. Na lista de aplicações registadas, escolha **+ Registo de aplicação nova**.

   ![Escolha "Registos de aplicações", "+ Registo de aplicação nova"](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. Em **Criar**, indique os detalhes da sua aplicação do Azure AD, conforme descrito na tabela, e escolha **Criar**. 

   ![Criar aplicação do Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Definição | Valor sugerido | Descrição | 
   | ------- | --------------- | ----------- | 
   | **Nome** | *nome-da-api-web* | O nome da aplicação do Azure AD da API Web | 
   | **Tipo de Aplicação** | **Aplicação Web / API** | O tipo da aplicação | 
   | **URL de início de sessão** | `https://login.windows.net` | | 
   |||| 

5. Quando regressar à lista **Registos de aplicações** do diretório, selecione a sua aplicação do Azure AD.

   ![Selecione a sua aplicação do Azure AD a partir da lista](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Quando for apresentada a página de detalhes da aplicação, confirme que **copia e guarda o *ID da Aplicação* num local seguro**. Vai precisar deste ID mais tarde.

   ![Guardar o "ID da Aplicação" para utilização posterior](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Agora, indique um URL de resposta para a aplicação do Azure AD. No menu **Definições** da aplicação, escolha **URLs de Resposta**. Introduza este URL e escolha **Guardar**.

   ![URLs de Resposta](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Definição | Valor sugerido | Descrição | 
   | ------- | --------------- | ----------- | 
   | **URLs de Resposta** | Na sua própria API, introduza este URL: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Permissões delegadas** | {não é necessário} | | 
   | **ID de Cliente** | {não é necessário} | | 
   |||| 

   > [!TIP]
   > Se o menu **Definições** não tiver aparecido anteriormente, escolha **Definições** aqui:
   >
   > ![Escolher "Definições"](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Criar a sua segunda aplicação do Azure AD para o conector personalizado

A segunda aplicação do Azure AD protege o registo do conector personalizado e acrescenta o acesso delegado à API Web que a primeira aplicação do Azure AD protege. 

> [!IMPORTANT]
> Certifique-se de que ambas as aplicações existem no mesmo diretório.

1. Regresse à lista **Registos de aplicações** e escolha novamente **+ Registo de aplicação nova**.

   ![Escolha "Registos de aplicações", "+ Registo de aplicação nova"](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. Em **Criar**, indique os detalhes da segunda aplicação do Azure AD, conforme descrito na tabela, e escolha **Criar**. 

   ![Criar aplicação do Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Definição | Valor sugerido | Descrição | 
   | ------- | --------------- | ----------- | 
   | **Nome** | *nome-do-seu-conector* | O nome da aplicação do Azure AD do conector | 
   | **Tipo de Aplicação** | **Aplicação Web / API** | O tipo da aplicação | 
   | **URL de início de sessão** | `https://login.windows.net` | | 
   |||| 

3. Quando regressar à lista **Registos de aplicações** do diretório, selecione a sua segunda aplicação do Azure AD.

   ![Selecione a segunda aplicação do Azure AD a partir da lista](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Quando for apresentada a página de detalhes da aplicação, confirme que também **copia e guarda o *ID da Aplicação* num local seguro**. Vai precisar deste ID mais tarde.

   ![Guardar o "ID da Aplicação" para utilização posterior](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Agora, indique um URL de resposta para a aplicação do Azure AD. No menu **Definições** da aplicação, escolha **URLs de Resposta**. Introduza este URL e escolha **Guardar**.

   | Definição | Valor sugerido | 
   | ------- | --------------- | 
   | **URLs de Resposta** | No conector personalizado do Azure Resource Manager, introduza este URL: `https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Se o menu **Definições** não tiver aparecido anteriormente, escolha **Definições** aqui:
   >
   > ![Escolher "Definições"](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. De novo no menu **Definições**, escolha **Permissões necessárias** > **Adicionar**.

   ![Permissões obrigatórias > Adicionar](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Quando se abrir o menu **Adicionar o acesso à API**, escolha **Selecionar uma API** > 
**API de Gestão do Serviço Microsoft Azure** > **Selecionar**.

   ![Selecionar uma API](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. No menu **Adicionar acesso à API**, escolha **Selecionar permissões**. Em **Permissões delegadas**, escolha **Aceder à Gestão do Serviço do Azure como utilizadores da organização** > **Selecionar**.

   ![Escolha "Permissões delegadas" > "Aceder à Gestão do Serviço do Azure como utilizadores da organização”](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Caso contrário, para outras opções:

   | Opção | Valor sugerido | Descrição | 
   | ------ | --------------- | ----------- | 
   | **Permissões delegadas** | | Selecione as permissões para acesso delegado à sua API Web | 
   |||| 

9. Agora, no menu **Adicionar acesso à API**, escolha **Concluído**.

   ![Menu "Adicionar acesso à API" > "Concluído"](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Agora, gere uma *chave de cliente*, ou “segredo”, para a aplicação do Azure AD do conector. 

    1. Novamente no menu **Definições**, escolha **Chaves**. 
    Indique um nome para a chave com 16 carateres ou menos, selecione um período de validade e escolha **Guardar**.

       ![Criar uma ligação de cliente](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Quando for apresentada a sua chave gerada, **confirme que a copia e guarda num local seguro** antes de sair da página **Chaves**.
    
       ![Copiar e guardar a chave manualmente](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Depois de guardar a chave, pode fechar o menu **Definições** com segurança.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Adicionar a autenticação do Azure AD à sua aplicação API Web

Agora, ative a autenticação na API Web com a primeira aplicação do Azure AD. Para obter mais informações, saiba [como configurar a sua aplicação de serviço de aplicações para utilizar o início de sessão do Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. No [portal do Azure](https://portal.azure.com), localizar a aplicação de Web API que publicou anteriormente no [criar conectores personalizados a partir de APIs da Web](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. Em **Definições**, escolha **Autenticação/Autorização**. 

   1. Em **Autenticação do Serviço de Aplicações**, escolha **Ativar**.
   2. Em **Ação a tomar quando o pedido não é autenticado**, selecione **Iniciar sessão com o Azure Active Directory**.
   3. Em **Fornecedores de Autenticação**, selecione **Azure Active Directory**. 

   ![Escolha “Autenticação/Autorização”](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Na página **Definições do Azure Active Directory**:

   1. Em **Modo de gestão**, escolha **Expresso**.

   2. Agora, selecione a aplicação do Azure AD que a aplicação API Web utiliza para autenticação. 
   Escolha **Selecionar Aplicação do AD Existente** > **Aplicação do Azure AD**.

   3. Em **Aplicações do Azure AD**, selecione a aplicação do Azure AD que criou anteriormente para a aplicação API Web. 
   
   4. Escolha **OK** até regressar à página **Autenticação/Autorização**.

   Por exemplo:

   ![Escolha a aplicação do Azure AD que representa a sua aplicação API Web](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Na página **Autenticação/Autorização**, escolha **Guardar**.

   ![Guardar definições da autenticação](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Agora, a aplicação API Web pode utilizar o Azure AD para autenticação.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Configurar a autenticação do Azure AD no ficheiro OpenAPI da sua API Web

Abra o ficheiro OpenAPI da aplicação API Web, para que possa adicionar o objeto `securityDefintions` e a autenticação do Azure AD na propriedade `host`. Eis um exemplo que mostra a propriedade `host` e o objeto `securityDefinitions`:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
Está agora pronto para criar e registar o seu conector personalizado.

## <a name="next-steps"></a>Passos Seguintes

* [Registar os seus conectores](../logic-apps/logic-apps-custom-connector-register.md)
* [Custom connector FAQ](../logic-apps/custom-connector-faq.md) (FAQ dos conectores personalizados)
