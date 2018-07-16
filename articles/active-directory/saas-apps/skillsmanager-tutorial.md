---
title: 'Tutorial: Integração do Azure Active Directory com o Gestor de habilidades | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Gestor de habilidades.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dab8debd-3b7b-4656-9bf0-1963ad8fce05
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: jeedes
ms.openlocfilehash: 06bd3670c576a0515956147996d7500c75cf8708
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050685"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-manager"></a>Tutorial: Integração do Azure Active Directory com o Gestor de habilidades

Neste tutorial, saiba como integrar o Gestor de habilidades com o Azure Active Directory (Azure AD).

Integrar o Gestor de competências no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Gestor de habilidades.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Gestor de habilidades (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Gestor de habilidades, precisa do seguinte:

- Uma subscrição do Azure AD
- Um Gestor de habilidades de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Gestor de habilidades da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-skills-manager-from-the-gallery"></a>Adicionando o Gestor de habilidades da Galeria
Para configurar a integração do Gestor de habilidades para o Azure AD, terá de adicionar o Gestor de habilidades da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Gestor de habilidades da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Manager habilidades**, selecione **habilidades Manager** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Gestor de habilidades na lista de resultados](./media/skillsmanager-tutorial/tutorial_skillsmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Gestor de habilidades com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Gestor de habilidades para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Gestor de habilidades deve ser estabelecido.

No Gestor de habilidades, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Gestor de habilidades, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Gestor de habilidades](#create-a-skills-manager-test-user)**  - para ter um equivalente da Eduarda Almeida no Gestor de habilidades que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Gestor de habilidades.

**Para configurar o Azure AD início de sessão único com o Gestor de habilidades, execute os seguintes passos:**

1. No portal do Azure, no **Manager habilidades** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/skillsmanager-tutorial/tutorial_skillsmanager_samlbase.png)

3. Sobre o **URLs de domínio do Gestor de habilidades e** secção, execute os seguintes passos:

    ![URLs de domínio do Gestor de habilidades e únicas início de sessão em informações](./media/skillsmanager-tutorial/tutorial_skillsmanager_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://subdomain.skills-manager.com/kennametal`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://subdomain.skills-manager.com/public/SamlLogin2.aspx`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte do Gestor de habilidades](https://www.ibm.com/support/uk/?lnk=msu_uk) obter esses valores.
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/skillsmanager-tutorial/tutorial_skillsmanager_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/skillsmanager-tutorial/tutorial_general_400.png)

6. Na **configuração do Gestor de habilidades** secção, clique em **configurar Gestor de habilidades** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do Gestor de habilidades](./media/skillsmanager-tutorial/tutorial_skillsmanager_configure.png) 

7. Para configurar o início de sessão único num **Manager habilidades** lado, terá de enviar o transferido **Certificate(Base64)**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [equipa de suporte do Gestor de habilidades](https://www.ibm.com/support/uk/?lnk=msu_uk). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/skillsmanager-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/skillsmanager-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/skillsmanager-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/skillsmanager-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-skills-manager-test-user"></a>Criar um utilizador de teste do Gestor de habilidades

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Gestor de habilidades. Trabalhar com [equipa de suporte do Gestor de habilidades](https://www.ibm.com/support/uk/?lnk=msu_uk) para adicionar os utilizadores na plataforma do Gestor de habilidades. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o Gestor de habilidades.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para o Gestor de habilidades, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **habilidades Manager**.

    ![A ligação de Gestor de habilidades na lista de aplicações](./media/skillsmanager-tutorial/tutorial_skillsmanager_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o mosaico do Gestor de habilidades no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Gestor de habilidades.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsmanager-tutorial/tutorial_general_01.png
[2]: ./media/skillsmanager-tutorial/tutorial_general_02.png
[3]: ./media/skillsmanager-tutorial/tutorial_general_03.png
[4]: ./media/skillsmanager-tutorial/tutorial_general_04.png

[100]: ./media/skillsmanager-tutorial/tutorial_general_100.png

[200]: ./media/skillsmanager-tutorial/tutorial_general_200.png
[201]: ./media/skillsmanager-tutorial/tutorial_general_201.png
[202]: ./media/skillsmanager-tutorial/tutorial_general_202.png
[203]: ./media/skillsmanager-tutorial/tutorial_general_203.png

