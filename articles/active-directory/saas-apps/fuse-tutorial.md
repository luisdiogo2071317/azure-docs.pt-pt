---
title: 'Tutorial: Integração do Active Directory do Azure com Fuse | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e fusíveis.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0054e805f4d07a92361079eeee391c8367b4117
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208491"
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a>Tutorial: Integração do Active Directory do Azure com Fuse

Neste tutorial, saiba como integrar Fuse com o Azure Active Directory (Azure AD).

Integração de fusíveis com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao fusíveis.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Fuse (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Fuse, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Fuse logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Fuse a partir da Galeria
2. Configurar e testar o Azure AD início de sessão único

## <a name="add-fuse-from-the-gallery"></a>Adicionar Fuse a partir da Galeria
Para configurar a integração de ir para o Azure AD, terá de adicionar Fuse a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Fuse a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **combinar**, selecione **combinar** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Combinar na lista de resultados](./media/fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Fuse com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Fuse a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Fuse deve ser estabelecido.

Fuse, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Fuse, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de fusíveis](#create-a-fuse-test-user)**  - para ter um equivalente da Eduarda Almeida na Fuse que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de fusíveis.

**Para configurar o Azure AD início de sessão único com Fuse, execute os seguintes passos:**

1. No portal do Azure, sobre o **combinar** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/fuse-tutorial/tutorial_fuse_samlbase.png)

3. Sobre o **combinar o domínio e URLs** secção, execute os seguintes passos:

    ![Combinar o domínio e URLs únicas início de sessão em informações](./media/fuse-tutorial/tutorial_fuse_url.png)
    
    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant name>.fusionuniversal.com/`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de combinar](mailto:support@fusion-universal.com) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/fuse-tutorial/tutorial_fuse_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/fuse-tutorial/tutorial_general_400.png)

6. Sobre o **configuração combinar** secção, clique em **configurar combinar** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de fuse](./media/fuse-tutorial/tutorial_fuse_configure.png) 

7. Para obter SSO configurado para a sua aplicação, contacte [equipa de suporte de fusíveis](mailto:support@fusion-universal.com) e fornecê-los com o seguinte:

    * O transferido **ficheiro de certificado (Raw)**
    * O **URL do serviço de início de início de sessão único SAML**
    * O **ID de entidade SAML**
    * O **URL de fim de sessão**

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/fuse-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/fuse-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/fuse-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/fuse-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-fuse-test-user"></a>Criar um utilizador de teste Fuse

Nesta secção, vai criar um usuário chamado Eduarda Almeida no fusíveis. Trabalhe em conjunto com [equipa de suporte de fusíveis](mailto:support@fusion-universal.com) para adicionar os utilizadores da plataforma de fusíveis.


### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso para fusíveis.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Fuse, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **combinar**.

    ![A ligação de fusíveis na lista de aplicações](./media/fuse-tutorial/tutorial_fuse_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de fusíveis no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de fusíveis.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/fuse-tutorial/tutorial_general_01.png
[2]: ./media/fuse-tutorial/tutorial_general_02.png
[3]: ./media/fuse-tutorial/tutorial_general_03.png
[4]: ./media/fuse-tutorial/tutorial_general_04.png

[100]: ./media/fuse-tutorial/tutorial_general_100.png

[200]: ./media/fuse-tutorial/tutorial_general_200.png
[201]: ./media/fuse-tutorial/tutorial_general_201.png
[202]: ./media/fuse-tutorial/tutorial_general_202.png
[203]: ./media/fuse-tutorial/tutorial_general_203.png

