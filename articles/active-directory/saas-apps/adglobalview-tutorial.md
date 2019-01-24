---
title: 'Tutorial: Integração do Active Directory do Azure com ADP Globalview | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ADP Globalview.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 8ac0856dcadb92b79090a6520ef29707fd7a3bae
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813683"
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Tutorial: Integração do Active Directory do Azure com ADP Globalview

Neste tutorial, saiba como integrar ADP Globalview com o Azure Active Directory (Azure AD).

Integrar ADP Globalview no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ADP Globalview
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ADP Globalview (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ADP Globalview, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ADP Globalview logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ADP Globalview da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-adp-globalview-from-the-gallery"></a>Adicionando ADP Globalview da Galeria
Para configurar a integração do ADP Globalview com o Azure AD, terá de adicionar ADP Globalview a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ADP Globalview a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **ADP Globalview**.

    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. No painel de resultados, selecione **ADP Globalview**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com ADP Globalview com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ADP Globalview a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ADP Globalview deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no ADP Globalview.

Para configurar e testar o Azure AD início de sessão único com ADP Globalview, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ADP Globalview](#creating-an-adp-globalview-test-user)**  - para ter um equivalente da Eduarda Almeida na ADP Globalview que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ADP Globalview.

**Para configurar o Azure AD início de sessão único com ADP Globalview, execute os seguintes passos:**

1. No portal do Azure, sobre o **ADP Globalview** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. Sobre o **ADP Globalview domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_url.png)

     Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.globalview.adp.com/federate` ou `https://<subdomain>.globalview.adp.com/federate2`

    > [!NOTE] 
    > O valor não é real. Atualize o valor com o identificador real. Contacte [ADP Globalview suporte](https://www.adp.com/contact-us/overview.aspx) para obter o valor.
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. O aplicativo ADP GlobalView espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. 

6. Captura de ecrã seguinte mostra um exemplo para o mesmo. Os nomes de afirmação sempre ser **"PersonImmutableID"** e o valor dos quais podemos mapeou para ExtensionAttribute2, que contém o campo IDdeEmpregado do utilizador. Aqui o mapeamento de utilizador do Azure AD para ADP GlobalView é feito a EmployeeID, mas pode mapeá-lo para um valor diferente que também com base nas suas definições de aplicação. Pode trabalhar com a equipe de ADP GlobalView primeiro para utilizar o identificador correto de um usuário e mapear esse valor com o **"PersonImmutableID"** de afirmação. Também pode mapear a afirmação de ID de utilizador e E-Mail, conforme mostrado na imagem.

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | personalimmutableid | user.extensionattribute2 |
    | e-mail               | user.mail |
    | ID de utilizador              | user.userprincipalname|
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

    > [!NOTE] 
    > Antes de poder configurar a asserção de SAML, terá de contactar o seu [ADP Globalview suporte](https://www.adp.com/contact-us/overview.aspx) e pedir o valor do atributo de identificador exclusivo para o seu inquilino. Vai precisar deste valor para configurar a afirmação personalizada para a sua aplicação. 

8. Sobre o **ADP Globalview configuração** secção, clique em **configurar Globalview de ADP** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_general_400.png)

10. Para configurar o início de sessão único num **ADP Globalview** lado, terá de enviar o transferido **certificado (Base64)**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [ADP Globalview suporte](https://www.adp.com/contact-us/overview.aspx).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/create_aaduser_01.png) 

2.  Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/adglobalview-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-adp-globalview-test-user"></a>Criar um utilizador de teste ADP Globalview

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no ADP GlobalView. Trabalhar com [ADP Globalview suporte](https://www.adp.com/contact-us/overview.aspx) para adicionar os utilizadores na conta ADP GlobalView. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ADP Globalview.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a ADP Globalview, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ADP Globalview**.

    ![Configurar o início de sessão único](./media/adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.  

Ao clicar no mosaico ADP GlobalView no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ADP GlobalView.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/adglobalview-tutorial/tutorial_general_203.png

