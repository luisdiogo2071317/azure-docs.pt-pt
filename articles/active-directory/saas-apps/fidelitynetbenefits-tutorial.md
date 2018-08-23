---
title: 'Tutorial: Integração do Azure Active Directory com fidelidade NetBenefits | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e NetBenefits fidelidade.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: a1308035a8b758a9e2f824de3a78c03103c19931
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42055763"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Tutorial: Integração do Azure Active Directory com fidelidade NetBenefits

Neste tutorial, saiba como integrar NetBenefits de fidelidade com o Azure Active Directory (Azure AD).

Integrar NetBenefits de fidelidade com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao NetBenefits fidelidade.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para fidelidade NetBenefits (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com fidelidade NetBenefits, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma fidelidade NetBenefits logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando NetBenefits fidelidade da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Adicionando NetBenefits fidelidade da Galeria

Para configurar a integração do NetBenefits de fidelidade com o Azure AD, terá de adicionar fidelidade NetBenefits a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar fidelidade NetBenefits a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **fidelidade NetBenefits**, selecione **fidelidade NetBenefits** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Fidelidade NetBenefits na lista de resultados](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com NetBenefits de fidelidade com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no fidelidade NetBenefits a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no fidelidade NetBenefits deve ser estabelecido.

No NetBenefits fidelidade, **usuário** mapeamento deve ser feito com **utilizador do Azure AD** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com fidelidade NetBenefits, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de fidelidade NetBenefits](#create-a-fidelity-netbenefits-test-user)**  - para ter um equivalente da Eduarda Almeida na NetBenefits fidelidade que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo NetBenefits fidelidade.

**Para configurar o Azure AD início de sessão único com fidelidade NetBenefits, execute os seguintes passos:**

1. No portal do Azure, sobre o **fidelidade NetBenefits** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

3. Sobre o **fidelidade NetBenefits domínio e URLs** secção, execute os seguintes passos:

    ![Fidelidade NetBenefits domínio e URLs únicas início de sessão em informações](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

    a. Na **identificador** caixa de texto, escreva um URL:

    Para o ambiente de teste:  `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Para o ambiente de produção:  `urn:sp:fidelity:geninbndnbparts20`

    b. Na **URL de resposta** caixa de texto, introduza um URL que, para ser fornecido pelo fidelidade no momento da implementação ou contacte o seu Gestor de serviço de cliente de fidelidade atribuído.

4. Aplicação de NetBenefits fidelidade espera que as asserções SAML num formato específico. Vamos tiver mapeado a **identificador de utilizador** com o **user.userprincipalname**. Pode mapear isto com **employeeid** ou qualquer outra violação é aplicável a sua organização conforme **identificador de utilizador**. Captura de ecrã seguinte mostra apenas um exemplo disso.

    ![Atributo de NetBenefits fidelidade](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Fidelidade NetBenefits suporta estática e dinâmica de Federação. Estática significa que não utilizará SAML com base just-in-time utilizador aprovisionamento e dinâmica significa que ele oferece suporte apenas no tempo de aprovisionamento de utilizador. Para utilizar o JIT, os clientes de aprovisionamento com base tem de adicionar algumas declarações mais no Azure AD, como a data de nascimento do usuário etc. Esses detalhes são fornecidos pelo seu atribuído **Gestor de serviço de cliente de fidelidade** e têm de ativar esta federação dinâmica para a sua instância.

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png)

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

7. Sobre o **fidelidade NetBenefits configuração** secção, clique em **configurar NetBenefits de fidelidade** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de NetBenefits fidelidade](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png)

8. Para configurar o início de sessão único num **fidelidade NetBenefits** lado, terá de enviar o transferido **XML de metadados**, **SAML único início de sessão no URL do serviço** e  **ID de entidade de SAML** para **seu Gestor de serviço de cliente de fidelidade atribuído**. Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Criar um utilizador de teste de fidelidade NetBenefits

Nesta secção, vai criar um usuário chamado Eduarda Almeida no NetBenefits fidelidade. Se estiver a criar o Federação estática, trabalhe em conjunto com sua atribuído **Gestor de serviço de cliente de fidelidade** para criar os utilizadores na plataforma de fidelidade NetBenefits. Estes utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

Para a Federação dinâmica, os utilizadores são criados com o aprovisionamento de utilizador do Just In Time. Para utilizar o JIT, os clientes de aprovisionamento com base tem de adicionar algumas declarações mais no Azure AD, como a data de nascimento do usuário etc. Esses detalhes são fornecidos pelo seu atribuído **Gestor de serviço de cliente de fidelidade** e têm de ativar esta federação dinâmica para a sua instância.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a fidelidade NetBenefits.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a fidelidade NetBenefits, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **fidelidade NetBenefits**.

    ![A ligação de fidelidade NetBenefits na lista de aplicações](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de fidelidade NetBenefits no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação NetBenefits fidelidade.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png