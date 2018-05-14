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
ms.openlocfilehash: 43a4db9114cd47da5bef98ed634847b547589b47
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
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

5. Assim que a aplicação é adicionada, aceda a **propriedades** página e copie o **ID de objeto**.

<!-- ![Properties Page](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png) Note: Image is missing. -->

6. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    a. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.

    b. Tem de ter permissões suficientes para criar as funções. Clique em **modificar permissões** para obter as permissões necessárias. 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Selecione os seguintes permissões de lista (se ainda não tiver estes) e clique em "Modificar permissões" 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Isto irá pedir-lhe para iniciar sessão novamente e aceitar o consentimento do utilizador. Após aceitar o consentimento, são registadas no sistema novamente.

    e. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Extrair a propriedade appRoles do objeto principal do serviço. 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

    > [!Note]
    > Se estiver a utilizar a aplicação (não galeria) personalizada, consulte as funções predefinidos - msiam_access e de utilizador. Em caso de aplicação de galeria, msiam_access é a única função de predefinição. Não é necessário efetuar quaisquer alterações nas funções de predefinição.

    h. Agora tem de gerar novas funções para a sua aplicação. 

    i. Abaixo JSON é um exemplo de appRoles objeto. Crie um objeto semelhante para adicionar as funções que pretende para a sua aplicação. 

    ```
    {
       "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Administrators Only",
            "displayName": "Admin",
            "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "Administrator"
        }
    ]
    }
    ```
    > [!Note]
    > Só é possível adicionar novas funções depois do **msiam_access** para a operação de patch. Além disso, pode adicionar tantos funções como pretende que por sua necessidade da organização. Azure AD irá enviar o **valor** destas funções como o valor de afirmação na resposta SAML.
    
    j. Volte para o Explorador do gráfico e alterar o método de **obter** para **PATCH**. Aplicar o patch do objeto principal do serviço para ter pretendido funções atualizando appRoles propriedade semelhante às mostrado acima no exemplo. Clique em **executar consulta** para executar a operação de patch. Uma mensagem de êxito confirma que a criação da função.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Depois do principal de serviço é aplicado com várias funções, pode atribuir utilizadores as respetivas funções. Isto pode ser feito ao aceder ao portal e navegar para a respetiva aplicação. Clique em de **utilizadores e grupos** separador na parte superior. Isto apresenta uma lista de todos os utilizadores e grupos que já estão atribuídos à aplicação. Pode adicionar novos utilizadores em nova função e pode também selecionar o utilizador existente e clique em **editar** para alterar a função.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

     Para atribuir a função a qualquer utilizador, selecione a nova função e clique em **atribuir** botão na parte inferior da página.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Tenha em atenção que tem de atualizar a sua sessão no portal do Azure para ver as novas funções.

8. Depois de atribuir funções aos utilizadores, é necessário atualizar **atributos** tabela para definir o mapeamento personalizado do **função** de afirmação.

9. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | -------------- | ----------------|    
    | Nome da Função      | user.assignedrole |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Configurar o atributo de início de sessão único](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo conforme necessário. Neste exemplo tem utilizámos **nome da função** como nome de afirmação.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

10. Para testar a aplicação no IDP iniciada início de sessão único no registo para o painel de acesso (https://myapps.microsoft.com) e clique no mosaico aplicações. No SAML token, deverá ver todas as funções atribuídas para o utilizador com o nome de afirmação que atribuiu.

## <a name="update-existing-role"></a>Atualizar função existente

Para atualizar uma função existente, execute os seguintes passos-

1. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer).

2. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.
    
3. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrair a propriedade appRoles do objeto principal do serviço.
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Para atualizar a função existente,. Siga os passos abaixo:

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Alterar o método de **obter** para **PATCH**.

    * Copiar as funções de existentes e cole-os para o **corpo do pedido**.

    * Atualizar para atualizar o valor da função de **descrição da função**, **valor função** ou **função displayname** conforme necessário.

    * Depois de atualizar todas as funções necessárias, clique em **executar consulta**.
        
## <a name="delete-existing-role"></a>Eliminar função existente

Para eliminar uma função existente, execute os seguintes passos:

1. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

2. Inicie sessão site do Explorador de gráfico com as credenciais de administrador/coadministrador global para o seu inquilino.

3. Alterar a versão para **beta** e obter a lista de principais de serviço do seu inquilino utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Se estiver a utilizar vários diretórios, deve seguir este padrão `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Procure o **id de objeto**, que copiou a partir da página de propriedades e utilize o seguinte consulta para obter para o Principal de serviço correspondentes.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrair a propriedade appRoles do objeto principal do serviço.
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Para eliminar a função existente,. Siga os passos abaixo:

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    * Alterar o método de **obter** para **PATCH**.

    * Copiar as funções existentes da aplicação e cole-os no **corpo do pedido**.
        
    * Definir o **IsEnabled** valor **falso** para a função, o que pretende eliminar

    * Clique em **executar consulta**.
    
    > [!NOTE] 
    > Certifique-se de que tem **msiam_access** função de utilizador e o id de correspondência na função de gerado.
    
7. Depois da função estiver desativada, elimine o bloco função da secção appRoles, mantenha o método como **PATCH** e clique em **executar consulta**.
    
8. Depois de executar a consulta será eliminada a função.
    
    > [!NOTE]
    > A função tem de ser desativada primeiro para que possa ser removido. 

## <a name="next-steps"></a>Próximos Passos

Consulte [documentação da aplicação ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) para os passos adicionais.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
