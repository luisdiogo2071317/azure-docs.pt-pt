---
title: Autorizar a contas de programador com o Azure Active Directory - API Management do Azure | Microsoft Docs
description: Saiba como autorizar os utilizadores com o Azure Active Directory na API Management.
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: c9d99d6f7c2777c8e68f5db50b402280377d88e9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Como autorizar contas de programador com o Azure Active Directory na API Management do Azure

Este guia mostra como ativar o acesso ao portal do programador para os utilizadores do Azure Active Directory. Este guia mostra também como gerir grupos de utilizadores do Active Directory do Azure através da adição de grupos externos que contenham os utilizadores de um Azure Active Directory.

> [!WARNING]
> Integração do Active Directory do Azure está disponível no [programador, Standard e Premium](https://azure.microsoft.com/pricing/details/api-management/) camadas apenas.

## <a name="prerequisites"></a>Pré-requisitos

- Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
- Importar e publicar uma instância de API Management. Para obter mais informações, consulte [importação e publicar](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Como autorizar contas de programador com o Azure Active Directory

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Selecione ![seta](./media/api-management-howto-aad/arrow.png).
3. Escreva "api" na caixa de pesquisa.
4. Clique em **dos serviços de gestão de API**.
5. Selecione a instância de serviço APIM.
6. Em **segurança**, selecione **identidades**.

    ![Identidades externas](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Clique em **+ adicionar** do nível superior.

    O **Adicionar fornecedor de identidade** janela aparece à direita.
8. Em **tipo de fornecedor**, selecione **do Azure Active Directory**.

    Controlos que permitem-lhe para introduzir outras informações necessárias que são apresentados na janela. Os controlos incluem: **ID de cliente**, **segredo do cliente** (pode obter informações mais tarde no tutorial).
9. Anote o **URL de redirecionamento**.  
10. No seu browser, abra um separador de diferentes. 
11. Abre o [Portal do Azure](https://portal.azure.com).
12. Selecione ![seta](./media/api-management-howto-aad/arrow.png).
13. O tipo "ativo", o **do Azure Active Directory** aparece.
14. Selecione **do Azure Active Directory**.
15. Em **GERIR**, selecione **registo de aplicação**.

    ![Registo de aplicações](./media/api-management-howto-aad/api-management-with-aad002.png)
16. Clique em **Novo registo de aplicação**.

    O **criar** janela aparece à direita. É onde introduziu as informações de relavent de aplicação do AAD.
17. Eneter um nome para a aplicação.
18. Para o tipo de aplicação, selecione **aplicação/API Web**.
19. Para o URL de início de sessão, introduza o URL de início de sessão do portal do programador. Neste exemplo, o URL de início de sessão é: https://apimwithaad.portal.azure-api.net/signin.
20. Clique em **criar** ao criar a aplicação.
21. Para localizar a aplicação, selecione **registos de aplicação** e procurar por nome.

    ![Registo de aplicações](./media/api-management-howto-aad/find-your-app.png)
22. Depois da aplicação está registada, aceda ao **URL de resposta** e certifique-se a "URL de redirecionamento" está definido para o valor obtido no passo 9. 
23. Se pretender configurar a sua aplicação (por exemplo, altere **URL de ID de aplicação**) selecione **propriedades**.

    ![Registo de aplicações](./media/api-management-howto-aad/api-management-with-aad004.png)

    Se vários diretórios de Active Directory do Azure vai ser utilizado para esta aplicação, clique em **Sim** para **aplicação é a multi-inquilino**. A predefinição é **não**.
24. Definir permissões de aplicação, selecionando **as permissões necessárias**.
25. Selecione a sua aplicação e verificação **ler os dados de diretório** e **iniciar sessão e ler o perfil de utilizador**.

    ![Registo de aplicações](./media/api-management-howto-aad/api-management-with-aad005.png)

    Para obter mais informações sobre a aplicação e as permissões delegadas, consulte [aceder a Graph API][Accessing the Graph API].
26. Na janela da esquerda, copie o **ID da aplicação** valor.

    ![Registo de aplicações](./media/api-management-howto-aad/application-id.png)
27. Mudar para a sua aplicação de API Management. O **Adicionar fornecedor de identidade** deverá ser apresentada a janela. <br/>Colar o **ID da aplicação** valor o **Id de cliente** caixa.
28. Mudar para a configuração do Azure Active Directory e clique em **chaves**.
29. Crie uma nova chave por speicifying um nome e uma duração. 
30. Clique em **Guardar**. A chave obtém gerada.

    Copie a chave para a área de transferência.

    ![Registo de aplicações](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Tome nota desta chave. Depois de fechar a janela de configuração do Azure Active Directory, a chave não pode ser apresentada.
    > 
    > 
31. Mudar para a sua aplicação de API Management. <br/>No **Adicionar fornecedor de identidade** janela, cole a chave para o **segredo do cliente** caixa de texto.
32. O **Adicionar fornecedor de identidade** janela, contém o **permitido inquilinos** caixa de texto, em especifique os diretórios têm acesso a APIs da instância do serviço de API Management. <br/>Especifique os domínios das instâncias do Azure Active Directory ao qual pretende conceder acesso. Pode separar vários domínios com newlines, espaços ou vírgulas.

    Podem ser especificados vários domínios a **permitido inquilinos** secção. Antes de qualquer utilizador pode iniciar sessão a partir de um domínio diferente do domínio original em que a aplicação foi registada, um administrador global do outro domínio tem de conceder permissão para a aplicação para aceder a dados de diretório. Para conceder permissão, o administrador global deve passar para `https://<URL of your developer portal>/aadadminconsent` (por exemplo, https://contoso.portal.azure-api.net/aadadminconsent), introduza o nome de domínio de inquilino do Active Directory que pretendem conceder acesso e clique em submeter. No exemplo seguinte, um administrador global de `miaoaad.onmicrosoft.com` está a tentar dar permissão para este portal para programadores específico. 

33. Depois da configuração pretendida for especificada, clique em **adicionar**.

    ![Registo de aplicações](./media/api-management-howto-aad/api-management-with-aad007.png)

Depois das alterações são guardadas, os utilizadores especificados do Azure Active Directory podem iniciar sessão no portal do programador, seguindo os passos no [inicie sessão no portal do programador, utilizando uma conta do Azure Active Directory](#log_in_to_dev_portal).

![Permissões](./media/api-management-howto-aad/api-management-aad-consent.png)

No ecrã seguinte, será solicitado para confirmar a conceder a permissão de administrador global. 

![Permissões](./media/api-management-howto-aad/api-management-permissions-form.png)

Se tentar um não-administrador global iniciar sessão antes de são concedidas permissões por um administrador global, a tentativa de início de sessão falha e é apresentado um ecrã de erro.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Como adicionar um grupo externo para o Azure Active Directory

Depois de ativar o acesso aos utilizadores do Azure Active Directory, pode adicionar grupos do Azure Active Directory para gestão de API para gerir mais facilmente a associação de programadores do grupo de produtos pretendidos.

Para configurar um grupo externo do Azure Active Directory, o Azure Active Directory tem primeiro de ser configurado no separador de identidades, seguindo o procedimento na secção anterior. 

Grupos externos do Azure Active Directory são adicionados a partir de **grupos** separador da sua instância da API Management.

![Grupos](./media/api-management-howto-aad/api-management-with-aad008.png)

1. Selecione o separador **Grupos**.
2. Clique em de **grupo AAD adicionar** botão.
3. Selecione o grupo que pretende adicionar.
4. Prima **selecione** botão.

Assim que tiver sido criado um grupo do Azure Active Directory, pode rever e configurar as propriedades de grupos externos assim que tiverem sido adicionados, clique no nome do grupo do **grupos** separador.

Aqui pode editar o **nome** e **Descrição** do grupo.
 
Os utilizadores do configurado do Azure Active Directory podem iniciar sessão para o portal do programador e ver e subscrever a quaisquer grupos para o qual têm visibilidade ao seguir as instruções na secção seguinte.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>Como iniciar sessão portal do programador com uma conta do Azure Active Directory

Para iniciar sessão no portal do programador, utilizando uma conta do Azure Active Directory configurada nas secções anteriores, abra uma nova utilizando de janela de browser de **URL de início de sessão** na configuração da aplicação do Active Directory e clique em **do Azure Active Directory**.

![Portal do Programador][api-management-dev-portal-signin]

Introduza as credenciais de um dos utilizadores no Azure Active Directory e clique em **sessão**.

![Iniciar sessão][api-management-aad-signin]

Poderá ser pedido com um formulário de registo se são necessárias quaisquer informações adicionais. Preencha o formulário de registo e clique em **inscrever**.

![Registo][api-management-complete-registration]

Agora, o utilizador é registado no portal do programador para a instância de serviço de API Management.

![Registo Concluído][api-management-registration-complete]

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

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
