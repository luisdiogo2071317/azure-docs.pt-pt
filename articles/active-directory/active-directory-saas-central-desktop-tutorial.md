---
title: "Tutorial: Integração do Azure Active Directory com o ambiente de trabalho Central | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e o ambiente de trabalho Central."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 94c67bef7a0c6ba60fc9c7a60c79a23bf7984fb1
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Tutorial: Integração do Azure Active Directory com o ambiente de trabalho Central

Neste tutorial, irá aprender a integrar o ambiente de trabalho Central com o Azure Active Directory (Azure AD).

Integração do ambiente de trabalho Central com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ambiente de trabalho Central.
- Pode permitir aos utilizadores obter iniciada automaticamente ambiente de trabalho Central com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o ambiente de trabalho Central, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição do ambiente de trabalho Central único início de sessão em ativado

> [!NOTE]
> Não recomendamos a utilização de um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, exceto se for necessário.
- Se ainda não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário é descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de ambiente de trabalho Central de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-central-desktop-from-the-gallery"></a>Adicionar Central de ambiente de trabalho a partir da Galeria
Para configurar a integração do ambiente de trabalho Central com o Azure AD, tem de adicionar um ambiente de trabalho Central na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o ambiente de trabalho Central na galeria do, siga os passos seguintes:**

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar novas aplicações, selecione o **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **ambiente de trabalho Central**. Selecione **ambiente de trabalho Central** no painel de resultados e, em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Ambiente de trabalho central na lista de resultados](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o ambiente de trabalho Central com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo no ambiente de trabalho Central for para um utilizador no Azure AD. Por outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e um utilizador relacionado no ambiente de trabalho Central.

No ambiente de trabalho Central, dê **Username** o mesmo valor como **nome de utilizador** no Azure AD. Agora tem de estabelecer a ligação entre dois utilizadores.

Para configurar e testar o Azure AD-início de sessão único com o ambiente de trabalho Central, tem de concluir os blocos modulares seguintes:

1. [Configurar o Azure AD-início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste do ambiente de trabalho Central](#create-a-central-desktop-test-user) para ter um homólogo de Britta Simon no ambiente de trabalho Central que está ligada a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#test-single-sign-on) para verificar que a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de ambiente de trabalho Central.

**Para configurar o Azure AD-início de sessão único com o ambiente de trabalho Central, siga os passos seguintes:**

1. No portal do Azure, no **ambiente de trabalho Central** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. Para ativar o início de sessão, no **de sessão único-** caixa de diálogo a **modo** na lista pendente, selecione **baseados em SAML início de sessão**.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. No **Central de ambiente de trabalho de domínios e URLs** secção, siga os passos seguintes:

    ![Domínio de ambiente de trabalho central e os URLs únicos de informações de início de sessão](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. No **URL de início de sessão** caixa, escreva um URL com o padrão do seguinte:`https://<companyname>.centraldesktop.com`

    b. No **identificador** caixa, escreva um URL com o padrão do seguinte:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. No **URL de resposta** caixa, escreva um URL com o padrão do seguinte:`https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualizar estes valores com o identificador real, URL de resposta e início de sessão no URL. Contacte o [equipa de suporte de cliente de ambiente de trabalho Central](https://imeetcentral.com/contact-us) para obter estes valores. 

4. No **certificado de assinatura de SAML** secção, selecione **certificado**. Em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Selecione o botão **Guardar**.

    ![Configurar o início de sessão de gravação botão](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. No **configuração de ambiente de trabalho Central** secção, selecione **configurar o ambiente de trabalho Central** para abrir o **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **referência rápida** secção.

    ![Configuração de ambiente de trabalho central](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Inicie sessão no seu **ambiente de trabalho Central** inquilino.

8. Aceda a **definições**. Selecione **avançadas**e, em seguida, selecione **início de sessão único**.

    ![Configuração - avançada](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "configuração - avançada")

9. No **início de sessão único em definições** página, siga os passos seguintes:

    ![Único as definições de início de sessão](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "início de sessão único em definições")
    
    a. Selecione **ativar SAML v2 início de sessão único**.
    
    b. No **SSO URL** caixa, cole o **ID de entidade de SAML** valor que copiou do portal do Azure.
    
    c. No **URL de início de sessão de SSO** caixa, cole o **único início de sessão no URL do serviço SAML** valor que copiou do portal do Azure.
    
    d. No **URL de fim de sessão do SSO** caixa, cole o **Sign-Out URL** valor que copiou do portal do Azure.

10. No **método de verificação de assinatura de mensagem** secção, siga os passos seguintes:

    ![Método de verificação de assinatura da mensagem](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "método de verificação de assinatura de mensagem") um. Selecione **certificado**.
    
    b. No **SSO certificado** lista, selecione **RSH SHA256**.
    
    c. Abra o seu certificado transferido no bloco de notas. Em seguida, copie o conteúdo de certificado e cole-o para o **SSO certificado** campo.
        
    d. Selecione **apresentar uma ligação à sua página de início de sessão SAMLv2**.
    
    e. Selecione **atualização**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporados documentação através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados em [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-central-desktop-test-user"></a>Criar um utilizador de teste do ambiente de trabalho Central

Para utilizadores do Azure AD conseguir iniciar sessão, têm de ser aprovisionados na aplicação de ambiente de trabalho Central. Esta secção descreve como criar contas de utilizador do Azure AD no ambiente de trabalho Central.

> [!NOTE]
> Aprovisionar contas de utilizador do Azure AD, pode utilizar quaisquer outras ferramentas de criação de conta de utilizador de ambiente de trabalho Central ou APIs fornecidas pelo ambiente de trabalho Central.

**Aprovisionar contas de utilizador ao ambiente de trabalho Central:**

1. Inicie sessão no seu inquilino de ambiente de trabalho Central.

2. Aceda a **pessoas** > **membros internos**.

3. Selecione **adicionar membros internos**.

    ![As pessoas](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "pessoas")
    
4. No **endereço de correio eletrónico de novos membros** caixa, escreva uma conta do Azure AD que pretende aprovisionar e, em seguida, selecione **seguinte**.

    ![Endereços de novos membros de correio eletrónico](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "endereços dos novos membros de E-Mail")

5. Selecione **membro ou membros adicionar interno**.

    ![Adicionar membro de interno](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "membro interno adicionar")
   
   >[!NOTE]
   >Os utilizadores que adicionar recebem uma mensagem de e-mail que inclui uma ligação de confirmação para a ativação das respetivas contas.
   
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo-lhes acesso ao ambiente de trabalho Central.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon ao ambiente de trabalho Central, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Ir para a vista de diretório e, em seguida, aceda a **aplicações empresariais**.

2. Selecione **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **ambiente de trabalho Central**.

    ![A ligação de ambiente de trabalho Central na lista de aplicações](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** no **utilizadores** lista.

6. No **utilizadores e grupos** caixa de diálogo, clique em de **selecione** botão.

7. No **adicionar atribuição** caixa de diálogo, selecione o **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, teste a configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico de ambiente de trabalho Central no painel de acesso, que automaticamente obter sessão na sua aplicação de ambiente de trabalho Central.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_203.png

