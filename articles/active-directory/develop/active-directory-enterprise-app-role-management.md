---
title: Configurar afirmação de função emitida no token SAML para aplicações da empresa no Azure Active Directory | Microsoft Docs
description: Saiba como configurar a afirmação de função emitida no token SAML para aplicações da empresa no Azure Active Directory
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 88a9f5988d1fe3f4de4fe10da23a5f713e3f3370
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="configuring-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Configurar a afirmação de função emitida no token SAML para aplicações da empresa no Azure Active Directory

Esta funcionalidade permite aos utilizadores personalizar o tipo de afirmação para afirmação 'Funções' no token de resposta recebida após autorizar uma aplicação utilizar o Azure AD.

## <a name="prerequisites"></a>Pré-requisitos
- Uma subscrição do Azure AD com a configuração de diretório
- Um único início de sessão ativada subscrição
- É necessário configurar o SSO com a sua aplicação

## <a name="when-to-use-this-feature"></a>Quando utilizar esta funcionalidade

Se a aplicação espera funções personalizadas a transmitir em resposta SAML, terá de utilizar esta funcionalidade. Esta funcionalidade permite-lhe criar tantas funções como tem de ser transmitidos atrás do Azure AD para a aplicação.

## <a name="steps-to-use-this-feature"></a>Passos para utilizar esta funcionalidade

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva o nome da sua aplicação, selecione a aplicação a partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Aplicação na lista de resultados](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Assim que a aplicação é adicionada, aceda a **propriedades** página e copie o **ID de objeto**

    ![Página de propriedades](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    a. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.

    b. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    c. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    d. Extrair a propriedade appRoles do objeto principal do serviço.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    e. Tem de gerar novas funções para a sua aplicação agora. Pode transferir a função do Azure AD gerador de [aqui](https://app.box.com/s/jw6m9p9ehmf4ut5jx8xhcw87cu09ml3y).

    f. Abra o gerador de AD do Azure e execute os seguintes passos-

    ![Gerador do Azure AD](./media/active-directory-enterprise-app-role-management/azure_ad_role_generator.png)
    
    Introduza **nome da função**, **descrição da função**, e **função valor**. Clique em **adicionar** para adicionar a função
    
    Depois de ter adicionado todas as funções necessárias, clique em **gerar**
    
    Copiar o conteúdo ao clicar em **conteúdo de cópia**

    > [!NOTE] 
    > Certifique-se de que tem **msiam_access** função de utilizador e o id de correspondência na função de gerado.

    g. Volte para o Explorador do gráfico. Alterar o método de **obter** para **PATCH**. Aplicar o patch do objeto principal do serviço para ter pretendido appRoles atualizando propriedade appRoles com os valores copiados. Clique em **executar consulta**.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-patch.png)

    > [!NOTE]
    > Segue-se um exemplo de appRoles objeto. 
    > 
    >   ```
    > {
    > "appRoles": [
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "msiam_access",
    >   "displayName": "msiam_access",
    >   "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
    >   "isEnabled": true,
    >   "origin": "Application",
    >   "value": null
    > },
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "teacher",
    >   "displayName": "teacher",
    >   "id": "6478ffd2-5dbd-4584-b2ce-137390b09b60",
    >   "isEnabled": ,
    >   "origin": "ServicePrincipal",
    >   "value": "teacher"
    > }
    > ] 
    > }
    >
    >   ```

7. Depois do principal de serviço é aplicado com várias funções, iremos pode atribuir utilizadores as respetivas funções. Isto pode ser feito ao aceder ao Portal e navegar para a respetiva aplicação. Em seguida, clicar no **utilizadores e grupos** separador na parte superior. Este processo irá listar todos os utilizadores ou grupos.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/userrole.png)

    a. Para atribuir uma função a qualquer utilizador, basta selecionar utilizador/grupo específico e clique em **atribuir** botão na parte inferior da página.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/userandgroups.png)

    b. Clicar em que oferece um pop-up para selecionar uma função de diferentes funções que estão definidas para o principal de serviço correspondentes.

    c. Escolha a função necessária e clique em submeter.

8. Depois de atribuir funções aos utilizadores, é necessário atualizar **atributos** tabela para definir o mapeamento personalizado do **função** de afirmação.

9. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | -------------- | ----------------|    
    | Nome da Função      | user.assignedrole |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Configurar o atributo de início de sessão único](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

10. Para testar a aplicação no IDP iniciada início de sessão único no registo para o painel de acesso (https://myapps.microsoft.com) e clique no mosaico aplicações. No SAML token, deve ver todas as funções atribuídas para o utilizador com o nome de afirmação que atribuiu.

## <a name="update-existing-role"></a>Atualizar função existente

1. Para atualizar uma função existente, execute os seguintes passos-

    a. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    b. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.
    
    c. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
        
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. Extrair a propriedade appRoles do objeto principal do serviço.
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)
    
    f. Para atualizar a função existente,. Siga os passos abaixo:

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Alterar o método de **obter** para **PATCH**.

    * Copiar as funções existentes da aplicação e cole-os para o **corpo do pedido**.
    
    * Atualize o valor da função, substituindo o **descrição da função**, **valor função**, e **função displayname** de acordo com os requisitos da organização.
    
    * Depois de atualizar todas as funções necessárias, clique em **executar consulta**.
        
## <a name="delete-existing-role"></a>Eliminar função existente

1. Para eliminar uma função existente, execute os seguintes passos-

    a. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    b. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.

    c. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. Extrair a propriedade appRoles do objeto principal do serviço.
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    f. Para eliminar a função existente,. Siga os passos abaixo:

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-patchdelete.png)

    Alterar o método de **obter** para **PATCH**.

    Copiar as funções existentes da aplicação e cole-os no **corpo do pedido**.
    
    Definir o **IsEnabled** valor **falso** para a função que pretende eliminar

    Clique em **executar consulta**.
    
    > [!NOTE] 
    > Certifique-se de que tem **msiam_access** função de utilizador e o id de correspondência na função de gerado.
    
    g. Depois de efetuar o processo de acima, mantenha o método como **PATCH** e cole a função de remianing conteúda a **corpo do pedido** e clique em **executar consulta**.
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-patchfinal.png)

    h. Depois de executar a consulta será eliminada a função.
    
    > [!NOTE]
    > A função tem de ser desativada primeiro para que possa ser removido. 

## <a name="next-steps"></a>Próximos Passos

Consulte [documentação da aplicação ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-tutorial-list) para os passos adicionais.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
