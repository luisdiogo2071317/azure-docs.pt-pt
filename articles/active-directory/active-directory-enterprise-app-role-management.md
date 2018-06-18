---
title: Configurar a afirmação de função emitida no token SAML para aplicações da empresa no Azure Active Directory | Microsoft Docs
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
ms.date: 05/30/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 5aa716f91a3155e81ef8dc7c436b4a9a5811238b
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34723257"
---
# <a name="configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Configurar a afirmação de função emitida no token SAML para aplicações da empresa no Azure Active Directory

Ao utilizar o Azure Active Directory (Azure AD), pode personalizar o tipo de afirmação de função de afirmação no token de resposta que recebe depois autorizar uma aplicação.

## <a name="prerequisites"></a>Pré-requisitos
- Uma subscrição do Azure AD com a configuração do diretório.
- Uma subscrição que tenha início de sessão único (SSO) ativado. Tem de configurar o SSO com a sua aplicação.

## <a name="when-to-use-this-feature"></a>Quando utilizar esta funcionalidade

Se a aplicação espera funções personalizadas a transmitir numa resposta SAML, terá de utilizar esta funcionalidade. Pode criar tantas funções como tem de ser transmitidos atrás do Azure AD para a aplicação.

## <a name="create-roles-for-an-application"></a>Criar funções de uma aplicação

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![Ícone do Active Directory do Azure][1]

