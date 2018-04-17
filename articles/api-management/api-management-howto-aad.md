---
title: Autorizar a contas de programador, utilizando o Azure Active Directory - API Management do Azure | Microsoft Docs
description: Saiba como autorizar os utilizadores ao utilizar o Azure Active Directory na API Management.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: 9f3669d205ab4bd24ccba53ffb532fe1d88131ac
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Autorizar a contas de programador na API Management do Azure utilizando o Azure Active Directory

Este artigo mostra como ativar o acesso ao portal do programador para os utilizadores do Azure Active Directory (Azure AD). Este guia mostra também como gerir grupos de utilizadores do Azure AD através da adição de grupos externos que contenham os utilizadores.

> [!NOTE]
> Integração do AD do Azure está disponível no [programador, Standard e Premium](https://azure.microsoft.com/pricing/details/api-management/) camadas apenas.

## <a name="prerequisites"></a>Pré-requisitos

- Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
- Importar e publicar uma instância de API Management do Azure. Para obter mais informações, consulte [importação e publicar](import-and-publish.md).

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Autorizar a contas de programador por utilizar o Azure AD

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Seleccionar ![seta](./media/api-management-howto-aad/arrow.png).
3. Tipo **api** na caixa de pesquisa.
4. Selecione **dos serviços de gestão de API**.
5. Selecione a instância de serviço de API Management.
6. Em **segurança**, selecione **identidades**.

7. Selecione **+ adicionar** do nível superior.

    O **Adicionar fornecedor de identidade** painel aparece à direita.
8. Em **tipo de fornecedor**, selecione **do Azure Active Directory**.

    Controlos que permitem-lhe para introduzir outras informações necessárias que aparecem no painel. Os controlos incluem **ID de cliente** e **segredo do cliente**. (Receberá informações sobre estes controlos mais tarde no artigo).
9. Tome nota do conteúdo do **URL de redirecionamento**.
    
   ![Passos para adicionar um fornecedor de identidade no portal do Azure](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. No seu browser, abra um separador de diferentes. 
11. Aceda ao [Portal do Azure](https://portal.azure.com).
12. Seleccionar ![seta](./media/api-management-howto-aad/arrow.png).
13. Tipo **Active Directory**. O **do Azure Active Directory** painel aparece.
14. Selecione **Azure Active Directory**.
15. Em **GERIR**, selecione **registos de aplicação**.
16. Selecione **Novo registo de aplicação**.

    ![Selecione as opções para criar um novo registo de aplicação](./media/api-management-howto-aad/api-management-with-aad002.png)

    O **criar** painel aparece à direita. É onde introduziu as informações relevantes para a aplicação do Azure AD.
17. Introduza um nome para a aplicação.
18. Para o tipo de aplicação, selecione **aplicação/API Web**.
19. Para o URL de início de sessão, introduza o URL de início de sessão do portal do programador. Neste exemplo, o URL de início de sessão é https://apimwithaad.portal.azure-api.net/signin.
20. Selecione **criar** ao criar a aplicação.
21. Para localizar a aplicação, selecione **registos de aplicação** e procurar por nome.

    ![Caixa de onde procurar uma aplicação](./media/api-management-howto-aad/find-your-app.png)
22. Depois da aplicação está registada, aceda ao **URL de resposta** e certifique-se **URL de redirecionamento** está definido para o valor que recebeu do passo 9. 
23. Se pretender configurar a sua aplicação (por exemplo, altere **URL de ID de aplicação**), selecione **propriedades**.

    ![Abrir o painel "Propriedades"](./media/api-management-howto-aad/api-management-with-aad004.png)

    Se várias instâncias do Azure AD serão utilizadas para esta aplicação, selecione **Sim** para **tenanted de várias**. A predefinição é **não**.
24. Definir permissões de aplicação, selecionando **as permissões necessárias**.
25. Selecione a aplicação e, em seguida, selecione o **ler os dados de diretório** e **iniciar sessão e ler o perfil de utilizador** caixas de verificação.

    ![Caixas de verificação das permissões](./media/api-management-howto-aad/api-management-with-aad005.png)

    Para obter mais informações sobre as permissões de aplicação e as permissões delegadas, consulte [aceder a Graph API][Accessing the Graph API].
26. No painel esquerdo, copie o **ID da aplicação** valor.

    ![Valor de "ID da aplicação"](./media/api-management-howto-aad/application-id.png)
27. Mudar para a sua aplicação de API Management. 

    No **Adicionar fornecedor de identidade** janela, cole o **ID da aplicação** valor o **ID de cliente** caixa.
28. Mudar para a configuração do Azure AD e selecione **chaves**.
29. Crie uma nova chave, especificando um nome e uma duração. 
30. Selecione **Guardar**. A chave é gerada.

    Copie a chave para a área de transferência.

    ![Seleções para a criação de uma chave](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Tome nota desta chave. Depois de fechar o painel de configuração do Azure AD, a chave não pode ser apresentada.
    > 
    > 
31. Mudar para a sua aplicação de API Management. 

    No **Adicionar fornecedor de identidade** janela, cole a chave no **segredo do cliente** caixa de texto.
32. O **Adicionar fornecedor de identidade** janela também contém a **permitido inquilinos** caixa de texto. Não existe, especifique os domínios das instâncias do Azure AD para o qual pretende conceder acesso a APIs da instância do serviço de API Management. Pode separar vários domínios com newlines, espaços ou vírgulas.

    Pode especificar vários domínios no **permitido inquilinos** secção. Antes de qualquer utilizador pode iniciar sessão a partir de um domínio diferente do domínio original em que a aplicação foi registada, um administrador global do outro domínio tem de conceder permissão para a aplicação para aceder a dados de diretório. Para conceder permissão, o administrador global deve:
    
    a. Aceda a `https://<URL of your developer portal>/aadadminconsent` (por exemplo, https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Escreva o nome de domínio do inquilino do Azure AD que pretende conceder acesso.
    
    c. Selecione **submeter**. 
    
    No exemplo seguinte, um administrador global de miaoaad.onmicrosoft.com está a tentar dar permissão para este portal para programadores específico. 

33. Depois de especificar a configuração pretendida, selecione **adicionar**.

    !["Adicionar" botão no painel "Adicionar fornecedor de identidade"](./media/api-management-howto-aad/api-management-with-aad007.png)

Depois das alterações são guardadas, os utilizadores no Azure AD especificado instância pode iniciar sessão no portal do programador, seguindo os passos no [inicie sessão no portal do programador, utilizando uma conta do Azure AD](#log_in_to_dev_portal).

![Introduzir o nome de um inquilino do Azure AD](./media/api-management-howto-aad/api-management-aad-consent.png)

No ecrã seguinte, o administrador global é-lhe pedido para confirmar a conceder a permissão. 

![Confirmação de atribuição de permissões](./media/api-management-howto-aad/api-management-permissions-form.png)

Se um não-administrador global tenta iniciar sessão antes de um administrador global concede permissões, a tentativa de início de sessão falha e é apresentado um ecrã de erro.

## <a name="add-an-external-azure-ad-group"></a>Adicionar um externo grupo do Azure AD

Depois de ativar o acesso de utilizadores numa instância do Azure AD, pode adicionar grupos do Azure AD na API Management. Em seguida, pode gerir mais facilmente a associação de programadores do grupo de produtos pretendidos.

Para configurar um externo grupo do Azure AD, primeiro tem de configurar a instância do Azure AD no **identidades** separador, seguindo o procedimento na secção anterior. 

Adicionar externo do Azure AD de grupos do **grupos** separador da sua instância da API Management.

1. Selecione o separador **Grupos**.
2. Selecione o **grupo AAD adicionar** botão.
   ![Botão "Adicionar grupo AAD"](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Selecione o grupo que pretende adicionar.
4. Prima a **selecione** botão.

Depois de adicionar um externo do Azure AD grupo, pode rever e configurar as respetivas propriedades. Selecione o nome do grupo do **grupos** separador. Aqui, pode editar **nome** e **Descrição** informações para o grupo.
 
Os utilizadores a partir de instância do Azure AD pode agora iniciar sessão portal do programador. Podem ver e subscrever a quaisquer grupos para o qual têm visibilidade.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Inicie sessão no portal do programador, utilizando uma conta do Azure AD

Para iniciar sessão no portal do programador, utilizando uma conta do Azure AD que configurou nas secções anteriores:

1. Abra uma nova janela do browser, utilizando o URL de início de sessão da configuração de aplicação do Active Directory e selecione **do Azure Active Directory**.

   ![Página de início de sessão][api-management-dev-portal-signin]

2. Introduza as credenciais de um dos utilizadores no Azure AD e selecione **sessão**.

   ![Iniciar sessão com o nome de utilizador e palavra-passe][api-management-aad-signin]

3. Pode ser pedido com um formulário de registo se são necessárias quaisquer informações adicionais. Preencha o formulário de registo e selecione **inscrever**.

   ![Botão de "Inscrição" no formulário de registo][api-management-complete-registration]

O utilizador tem agora sessão iniciada ao portal do programador para a instância de serviço de API Management.

![Portal do programador após a conclusão do registo][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
