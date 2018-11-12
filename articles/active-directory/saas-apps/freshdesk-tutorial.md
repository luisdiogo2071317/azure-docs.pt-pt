---
title: 'Tutorial: Integração do Azure Active Directory com o FreshDesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010818"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Integração do Azure Active Directory com o FreshDesk

Neste tutorial, saiba como integrar o FreshDesk no Azure Active Directory (Azure AD).

Integrar o FreshDesk no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o FreshDesk.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o FreshDesk (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o FreshDesk, terá dos seguintes itens:

- Uma subscrição do Azure
- Um FreshDesk logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar FreshDesk a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-freshdesk-from-the-gallery"></a>Adicionar FreshDesk a partir da Galeria

Para configurar a integração do FreshDesk no Azure AD, terá de adicionar FreshDesk a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar FreshDesk a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **FreshDesk**, selecione **FreshDesk** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![FreshDesk na lista de resultados](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o FreshDesk, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no FreshDesk a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no FreshDesk deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o FreshDesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do FreshDesk](#creating-a-freshdesk-test-user)**  - para ter um equivalente da Eduarda Almeida no FreshDesk a que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de FreshDesk.

**Para configurar o Azure AD início de sessão único com o FreshDesk, execute os seguintes passos:**

1. No portal do Azure, sobre o **FreshDesk** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial_general_301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![Domínio de FreshDesk e URLs únicas início de sessão em informações](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.freshdesk.com` ou qualquer outro valor Freshdesk sugeriu.

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.freshdesk.com` ou qualquer outro valor Freshdesk sugeriu.

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) obter esses valores.

5. Aplicação de FreshDesk espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso. O valor predefinido **identificador de utilizador** é **user.userprincipalname** mas **FreshDesk** espera que isso seja mapeado com o endereço de e-mail do utilizador. Para que pode usar **user.mail** atributo da lista ou utilize o valor do atributo adequado com base na configuração da sua organização.

    ![image](./media/freshdesk-tutorial/i4-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador e afirmações** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    a. Clique em **ícone de edição** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/freshdesk-tutorial/i2-attribute.png)

    ![image](./media/freshdesk-tutorial/i3-attribute.png)

    b. Partir do **atributo de origem** lista, selecione **user.mail**.

    c. Clique em **Guardar**.

7. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. Open **linha de comandos** e execute os seguintes comandos:

    a. Introduza `certutil.exe -dump FreshDesk.cer` valor no prompt de comando.

    > [!NOTE]
    > Aqui **FreshDesk.cer** é o certificado que transferiu do portal do Azure.

    b. Copiar o **Cert Hash(sha256)** valor e cole-o bloco de notas. 

9. Sobre o **configurar o FreshDesk** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![Configuração de FreshDesk](common/configuresection.png)

10. Numa janela do browser web diferente, inicie sessão no site da sua empresa Freshdesk como um administrador.

11. Selecione o **ícone de definições** e, no **segurança** secção, execute os seguintes passos:

    ![Início de sessão único](./media/freshdesk-tutorial/IC776770.png "início de sessão único")
  
    a. Para **único início de sessão (SSO)**, selecione **no**.

    b. Selecione **SAML SSO**.

    c. Na **URL de início de sessão de SAML** caixa de texto, colar **URL de início de sessão** valor, que copiou do portal do Azure.

    d. Na **URL de fim de sessão** caixa de texto, colar **URL de fim de sessão** valor, que copiou do portal do Azure.

    e. Na **impressão digital do certificado de segurança** caixa de texto, colar **Cert Hash(sha256)** valor que obteve anteriormente.
  
    f. Clique em **Guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-freshdesk-test-user"></a>Criar um utilizador de teste do FreshDesk

Para habilitar os utilizadores do Azure AD iniciar sessão no FreshDesk, tem de ser aprovisionados no FreshDesk.  
No caso do FreshDesk, aprovisionamento é uma tarefa manual.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Freshdesk** inquilino.

2. No menu na parte superior, clique em **administrador**.

    ![Admin](./media/freshdesk-tutorial/IC776772.png "Admin")

3. Na **definições gerais** separador, clique em **agentes**.
  
    ![Agentes](./media/freshdesk-tutorial/IC776773.png "agentes")

4. Clique em **novo agente**.

    ![Novo agente](./media/freshdesk-tutorial/IC776774.png "novo agente")

5. Na caixa de diálogo informações do agente, execute os seguintes passos:

    ![Informações de agentes](./media/freshdesk-tutorial/IC776775.png "informações de agentes")

    a. Na **E-Mail** caixa de texto, o Azure AD de tipo de endereço de e-mail da conta do Azure AD que pretende aprovisionar.

    b. Na **FullName** caixa de texto, escreva o nome da conta do Azure AD que pretende aprovisionar.

    c. Na **Title** caixa de texto, digite o título da conta do Azure AD que pretende aprovisionar.

    d. Clique em **Guardar**.

    >[!NOTE]
    >O titular da conta do Azure AD irá receber uma mensagem de e-mail que inclui uma ligação para confirmar a conta antes de ser ativado.
    >
    >[!NOTE]
    >Pode utilizar quaisquer outras Freshdesk utilizador conta criação ferramentas ou APIs fornecidas pelo Freshdesk para aprovisionar contas de utilizador do AAD para o FreshDesk.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o FreshDesk.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **FreshDesk**.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do FreshDesk no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de FreshDesk.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
