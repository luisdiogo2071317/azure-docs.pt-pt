---
title: 'Tutorial: Integração do Active Directory do Azure com AnswerHub | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3bced675ad074de7050aee08d2a63b4831ba3606
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810351"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Tutorial: Integração do Active Directory do Azure com AnswerHub

Neste tutorial, saiba como integrar AnswerHub com o Azure Active Directory (Azure AD).

Integrar AnswerHub no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao AnswerHub
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para AnswerHub (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com AnswerHub, terá dos seguintes itens:

- Uma subscrição do Azure
- Um AnswerHub logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando AnswerHub da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-answerhub-from-the-gallery"></a>Adicionando AnswerHub da Galeria
Para configurar a integração do AnswerHub com o Azure AD, terá de adicionar AnswerHub a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar AnswerHub a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **AnswerHub**.

    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/tutorial_answerhub_search.png)

5. No painel de resultados, selecione **AnswerHub**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com AnswerHub com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no AnswerHub a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no AnswerHub deve ser estabelecido.

AnswerHub, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com AnswerHub, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste AnswerHub](#creating-an-answerhub-test-user)**  - para ter um equivalente da Eduarda Almeida na AnswerHub que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo AnswerHub.

**Para configurar o Azure AD início de sessão único com AnswerHub, execute os seguintes passos:**

1. No portal do Azure, sobre o **AnswerHub** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Sobre o **AnswerHub domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_answerhub_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<company>.answerhub.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente AnswerHub](mailto:success@answerhub.com) obter esses valores. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_general_400.png)

6. Sobre o **AnswerHub configuração** secção, clique em **configurar AnswerHub** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_answerhub_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa AnswerHub como um administrador.
   
    >[!NOTE]
    >Se precisar de ajuda para configurar AnswerHub, contacte [equipa de suporte do AnswerHub](mailto:success@answerhub.com.).
   
8. Aceda a **administração**.

9. Clique nas **utilizadores e grupos** separador.

10. No painel de navegação no lado esquerdo, no **definições de redes sociais** secção, clique em **configuração de SAML**.

11. Clique em **configuração do IDP** separador.

12. Sobre o **configuração do IDP** separador, execute os seguintes passos:

     ![Configuração SAML](./media/answerhub-tutorial/ic785172.png "configuração SAML")  
  
     a. Na **URL de início de sessão do IDP** caixa de texto, colar **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
  
     b. Na **URL de fim de sessão do IDP** caixa de texto, colar **URL de fim de sessão** valor que copiou do portal do Azure.
     
     c. Na **formato de identificador de nome de IDP** caixa de texto, introduza o utilizador identificador valor mesmo como selecionado no portal do Azure na **atributos de utilizador** secção.
  
     d. Clique em **chaves e certificados**.

13. No separador chaves e certificados, execute os seguintes passos:
    
     ![Chaves e certificados](./media/answerhub-tutorial/ic785173.png "chaves e certificados")  
 
     a. Abra o seu certificado codificado de base 64 que transferiu a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **IDP chave pública (x 509 formato)** caixa de texto.
  
     b. Clique em **Guardar**.

14. Sobre o **configuração do IDP** separador, clique em **guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/answerhub-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-answerhub-test-user"></a>Criar um utilizador de teste AnswerHub

Para ativar a utilizadores do Azure AD iniciar sessão no AnswerHub, tem de ser aprovisionados em AnswerHub.  
No caso de AnswerHub, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **AnswerHub** site da empresa como administrador.

2. Aceda a **administração**.

3. Clique nas **utilizadores e grupos** separador.

4. No painel de navegação no lado esquerdo, no **gerir utilizadores** secção, clique em **criar ou importar utilizadores**.
   
   ![Os utilizadores e grupos](./media/answerhub-tutorial/ic785175.png "utilizadores e grupos")

5. Tipo de **endereço de E-Mail**, **nome de utilizador** e **palavra-passe** de uma conta válida do Azure Active Directory que pretende aprovisionar em caixas de texto relacionadas e, em seguida, clique em  **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras AnswerHub utilizador conta criação ferramentas ou APIs fornecidas pelo AnswerHub para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para AnswerHub.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a AnswerHub, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **AnswerHub**.

    ![Configurar o início de sessão único](./media/answerhub-tutorial/tutorial_answerhub_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico AnswerHub no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo AnswerHub.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/answerhub-tutorial/tutorial_general_01.png
[2]: ./media/answerhub-tutorial/tutorial_general_02.png
[3]: ./media/answerhub-tutorial/tutorial_general_03.png
[4]: ./media/answerhub-tutorial/tutorial_general_04.png

[100]: ./media/answerhub-tutorial/tutorial_general_100.png

[200]: ./media/answerhub-tutorial/tutorial_general_200.png
[201]: ./media/answerhub-tutorial/tutorial_general_201.png
[202]: ./media/answerhub-tutorial/tutorial_general_202.png
[203]: ./media/answerhub-tutorial/tutorial_general_203.png

