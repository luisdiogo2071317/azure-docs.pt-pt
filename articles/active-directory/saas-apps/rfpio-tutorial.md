---
title: 'Tutorial: Integração do Azure Active Directory com RFPIO | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e RFPIO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 59b05814be0be9042e7507cc8d928b5f5feb80ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051766"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Tutorial: Integração do Azure Active Directory com RFPIO

Neste tutorial, saiba como integrar RFPIO com o Azure Active Directory (Azure AD).

Integrar RFPIO no Azure AD fornece as seguintes vantagens:

- Pode controlar quem no Azure AD que tenha acesso ao RFPIO.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para RFPIO (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com RFPIO, terá dos seguintes itens:

- Uma subscrição do Azure AD.
- Uma RFPIO única início de sessão no ativada subscrição.

> [!NOTE]
> Não recomendamos que utilize um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter um [versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando RFPIO da galeria.
2. Configuração e teste do Azure AD início de sessão único.

## <a name="add-rfpio-from-the-gallery"></a>Adicionar RFPIO a partir da Galeria
Para configurar a integração do RFPIO com o Azure AD, terá de adicionar RFPIO a partir da Galeria à sua lista de aplicações de SaaS geridas.

### <a name="to-add-rfpio-from-the-gallery"></a>Para adicionar RFPIO a partir da Galeria

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, selecione o **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Selecione **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **RFPIO**.

    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/tutorial_rfpio_search.png)

5. No painel de resultados, selecione **RFPIO**e, em seguida, selecione a **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com RFPIO com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é a relação entre o utilizador de contraparte no RFPIO e um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no RFPIO deve ser estabelecido.

RFPIO, atribua o valor de **nome de utilizador** no Azure AD como o valor de **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com RFPIO, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**– para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**– para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste RFPIO](#creating-a-rfpio-test-user)**  – para ter um equivalente da Eduarda Almeida no RFPIO que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**– para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#testing-single-sign-on)**  – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo RFPIO.

**Para configurar o Azure AD início de sessão único com RFPIO, execute os seguintes passos:**

1. No portal do Azure, sobre o **RFPIO** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. Sobre o **RFPIO domínio e URLs** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_url.png)

    a. Na **identificador** caixa de texto, escreva o URL: `https://www.rfpio.com`

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Verifique **Mostrar definições de URL avançadas**.

    c. Na **estado de reencaminhamento** caixa de texto, introduza um valor de cadeia de caracteres. Contacte [equipa de suporte de RFPIO](https://www.rfpio.com/contact/) para obter este valor. 

4. Verifique **Mostrar definições de URL avançadas**. Se desejar configurar a aplicação no **SP** iniciada pelo modo: 

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_url2.png)

    Na **iniciar sessão no URL** caixa de texto, escreva o URL: `https://www.app.rfpio.com`

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_general_400.png)

7. Numa janela do browser web diferente, início de sessão para o **RFPIO** Web site como um administrador.

8. Clique no menu pendente na parte inferior esquerda canto.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app1.png)

9. Clique nas **definições de organização**. 

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app2.png)

10. Clique nas **funcionalidades e integração**.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app4.png)

11. Na **configuração de SAML SSO** clique em **editar**.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app3.png)

12. Nesta secção executa as ações seguintes:

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app5.png)
    
    a. Copie o conteúdo do **XML de metadados baixado** e cole-o para o **configuração de identidade** campo.

    > [!NOTE]
    >Para copiar o conteúdo de transferido **XML de metadados** utilização **bloco de notas + +** ou adequado **Editor de XML**. 

    b. Clique em **validar**.

    c. Depois de clicar em **validar**, Flip **SAML(Enabled)** como ativado.

    d. Clique em **submeter**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/rfpio-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-a-rfpio-test-user"></a>Criar um utilizador de teste RFPIO

Para ativar a utilizadores do Azure AD iniciar sessão no RFPIO, tem de ser aprovisionados em RFPIO.  
No caso de RFPIO, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa RFPIO como administrador.

2. Clique no menu pendente na parte inferior esquerda canto.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app1.png)

3. Clique nas **definições de organização**. 

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app2.png)

4. Clique em **os integrantes da EQUIPE**.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app6.png)

5. Clique em **adicionar membros**.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app7.png)

6. Na **adicionar novos membros** secção. Execute as ações seguintes:

    ![Configurar o início de sessão único](./media/rfpio-tutorial/app8.png)

    a. ENTER **endereço de E-Mail** no **introduza um e-mail por linha** campo.

    b. Selecione Plese **função** de acordo com os seus requisitos.

    c. Clique em **adicionar membros**.
        
    > [!NOTE]
    > O titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para RFPIO.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a RFPIO, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **RFPIO**.

    ![Configurar o início de sessão único](./media/rfpio-tutorial/tutorial_rfpio_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando clica no mosaico RFPIO no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo RFPIO.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/rfpio-tutorial/tutorial_general_01.png
[2]: ./media/rfpio-tutorial/tutorial_general_02.png
[3]: ./media/rfpio-tutorial/tutorial_general_03.png
[4]: ./media/rfpio-tutorial/tutorial_general_04.png

[100]: ./media/rfpio-tutorial/tutorial_general_100.png

[200]: ./media/rfpio-tutorial/tutorial_general_200.png
[201]: ./media/rfpio-tutorial/tutorial_general_201.png
[202]: ./media/rfpio-tutorial/tutorial_general_202.png
[203]: ./media/rfpio-tutorial/tutorial_general_203.png

