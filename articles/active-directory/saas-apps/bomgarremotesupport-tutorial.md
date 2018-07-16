---
title: 'Tutorial: Integração do Azure Active Directory com suporte remoto de Bomgar | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Bomgar remota de suporte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 193b163f-bdee-4974-b16d-777c51b991df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: c59f4291726b24b7c96bb60d0497c1578a3e4b0f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048186"
---
# <a name="tutorial-azure-active-directory-integration-with-bomgar-remote-support"></a>Tutorial: Integração do Azure Active Directory com Bomgar remota de suporte

Neste tutorial, saiba como integrar Bomgar remota de suporte com o Azure Active Directory (Azure AD).

Integrar Bomgar remota de suporte com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Bomgar remota de suporte.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Bomgar remota de suporte (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Bomgar remota de suporte, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um suporte de remoto Bomgar logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Bomgar remota de suporte da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bomgar-remote-support-from-the-gallery"></a>Adicionando Bomgar remota de suporte da Galeria
Para configurar a integração do Bomgar remota de suporte para o Azure AD, terá de adicionar Bomgar remota de suporte a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Bomgar remota de suporte a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **remota de suporte Bomgar**, selecione **Bomgar remota de suporte** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Bomgar remota de suporte na lista de resultados](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com suporte remoto Bomgar com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Bomgar remota de suporte a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no suporte de Bomgar remoto tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com Bomgar remota de suporte, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de suporte de remoto Bomgar](#create-a-bomgar-remote-support-test-user)**  - para ter um equivalente da Eduarda Almeida na Bomgar suporte remoto que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Bomgar remota de suporte.

**Para configurar o Azure AD início de sessão único com suporte remoto de Bomgar, execute os seguintes passos:**

1. No portal do Azure, sobre o **Bomgar remota de suporte** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_samlbase.png)

3. Sobre o **Bomgar remoto suporte de domínio e URLs** secção, execute os seguintes passos:

    ![URLs de domínio de suporte remoto Bomgar e únicas início de sessão em informações](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.trafficmanager.net/saml`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.trafficmanager.net`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador (ID de entidade). Contacte [equipa de suporte de cliente de suporte remoto Bomgar](https://www.bomgar.com/docs/index.htm#support) obter esses valores. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/bomgarremotesupport-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **Bomgar remoto suporta** lado, terá de enviar o transferido **XML de metadados** para [Bomgar remoto suporta a equipa de suporte](https://www.bomgar.com/docs/index.htm#support). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/bomgarremotesupport-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/bomgarremotesupport-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/bomgarremotesupport-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/bomgarremotesupport-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-bomgar-remote-support-test-user"></a>Criar um utilizador de teste Bomgar remota de suporte

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Bomgar remota de suporte. Suporte remoto de Bomgar suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Bomgar remota de suporte se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [Bomgar remoto suporta a equipa de suporte](https://www.bomgar.com/docs/index.htm#support).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso para Bomgar remota de suporte.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Bomgar remota de suporte, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Bomgar remota de suporte**.

    ![A ligação remota de suporte Bomgar na lista de aplicações](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Bomgar remota de suporte no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Bomgar remota de suporte.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bomgarremotesupport-tutorial/tutorial_general_01.png
[2]: ./media/bomgarremotesupport-tutorial/tutorial_general_02.png
[3]: ./media/bomgarremotesupport-tutorial/tutorial_general_03.png
[4]: ./media/bomgarremotesupport-tutorial/tutorial_general_04.png

[100]: ./media/bomgarremotesupport-tutorial/tutorial_general_100.png

[200]: ./media/bomgarremotesupport-tutorial/tutorial_general_200.png
[201]: ./media/bomgarremotesupport-tutorial/tutorial_general_201.png
[202]: ./media/bomgarremotesupport-tutorial/tutorial_general_202.png
[203]: ./media/bomgarremotesupport-tutorial/tutorial_general_203.png

