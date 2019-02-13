---
title: 'Tutorial: Integração do Active Directory do Azure com os planos de 10 000 ft | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e planos de 10 000 quadrado.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb6f0645f1a12566f05b5f44688e4f86ab1b9725
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56178265"
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a>Tutorial: Integração do Active Directory do Azure com os planos de 10 000 ft

Neste tutorial, saiba como integrar os planos de 10 000 quadrado com o Azure Active Directory (Azure AD).

Planos de 10 000 ft a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao quadrado 10.000 planos
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para planos de 10 000 ft (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com planos de 10 000 quadrado, precisa do seguinte:

- Uma subscrição do Azure
- Um quadrado 10.000 planos de início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar 10.000 ft planos da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-10000ft-plans-from-the-gallery"></a>Adicionar 10.000 ft planos da Galeria
Para configurar a integração dos planos de 10 000 quadrado com o Azure AD, terá de adicionar os planos de 10 000 ft a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar planos de 10 000 ft a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **planos de 10 000 ft**.

    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. No painel de resultados, selecione **planos de 10 000 ft**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com 10.000 ft planos com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no quadrado 10.000 planos para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no quadrado 10.000 planos deve ser estabelecido.

Nos planos de 10 000 quadrado, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com planos de 10 000 quadrado, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criação de um quadrado de 10 000 planos de utilizador de teste](#creating-a-10000ft-plans-test-user)**  - para ter um equivalente da Eduarda Almeida no quadrado 10.000 planos que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo de planos de 10 000 quadrado.

**Para configurar o Azure AD início de sessão único com planos de 10 000 quadrado, execute os seguintes passos:**

1. No portal do Azure, no **planos de 10 000 ft** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. Sobre o **10.000 ft planos de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.10000ft.com`

    b. Na **identificador** caixa de texto, escreva o URL: `https://app.10000ft.com/saml/metadata`

    > [!NOTE] 
    > O valor para **identificador** é diferente se tiver um domínio personalizado. Contacte [equipa de suporte de planos de 10 000 ft](https://www.10000ft.com/plans/support) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Raw)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_general_400.png)

6. Na **quadrado 10.000 planos de configuração** secção, clique em **configurar planos de 10 000 ft** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. Para configurar o início de sessão único em **planos de 10 000 ft** lado, terá de enviar o transferido **Certificate(Raw), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [10.000 ft Equipa de suporte de planos](https://www.10000ft.com/plans/support).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/10000ftplans-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-10000ft-plans-test-user"></a>Criação de um quadrado de 10 000 planos de utilizador de teste

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida nos planos de 10 000 quadrado. Planos de 10 000 ft suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de acesso a planos de 10 000 ft se não existir ainda. 

> [!NOTE]
> Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de planos de 10 000 ft](https://www.10000ft.com/plans/support).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso aos planos de 10 000 quadrado.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida aos planos de 10 000 quadrado, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **planos de 10 000 ft**.

    ![Configurar o início de sessão único](./media/10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  
Quando clica no mosaico de planos de 10 000 quadrado no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de planos de 10 000 quadrado.
 
## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/10000ftplans-tutorial/tutorial_general_203.png

