---
title: 'Tutorial: Integração do Azure Active Directory com Recognize | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Recognize.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: a4ea4272b11c167d95840b45b39634abea8c1cea
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051603"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Tutorial: Integração do Azure Active Directory com Recognize

Neste tutorial, saiba como integrar Recognize com o Azure Active Directory (Azure AD).

Reconhecer a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao reconhecer
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para reconhecer (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Recognize, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Recognize logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Recognize da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-recognize-from-the-gallery"></a>Adicionando Recognize da Galeria
Para configurar a integração de reconhecer no Azure AD, terá de adicionar reconhecer a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar reconhecer a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Recognize**.

    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/tutorial_recognize_search.png)

5. No painel de resultados, selecione **Recognize**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Recognize com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no reconhecer a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Recognize deve ser estabelecido.

Reconhecer, atribua o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Recognize, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de reconhecer](#creating-a-recognize-test-user)**  - para ter um equivalente da Eduarda Almeida na reconhecer que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo Recognize.

**Para configurar o Azure AD início de sessão único com Recognize, execute os seguintes passos:**

1. No portal do Azure, sobre o **Recognize** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_recognize_samlbase.png)

3. Sobre o **reconhecer o domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_recognize_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://recognizeapp.com/<your-domain>/saml/sso`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de reconhecer](mailto:support@recognizeapp.com) obter o URL de início de sessão e pode obter o valor do identificador ao abrir o URL de metadados do fornecedor de serviço da secção definições de SSO que é explicada mais tarde no tutorial. . 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_recognize_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_general_400.png)

6. Sobre o **configuração reconhecer** secção, clique em **configurar reconhecer** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_recognize_configure.png) 

7. Numa janela do browser web diferente, início de sessão no seu inquilino de reconhecer como administrador.

8. No canto superior direito, clique em **Menu**. Aceda a **da empresa administrador**.
   
    ![Configure o lado de início de sessão na aplicação única](./media/recognize-tutorial/tutorial_recognize_000.png)

9. No painel de navegação esquerdo, clique em **definições**.
   
    ![Configure o lado de início de sessão na aplicação única](./media/recognize-tutorial/tutorial_recognize_001.png)

10. Execute os seguintes passos no **as definições de SSO** secção.
   
    ![Configure o lado de início de sessão na aplicação única](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. Como **ativar o SSO**, selecione **ON**.

    b. Na **ID de entidade do IDP** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.
    
    c. Na **url de destino de Sso** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
    
    d. Na **url de destino de Slo** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure. 
    
    e. Abra sua transferido **certificado (Base64)** do ficheiro no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado** caixa de texto.
    
    f. Clique nas **guardar as definições de** botão. 

11. Junto a **as definições de SSO** secção, copie o URL sob **url de metadados do fornecedor de serviço**.
   
    ![Configure o lado de início de sessão na aplicação única](./media/recognize-tutorial/tutorial_recognize_003.png)

12. Abra o **ligação de URL de metadados** num navegador em branco para transferir o documento de metadados. Em seguida, copie o value(entityID) EntityDescriptor do ficheiro e cole-a no **identificador** na caixa de texto **seção reconhecer o domínio e URLs** no portal do Azure.
    
    ![Configure o lado de início de sessão na aplicação única](./media/recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/recognize-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-recognize-test-user"></a>Criar um utilizador de teste de reconhecer

Para habilitar logon Recognize de utilizadores do Azure AD, tem de ser aprovisionados em Recognize. No caso de reconhecer, o aprovisionamento é uma tarefa manual.

Esta aplicação não suporta o aprovisionamento SCIM, mas tem uma sincronização de utilizador alternativas que Aprovisiona utilizadores. 

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa reconhecer como administrador.

2. No canto superior direito, clique em **Menu**. Aceda a **da empresa administrador**.

3. No painel de navegação esquerdo, clique em **definições**.

4. Execute os seguintes passos no **sincronização de utilizador** secção.
   
   ![Novo utilizador](./media/recognize-tutorial/tutorial_recognize_005.png "novo utilizador")
   
   a. Como **sincronização ativada**, selecione **ON**.
   
   b. Como **fornecedor de sincronização de escolher**, selecione **Microsoft / Office 365**.
   
   c. Clique em **execute a sincronização de utilizador**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para reconhecer.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a reconhecer, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Recognize**.

    ![Configurar o início de sessão único](./media/recognize-tutorial/tutorial_recognize_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de reconhecer no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Recognize. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/recognize-tutorial/tutorial_general_01.png
[2]: ./media/recognize-tutorial/tutorial_general_02.png
[3]: ./media/recognize-tutorial/tutorial_general_03.png
[4]: ./media/recognize-tutorial/tutorial_general_04.png

[100]: ./media/recognize-tutorial/tutorial_general_100.png

[200]: ./media/recognize-tutorial/tutorial_general_200.png
[201]: ./media/recognize-tutorial/tutorial_general_201.png
[202]: ./media/recognize-tutorial/tutorial_general_202.png
[203]: ./media/recognize-tutorial/tutorial_general_203.png

