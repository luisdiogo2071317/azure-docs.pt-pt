---
title: 'Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS) para ligar várias contas | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure AD e várias contas do Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: jeedes
ms.openlocfilehash: 9634d8ede40500bf0a92ae07a1a514895d355a31
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437938"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Tutorial: Integração do Azure Active Directory com várias contas do Amazon Web Services (AWS)

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com várias contas do Amazon Web Services (AWS).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Amazon Web Services (AWS) (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure AD básica ou premium
- Amazon Web Services (AWS) vários início de sessão único de contas ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Amazon Web Services (AWS) da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adicionando o Amazon Web Services (AWS) da Galeria
Para configurar a integração do Amazon Web Services (AWS) para o Azure AD, terá de adicionar Amazon Web Services (AWS) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Amazon Web Services (AWS) a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Amazon Web Services (AWS)**, selecione **Amazon Web Services (AWS)** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Amazon Web Services (AWS) na lista de resultados](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

1. Depois do aplicativo é adicionado, vá para **propriedades** página e copie a **ID de objeto**.

    ![Amazon Web Services (AWS) na lista de resultados](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Amazon Web Services (AWS) para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Amazon Web Services (AWS) deve ser estabelecido.

No Amazon Web Services (AWS), atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Amazon Web Services (AWS), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Amazon Web Services (AWS).

**Para configurar o Azure AD início de sessão único com o Amazon Web Services (AWS), execute os seguintes passos:**

1. No portal do Azure, sobre o **Amazon Web Services (AWS)** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

1. Sobre o **Amazon Web Services (AWS) de domínio e URLs** secção, o utilizador não tem de efetuar outros passos de como a aplicação já está pré-integrada com o Azure.

    ![URLs de domínio do Amazon Web Services (AWS) e únicas início de sessão em informações](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

1. O aplicativo de Software do Amazon Web Services (AWS) espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único atributo](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_attribute.png)    

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo | Espaço de Nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Tem de configurar o aprovisionamento de utilizadores no Azure AD para obter todas as funções a partir da consola do AWS. Consulte os passos de aprovisionamento abaixo.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único atributo](./media/aws-multi-accounts-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Na **espaço de nomes** caixa de texto, digite o valor de espaço de nomes mostrado para essa linha.
    
    d. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/aws-multi-accounts-tutorial/tutorial_general_400.png)

1. Numa janela do browser diferente, início de sessão no site da sua empresa Amazon Web Services (AWS) como administrador.

1. Clique em **AWS Home**.
   
    ![Configurar a página inicial do início de sessão único][11]

1. Clique em **IAM** (identidade e gestão de acesso). 
   
    ![Configurar a identidade de início de sessão única][12]

1. Clique em **fornecedores de identidade**e, em seguida, clique em **criar fornecedor**. 
   
    ![Configurar o fornecedor de início de sessão único][13]

1. Sobre o **configurar o fornecedor** caixa de diálogo página, execute os seguintes passos: 
   
    ![Configurar o início de sessão único de caixa de diálogo][14]
 
    a. Como **tipo de fornecedor**, selecione **SAML**.

    b. Na **nome do fornecedor** caixa de texto, escreva um nome de fornecedor (por exemplo: *WAAD*).

    c. A carregar seu transferido **ficheiro de metadados** a partir do portal do Azure, clique em **Escolher ficheiro**.

    d. Clique em **passo seguinte**.

1. Sobre o **Certifique-se de informações do fornecedor** página de diálogo, clique em **criar**. 
    
    ![Configurar o início de sessão único verificar][15]

1. Clique em **funções**e, em seguida, clique em **criar função**. 
    
    ![Configurar funções de início de sessão únicas][16]

1. Sobre o **criar função** página, execute os seguintes passos:  
    
    ![Configurar a confiança de início de sessão única][19] 

    a. Selecione **Federação SAML 2.0** sob **selecione o tipo de entidade fidedigna**.

    b. Sob **escolher uma secção de SAML 2.0 fornecedor**, selecione a **fornecedor de SAML** que criou anteriormente (por exemplo: *WAAD*)

    c. Selecione **permitir programática e o acesso à consola de gestão do AWS**.
  
    d. Clique em **seguinte: permissões**.

1. Na **políticas de permissões de anexar** caixa de diálogo, clique em **próximo: revisão**.  
    
    ![Configurar a política de início de sessão única][33]

1. Sobre o **revisão** caixa de diálogo, execute os seguintes passos:   
    
    ![Configurar a revisão de início de sessão único][34] 

    a. Na **nome da função** caixa de texto, introduza o seu nome de função.

    b. Na **descrição da função** caixa de texto, introduza a descrição.

    a. Clique em **criar função**.

    b. Crie tantas funções conforme necessário e mapeá-los para o fornecedor de identidade.

1. Terminar sessão atual conta AWS e inicie sessão com outra conta em que pretende configurar o início de sessão único com o Azure AD.

1. Execute a 9 passo a passo e 17 para criar várias funções que pretende o programa de configuração para esta conta. Se tiver mais de duas contas, execute os mesmos passos para todas as contas criar funções para os mesmos.

1. Depois de todas as funções são criadas no contas, elas aparecem a **funções** lista para essas contas.

    ![Configuração de funções](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

1. É necessário capturar todas as funções ARN e entidades fidedignas para todas as funções entre todas as contas, que é necessário mapear manualmente com a aplicação do Azure AD. 

1. Clique em funções para copiar **função ARN** e **entidades fidedignas** valores. Vai precisar destes valores para todas as funções que precisa criar no Azure AD.

    ![Configuração de funções](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)
 
1. Executar o passo anterior para todas as funções em todas as contas e armazenar todos eles no formato **ARN de função, entidades fidedignas** num bloco de notas. 

1. Open [do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    a. Inicie sessão para o site de API do Graph com as credenciais de Administrador Global/Co-administradores para o seu inquilino.

    b. Tem de ter permissões suficientes para criar as funções. Clique em **modificar permissões** para obter as permissões necessárias. 

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Selecione seguindo as permissões da lista (se ainda não tiver estas) e clique em "Modificar permissões" 

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Isto irá pedir-lhe para iniciar sessão novamente e aceitar o consentimento. Depois de aceitar o consentimento, esteja conectado ao Explorador do gráfico novamente.

    e. Alterar o menu suspenso de versão para **beta**. Para obter todos os principais de serviço a partir do seu inquilino, utilize a seguinte consulta:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Se estiver a utilizar vários diretórios, em seguida, pode utilizar seguindo o padrão, que tem o seu domínio primário no mesmo  `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)
    
    f. Na lista de principais de serviço obtido, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para pesquisar o aplicativo de todos os ServicePrincipals listadas. Pode utilizar a seguinte consulta utilizando o **id de objeto** que copiou a partir da página de propriedades do Azure AD para obter ao Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Extrair a propriedade appRoles do objeto principal de serviço. 

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Agora precisa gerar novas funções para a sua aplicação. 

    i. Abaixo JSON é um exemplo do objeto appRoles. Crie um objeto semelhante para adicionar as funções que pretende para a sua aplicação. 

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Só é possível adicionar novas funções depois do **msiam_access** para a operação de correção. Além disso, pode adicionar tantas funções conforme desejado por sua organização necessitam. Azure AD irá enviar a **valor** destas funções, como o valor de afirmação na resposta SAML.
    
    j. Voltar à sua API do Graph e altere o método da **Obtenha** para **aplicar o PATCH**. O objeto Principal de serviço para ter pretendido funções por atualizar a propriedade appRoles semelhante ao mostrado acima no exemplo de patches. Clique em **executar consulta** para executar a operação de correção. Uma mensagem de êxito confirma a criação da função para a sua aplicação do Amazon Web Services.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Depois do Principal de serviço for corrigido com mais de funções, pode atribuir utilizadores/grupos para as respetivas funções. Isso pode ser feito ao aceder ao portal e navegar para a aplicação do Amazon Web Services. Clique nas **utilizadores e grupos** separador na parte superior. 

1. Recomendamos que crie novos grupos para cada função do AWS, de modo a que possa atribuir essa função específica desse grupo. Tenha em atenção que se trata de um para um mapeamento para um grupo a uma função. Em seguida, pode adicionar os membros que pertencem a esse grupo.

1. Assim que os grupos são criados, selecione o grupo e atribuir à aplicação. 

    ![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

1. Para atribuir a função ao grupo, selecione a função e clique em **atribuir** botão na parte inferior da página.

    ![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Tenha em atenção que terá de atualizar a sua sessão no portal do Azure para ver novas funções.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Amazon Web Services (AWS) no painel de acesso, deve obter a página de aplicativo do Amazon Web Services (AWS) com opção de selecionar a função.

![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Também pode verificar a resposta SAML para ver as funções que está a ser passadas como afirmações.

![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/aws-multi-accounts-tutorial/tutorial_general_01.png
[2]: ./media/aws-multi-accounts-tutorial/tutorial_general_02.png
[3]: ./media/aws-multi-accounts-tutorial/tutorial_general_03.png
[4]: ./media/aws-multi-accounts-tutorial/tutorial_general_04.png

[100]: ./media/aws-multi-accounts-tutorial/tutorial_general_100.png

[200]: ./media/aws-multi-accounts-tutorial/tutorial_general_200.png
[201]: ./media/aws-multi-accounts-tutorial/tutorial_general_201.png
[202]: ./media/aws-multi-accounts-tutorial/tutorial_general_202.png
[203]: ./media/aws-multi-accounts-tutorial/tutorial_general_203.png
[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_on.png

