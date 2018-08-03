---
title: 'Tutorial: Integração do Azure Active Directory com mesclar os Thoughtworks | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e mesclar os Thoughtworks.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a685b5702aa9f74f3e0abf2a06774a30ac0d996f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436999"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Tutorial: Integração do Azure Active Directory com mesclar os Thoughtworks

Neste tutorial, saiba como integrar a Thoughtworks ser combinado com o Azure Active Directory (Azure AD).

Integrar a Thoughtworks ser combinado com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para ser combinado a Thoughtworks
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para mesclar os Thoughtworks (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com mesclar os Thoughtworks, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um mesclar os Thoughtworks logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a Thoughtworks mesclar da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Adicionando a Thoughtworks mesclar da Galeria
Para configurar a integração do Thoughtworks ser combinado com o Azure AD, terá de adicionar mesclar os Thoughtworks partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a Thoughtworks ser combinado a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **mesclar os Thoughtworks**, selecione **mesclar os Thoughtworks** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Mesclar os Thoughtworks na lista de resultados](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Thoughtworks ser combinado com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Thoughtworks ser combinado a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no mesclar os Thoughtworks deve ser estabelecido.

Mesclar os Thoughtworks, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com mesclar os Thoughtworks, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste mesclar os Thoughtworks](#create-a-thoughtworks-mingle-test-user)**  - para ter um equivalente da Eduarda Almeida na Thoughtworks mesclar os que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo Thoughtworks mesclar.

**Para configurar o Azure AD início de sessão único com mesclar os Thoughtworks, execute os seguintes passos:**

1. No portal do Azure, sobre o **mesclar os Thoughtworks** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. Sobre o **domínio de mesclar os Thoughtworks e URLs** secção, execute os seguintes passos:

    ![Domínio de mesclar os Thoughtworks e URLs únicas início de sessão em informações](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > O valor não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de mesclar os Thoughtworks](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) para obter o valor. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Inicie sessão no seu **mesclar os Thoughtworks** site da empresa como administrador.

1. Clique nas **administrador** separador e, em seguida, clique em **SSO Config**.
   
    ![Guia do administrador](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO Config")

1. Na **SSO Config** secção, execute os seguintes passos:
   
    ![Configuração SSO](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO Config")
    
    a. Para carregar o ficheiro de metadados, clique em **Escolher ficheiro**. 

    b. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Botão Adicionar](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![A caixa de diálogo de utilizador](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Criar um utilizador de teste mesclar os Thoughtworks

Para utilizadores do Azure AD possam iniciar sessão, tem de ser aprovisionados para a aplicação Thoughtworks ser combinado com seus nomes de utilizador do Azure Active Directory. No caso de ser combinado a Thoughtworks, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu site de empresa mesclar os Thoughtworks como administrador.

1. Clique em **perfil**.
   
    ![Seu primeiro projeto](./media/thoughtworks-mingle-tutorial/ic785160.png "seu primeiro projeto")

1. Clique nas **administrador** separador e, em seguida, clique em **utilizadores**.
   
    ![Os utilizadores](./media/thoughtworks-mingle-tutorial/ic785161.png "utilizadores")

1. Clique em **novo utilizador**.
   
    ![Novo utilizador](./media/thoughtworks-mingle-tutorial/ic785162.png "novo utilizador")

1. Sobre o **novo utilizador** caixa de diálogo página, execute os seguintes passos:
   
    ![Caixa de diálogo do nova usuário](./media/thoughtworks-mingle-tutorial/ic785163.png "novo utilizador")  
 
    a. Tipo de **nome de início de sessão**, **nome a apresentar**, **palavra-passe de escolha**, **Confirmar palavra-passe** de um Azure válido conta AD que pretende aprovisionar para as caixas de texto relacionadas. 

    b. Como **tipo de utilizador**, selecione **utilizador completo**.

    c. Clique em **criar este perfil**.

>[!NOTE]
>Pode utilizar quaisquer outras mesclar os Thoughtworks utilizador conta criação ferramentas ou APIs fornecidas pelo mesclar os Thoughtworks, para aprovisionar contas de utilizador do AAD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ser combinado a Thoughtworks.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para ser combinado a Thoughtworks, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **mesclar os Thoughtworks**.

    ![A ligação ser combinado a Thoughtworks, na lista de aplicações](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de mesclar os Thoughtworks de mensagens em fila no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Thoughtworks mesclar.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

