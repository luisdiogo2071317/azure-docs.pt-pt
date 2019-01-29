---
title: 'Tutorial: Integração do Active Directory do Azure com Hackerone | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Hackerone.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: fdc9b67ef7f2becbeffe10261b3f2a96a225b5db
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159102"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Tutorial: Integração do Active Directory do Azure com HackerOne

Neste tutorial, saiba como integrar HackerOne com o Azure Active Directory (Azure AD).

Integrar HackerOne no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao HackerOne
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para HackerOne (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com HackerOne, terá dos seguintes itens:

- Uma subscrição do Azure
- Um HackerOne logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando HackerOne da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-hackerone-from-the-gallery"></a>Adicionando HackerOne da Galeria
Para configurar a integração do HackerOne com o Azure AD, terá de adicionar HackerOne a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar HackerOne a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **HackerOne**.

    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/tutorial_hackerone_search.png)

1. No painel de resultados, selecione **HackerOne**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com HackerOne com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no HackerOne a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no HackerOne deve ser estabelecido.

HackerOne, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com HackerOne, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste HackerOne](#creating-a-hackerone-test-user)**  - para ter um equivalente da Eduarda Almeida na HackerOne que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo HackerOne.

**Para configurar o Azure AD início de sessão único com HackerOne, execute os seguintes passos:**

1. No portal do Azure, sobre o **HackerOne** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_samlbase.png)

1. Sobre o **URL de início de sessão único de HackerOne e identificador** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://hackerone.com/<company name>/authentication`

    b. Na **identificador** caixa de texto, escreva um URL como:  `https://hackerone.com/users/saml/metadata`
    
    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de HackerOne](mailto:support@hackerone.com) para obter este valor. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_general_400.png)

1. Sobre o **HackerOne configuração** secção, clique em **configurar HackerOne** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_configure.png) 

1. Início de sessão no seu inquilino do HackerOne como administrador.

1. No menu na parte superior, clique a "**definições**."
   
    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_001.png) 

1. Navegue para "**autenticação**"e clique em"**adicionar definições de SAML**."
   
    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_003.png) 

1. Sobre o **definições de SAML** caixa de diálogo, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_004.png) 

    a. Na **domínio de E-Mail** caixa de texto, escreva um domínio registado.

    b. Na  **início de sessão único na URL** caixas de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    c. Abra sua **ficheiro de certificado** no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X509**  caixa de texto.
    
    d. Clique em **Guardar**.

1. Na caixa de diálogo Definições de autenticação, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_005.png) 

    a. Clique em **executar teste**.

    b. Se o valor do **Status** campo é igual a **pela última vez o status do teste: criado**, contacte seu [HackerOne a equipa de suporte](mailto:support@hackerone.com) para pedir uma revisão da sua configuração.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/hackerone-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-hackerone-test-user"></a>Criar um utilizador de teste HackerOne

Em seguida, criar um usuário chamado Eduarda Almeida no HackerOne. HackerOne suporta o aprovisionamento de just-in-time, que está ativado por predefinição.

Não existe nenhum item de ação para nesta secção. Quando acessa HackerOne, um novo utilizador é criado se não existir ainda.

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar a equipa de suporte de Certify. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para HackerOne.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a HackerOne, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **HackerOne**.

    ![Configurar o início de sessão único](./media/hackerone-tutorial/tutorial_hackerone_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Por fim, testar seu única início de sessão em configuração do Azure AD através do painel de acesso.  

Quando clica no mosaico HackerOne no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo HackerOne.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/hackerone-tutorial/tutorial_general_01.png
[2]: ./media/hackerone-tutorial/tutorial_general_02.png
[3]: ./media/hackerone-tutorial/tutorial_general_03.png
[4]: ./media/hackerone-tutorial/tutorial_general_04.png

[100]: ./media/hackerone-tutorial/tutorial_general_100.png

[200]: ./media/hackerone-tutorial/tutorial_general_200.png
[201]: ./media/hackerone-tutorial/tutorial_general_201.png
[202]: ./media/hackerone-tutorial/tutorial_general_202.png
[203]: ./media/hackerone-tutorial/tutorial_general_203.png

