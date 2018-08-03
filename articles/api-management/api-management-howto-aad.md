---
title: Autorizar contas de programador com o Azure Active Directory - gestão de API do Azure | Documentos da Microsoft
description: Saiba como autorizar os utilizadores com o Azure Active Directory na gestão de API.
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
ms.openlocfilehash: 6c288e4492ac56436d40d1e3db98af8eb7b173c8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436323"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Autorizar contas de programador com o Azure Active Directory na gestão de API do Azure

Este artigo mostra-lhe como permitir o acesso ao portal do programador para os utilizadores do Azure Active Directory (Azure AD). Este guia também mostra como gerir grupos de utilizadores do Azure AD através da adição de grupos externos que contenham os utilizadores.

> [!NOTE]
> Integração do Azure AD está disponível na [programador, Standard e Premium](https://azure.microsoft.com/pricing/details/api-management/) camadas apenas.

## <a name="prerequisites"></a>Pré-requisitos

- Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
- Importar e publicar uma instância de gestão de API do Azure. Para obter mais informações, consulte [importar e publicar](import-and-publish.md).

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Autorizar contas de programador através do Azure AD

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
1. Selecione ![seta](./media/api-management-howto-aad/arrow.png).
1. Tipo **api** na caixa de pesquisa.
1. Selecione **dos serviços de gestão de API**.
1. Selecionar a sua instância de serviço de Gestão de API.
1. Sob **SECURITY**, selecione **identidades**.

1. Selecione **+ adicionar** da parte superior.

    O **fornecedor de identidade de adicionar** é apresentado o painel à direita.
1. Sob **tipo de fornecedor**, selecione **Azure Active Directory**.

    Controles que permitem que insira outras informações necessárias são exibidos no painel. Os controles incluem **ID de cliente** e **segredo do cliente**. (Obtenha informações sobre esses controles posteriormente neste artigo.)
1. Tome nota do conteúdo do **URL de redirecionamento**.
    
   ![Passos para adicionar um fornecedor de identidade no portal do Azure](./media/api-management-howto-aad/api-management-with-aad001.png)  
1. No seu browser, abra um separador de diferente. 
1. Aceda ao [Portal do Azure](https://portal.azure.com).
1. Selecione ![seta](./media/api-management-howto-aad/arrow.png).
1. Tipo **Active Directory**. O **do Azure Active Directory** é apresentado o painel.
1. Selecione **Azure Active Directory**.
1. Sob **MANAGE**, selecione **registos das aplicações**.
1. Selecione **Novo registo de aplicação**.

    ![Seleções para a criação de um novo registo de aplicação](./media/api-management-howto-aad/api-management-with-aad002.png)

    O **criar** é apresentado o painel à direita. Que é onde introduz as informações relevantes para a aplicação do Azure AD.
1. Introduza um nome para a aplicação.
1. Para o tipo de aplicação, selecione **aplicação/API Web**.
1. Para o URL de início de sessão, introduza o URL de início de sessão do portal do programador. Neste exemplo, o URL de início de sessão é https://apimwithaad.portal.azure-api.net/signin.
1. Selecione **criar** para criar o aplicativo.
1. Para localizar a sua aplicação, selecione **registos das aplicações** e procurar por nome.

    ![Caixa de onde procurar uma aplicação](./media/api-management-howto-aad/find-your-app.png)
1. Depois da aplicação fica registada, aceda ao **URL de resposta** e certifique-se **URL de redirecionamento** está definido como o valor que obteve no passo 9. 
1. Se quiser configurar a sua aplicação (por exemplo, altere **URL de ID de aplicação**), selecione **propriedades**.

    ![Abrir o painel "Propriedades"](./media/api-management-howto-aad/api-management-with-aad004.png)

    Se várias instâncias do Azure AD serão utilizadas para esta aplicação, selecione **Sim** para **vários inquilinos**. A predefinição é **não**.
1. Definir permissões de aplicação, selecionando **permissões obrigatórias**.
1. Selecione a aplicação e, em seguida, selecione o **ler dados do diretório** e **iniciar sessão e ler o perfil de utilizador** caixas de verificação.

    ![Caixas de verificação de permissões](./media/api-management-howto-aad/api-management-with-aad005.png)

    Para obter mais informações sobre as permissões de aplicação e permissões delegadas, consulte [aceder à API do Graph][Accessing the Graph API].
1. No painel esquerdo, copie os **ID da aplicação** valor.

    ![Valor de "ID da aplicação"](./media/api-management-howto-aad/application-id.png)
1. Mude para a sua aplicação de gestão de API. 

    No **Adicionar fornecedor de identidade** janela, colar a **ID de aplicação** valor no **ID de cliente** caixa.
1. Volte para a configuração do Azure AD e selecione **chaves**.
1. Crie uma nova chave especificando um nome e uma duração. 
1. Selecione **Guardar**. A chave é gerada.

    Copie a chave para a área de transferência.

    ![Seleções para a criação de uma chave](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Tome nota desta chave. Depois de fechar o painel de configuração do Azure AD, a chave não é possível apresentar novamente.
    > 
    > 
1. Mude para a sua aplicação de gestão de API. 

    Na **Adicionar fornecedor de identidade** janela, cole a chave no **segredo do cliente** caixa de texto.

    > [!IMPORTANT]
    > Certifique-se atualizar o **segredo do cliente** antes da chave expirar. 
    >  
    >
1. O **Adicionar fornecedor de identidade** janela também contém o **inquilinos permitidos** caixa de texto. Lá, especifique os domínios das instâncias do Azure AD aos quais pretende conceder acesso às APIs de instância de serviço de gestão de API. É possível separar vários domínios com vírgulas, espaços ou garantidamente.

    É possível especificar vários domínios no **inquilinos permitidos** secção. Antes de qualquer utilizador pode iniciar sessão a partir de um domínio diferente daquele domínio original em que a aplicação foi registrada, um administrador global do domínio diferente tem de conceder permissão para a aplicação para aceder a dados do diretório. Para conceder permissão, o administrador global deve:
    
    a. Aceda a `https://<URL of your developer portal>/aadadminconsent` (por exemplo, https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Escreva o nome de inquilino do Azure AD que pretende dar acesso ao domínio.
    
    c. Selecione **submeter**. 
    
    No exemplo a seguir, um administrador global de miaoaad.onmicrosoft.com está a tentar dar permissão para este portal de programador específico. 

1. Depois de especificar a configuração pretendida, selecione **adicionar**.

    !["Adicionar" botão no painel de "Add identity provider"](./media/api-management-howto-aad/api-management-with-aad007.png)

Depois das alterações são guardadas, os utilizadores no Azure AD especificado instância pode iniciar sessão no portal do programador ao seguir os passos em [inicie sessão no portal do programador, utilizando uma conta do Azure AD](#log_in_to_dev_portal).

![Introduzir o nome de inquilino do Azure AD](./media/api-management-howto-aad/api-management-aad-consent.png)

No ecrã seguinte, o administrador global é-lhe pedido para confirmar a conceder a permissão. 

![Confirmação de atribuição de permissões](./media/api-management-howto-aad/api-management-permissions-form.png)

Se um administrador não global tentar iniciar sessão antes de um administrador global concede permissões, a tentativa de início de sessão falha e é apresentado um ecrã de erro.

## <a name="add-an-external-azure-ad-group"></a>Adicionar um externo grupo do Azure AD

Depois de ativar o acesso de utilizadores numa instância do Azure AD, é possível adicionar grupos do Azure AD na gestão de API. Em seguida, pode gerir mais facilmente a associação de desenvolvedores no grupo de produtos desejados.

Para configurar um externo grupo do Azure AD, primeiro tem de configurar a instância do Azure AD no **identidades** separador, seguindo o procedimento na secção anterior. 

Adicionar o Azure externo de grupos do AD a **grupos** separador da sua instância de gestão de API.

1. Selecione o separador **Grupos**.
1. Selecione o **grupo do AAD adicionar** botão.
   ![Botão "Adicionar grupo do AAD"](./media/api-management-howto-aad/api-management-with-aad008.png)
1. Selecione o grupo que pretende adicionar.
1. Prima a **selecione** botão.

Depois de adicionar um Azure AD externo grupo, pode rever e configurar as respetivas propriedades. Selecione o nome do grupo a partir da **grupos** separador. A partir daqui, pode editar **Name** e **Descrição** informações para o grupo.
 
Os utilizadores configurada de instância do Azure AD agora pode iniciar sessão no portal do programador. Podem ver e subscrever todos os grupos para os quais têm visibilidade.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Inicie sessão no portal do programador, utilizando uma conta do Azure AD

Para iniciar sessão no portal do programador, utilizando uma conta do Azure AD que configurou nas secções anteriores:

1. Abra uma nova janela do browser, utilizando o URL de início de sessão da aplicação de configuração do Active Directory e selecione **do Azure Active Directory**.

   ![Página de início de sessão][api-management-dev-portal-signin]

1. Introduza as credenciais de um dos utilizadores no Azure AD e selecione **iniciar sessão**.

   ![Iniciar sessão com o nome de utilizador e palavra-passe][api-management-aad-signin]

1. Pode ser pedido com um formulário de Registro se qualquer informação adicional é necessária. Preencha o formulário de registro e selecione **Inscreva-se**.

   ![Botão "Inscrever-se" no formulário de registro][api-management-complete-registration]

O utilizador tem sessão iniciada agora no portal do programador para a sua instância do serviço de gestão de API.

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