2. Selecione **aplicações empresariais**. Em seguida, selecione **todas as aplicações**.

    ![Painel de aplicações da empresa][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão no topo da caixa de diálogo.

    !["A nova aplicação" botão][3]

4. Na caixa de pesquisa, escreva o nome da sua aplicação e, em seguida, selecione a aplicação a partir do painel de resultados. Selecione o **adicionar** botão para adicionar a aplicação.

    ![Aplicação na lista de resultados](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Depois da aplicação é adicionada, avance para o **propriedades** página e copie o ID de objeto.

    ![Página de propriedades](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela e siga os passos seguintes:

    a. Iniciar sessão para o site do Explorador de gráfico com as credenciais de administrador ou coadmin global para o seu inquilino.

    b. Necessita de permissões suficientes para criar as funções. Selecione **modificar permissões** para obter as permissões. 

      ![O botão "modificar permissões"](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Selecione as seguintes permissões de lista (se ainda não tiver estes) e selecione **modificar permissões**. 

      ![Lista de permissões e o botão "Modificar permissões"](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Aceite o consentimento do utilizador. Tem sessão iniciada sistema novamente.

    e. Alterar a versão para **beta**e obter a lista de principais de serviço do seu inquilino, utilizando a seguinte consulta:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
      Se estiver a utilizar vários diretórios, siga este padrão: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
      ![Caixa de diálogo de Explorador de gráfico, com a consulta para obtenção de principais de serviço](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Na lista de principais de serviço obtida, obter que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os principais de serviço listado. Procure o ID de objeto que copiou do **propriedades** página e utilize a seguinte consulta para obter o principal de serviço:
    
      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Consulta para obter o principal de serviço tem de modificar](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Extrair o **appRoles** propriedade do objeto principal do serviço. 

      ![Detalhes da propriedade appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Se estiver a utilizar a aplicação personalizada (e não a aplicação do Azure Marketplace), verá duas funções predefinidas: msiam_access e de utilizador. Para a aplicação de Marketplace, msiam_access é a única função de predefinição. Não precisa de efetuar quaisquer alterações nas funções de predefinição.

    h. Gere novas funções para a sua aplicação. 

      O seguinte JSON é um exemplo do **appRoles** objeto. Crie um objeto semelhante para adicionar as funções que pretende para a sua aplicação. 

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
      > Só é possível adicionar novas funções após msiam_access para a operação de patch. Além disso, pode adicionar tantos funções conforme as necessidades da organização. Azure AD irá enviar o valor de uma destas funções como o valor de afirmação na resposta SAML. Para gerar o GUID valores para o ID de novas funções de utilizam as ferramentas do web como [Isto](https://www.guidgenerator.com/)
    
    i. Voltar ao Explorador do gráfico e alterar o método de **obter** para **PATCH**. Aplicar o patch do objeto principal do serviço para que as funções pretendidas, atualizando o **appRoles** propriedade, como o mostrado no exemplo anterior. Selecione **executar consulta** para executar a operação de patch. Uma mensagem de êxito confirma que a criação da função.

      ![Operação de patch com a mensagem de êxito](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Depois do principal de serviço é aplicado com várias funções, pode atribuir utilizadores as respetivas funções. Pode atribuir utilizadores ao aceder ao portal e navegar para a aplicação. Selecione o **utilizadores e grupos** separador. Este separador apresenta uma lista de todos os utilizadores e grupos que já estão atribuídos à aplicação. Pode adicionar novos utilizadores nas funções de novo. Também pode selecionar um utilizador existente e selecione **editar** para alterar a função.

    ![Separador "Utilizadores e grupos"](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Para atribuir a função a qualquer utilizador, selecione a nova função e selecione o **atribuir** botão na parte inferior da página.

    ![Painel "Editar Atribuição" e o painel "Selecionar função"](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Terá de atualizar a sua sessão no portal do Azure para ver as novas funções.

8. Atualização do **atributos** tabela para definir um mapeamento da afirmação de função personalizado.

9. No **atributos de utilizador** secção o **de sessão único-** diálogo caixa, configure o atributo de token SAML, como mostrado na imagem e efetuar os seguintes passos.
    
    | Nome do atributo | Valor do atributo |
    | -------------- | ----------------|    
    | Nome da função      | user.assignedrole |

    a. Selecione **adicionar atributo** para abrir o **adicionar atributo** painel.

      ![Botão "Adicionar o atributo"](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      ![Painel "Adicionar o atributo"](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. No **nome** caixa, escreva o nome de atributo conforme necessário. Este exemplo utiliza **nome da função** como o nome de afirmação.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** caixa em branco.
    
    e. Selecione **Ok**.

10. Para testar a aplicação num início de sessão que é iniciada por um fornecedor de identidade, inicie sessão no [painel de acesso](https://myapps.microsoft.com) e selecione o mosaico da aplicação. No SAML token, deverá ver todas as funções atribuídas para o utilizador com o nome de afirmação que atribuiu.

## <a name="update-an-existing-role"></a>Atualizar uma função existente

Para atualizar uma função existente, execute os seguintes passos:

1. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer).

2. Iniciar sessão para o site do Explorador de gráfico com as credenciais de administrador ou coadmin global para o seu inquilino.
    
3. Alterar a versão para **beta**e obter a lista de principais de serviço do seu inquilino, utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Se estiver a utilizar vários diretórios, siga este padrão: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Caixa de diálogo de Explorador de gráfico, com a consulta para obtenção de principais de serviço](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Na lista de principais de serviço obtida, obter que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os principais de serviço listado. Procure o ID de objeto que copiou do **propriedades** página e utilize a seguinte consulta para obter o principal de serviço:
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Consulta para obter o principal de serviço tem de modificar](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrair o **appRoles** propriedade do objeto principal do serviço.
    
    ![Detalhes da propriedade appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Para atualizar a função existente, utilize os seguintes passos.

    ![Corpo do pedido para "PATCH," com "Descrição" e "displayname" realçado](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    a. Alterar o método de **obter** para **PATCH**.

    b. Copiar as funções de existentes e cole-os em **corpo do pedido**.

    c. Atualize o valor de uma função ao atualizar a descrição da função, o valor de função ou o nome de apresentação da função, conforme necessário.

    d. Depois de atualizar todas as funções necessárias, selecione **executar consulta**.
        
## <a name="delete-an-existing-role"></a>Eliminar uma função existente

Para eliminar uma função existente, execute os seguintes passos:

1. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

2. Iniciar sessão para o site do Explorador de gráfico com as credenciais de administrador ou coadmin global para o seu inquilino.

3. Alterar a versão para **beta**e obter a lista de principais de serviço do seu inquilino, utilizando a seguinte consulta:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Se estiver a utilizar vários diretórios, siga este padrão: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo de Explorador de gráfico, com a consulta para obter a lista de principais de serviço](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Na lista de principais de serviço obtida, obter que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os principais de serviço listado. Procure o ID de objeto que copiou do **propriedades** página e utilize a seguinte consulta para obter o principal de serviço:
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Consulta para obter o principal de serviço tem de modificar](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrair o **appRoles** propriedade do objeto principal do serviço.
    
    ![Detalhes da propriedade appRoles do objeto principal de serviço](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Para eliminar a função existente, utilize os seguintes passos.

    ![Corpo do pedido para "PATCH," com IsEnabled definido como falso](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. Alterar o método de **obter** para **PATCH**.

    b. Copiar as funções existentes da aplicação e cole-os em **corpo do pedido**.
        
    c. Definir o **IsEnabled** valor **falso** para a função que pretende eliminar.

    d. Selecione **executar consulta**.
    
    > [!NOTE] 
    > Certifique-se de que possui a função de msiam_access e o ID de correspondência na função de gerado.
    
7. Depois da função estiver desativada, elimine o bloco de função a **appRoles** secção. Manter o método como **PATCH**e selecione **executar consulta**.
    
8. Depois de executar a consulta, a função é eliminada.
    
    > [!NOTE]
    > A função tem de ser desativado para que possa ser removido. 

## <a name="next-steps"></a>Passos Seguintes

Para obter os passos adicionais, consulte o [documentação da aplicação](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png