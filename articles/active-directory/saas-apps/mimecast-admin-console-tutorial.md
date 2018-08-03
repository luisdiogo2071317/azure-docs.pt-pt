---
title: 'Tutorial: Integração do Azure Active Directory com a consola de administração de Mimecast | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e a consola de administração de Mimecast.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: ce4142c5b4a20886a94c87699f262f7238fc2cb4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438574"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Tutorial: Integração do Azure Active Directory com a consola de administração de Mimecast

Neste tutorial, saiba como integrar a consola de administração de Mimecast com o Azure Active Directory (Azure AD).

Consola de administração de Mimecast a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à consola de administração de Mimecast.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada a consola de administração de Mimecast (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a consola de administração do Mimecast, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma consola de administração de Mimecast logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Mimecast consola de administração da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Adicionando Mimecast consola de administração da Galeria
Para configurar a integração Mimecast da consola de administração para o Azure AD, terá de adicionar a consola de administração do Mimecast a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a consola de administração de Mimecast partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **consola de administração de Mimecast**, selecione **consola de administração de Mimecast** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Consola de administração de Mimecast na lista de resultados](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a consola de administração de Mimecast com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na consola de administração de Mimecast a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na consola de administração de Mimecast deve ser estabelecido.

Na consola de administração de Mimecast, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com a consola de administração de Mimecast, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste da consola de administração de Mimecast](#create-a-mimecast-admin-console-test-user)**  - para ter um equivalente da Eduarda Almeida na consola de administração de Mimecast que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de consola de administração de Mimecast.

**Para configurar o Azure AD início de sessão único com a consola de administração de Mimecast, execute os seguintes passos:**

1. No portal do Azure, sobre o **consola de administração de Mimecast** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

1. Sobre o **Mimecast domínio da consola de administração e URLs** secção, execute os seguintes passos:

    ![Mimecast domínio da consola de administração e URLs únicas início de sessão em informações](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > O URL de início de sessão é específico de região.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/mimecast-admin-console-tutorial/tutorial_general_400.png)

1. Sobre o **Mimecast configuração da consola de administração** secção, clique em **configurar a consola de administração de Mimecast** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração da consola de administração de Mimecast](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

1. Numa janela do browser web diferente, inicie sessão na consola de administração de Mimecast como administrador.

1. Aceda a **serviços \> aplicação**.

    ![Os serviços](./media/mimecast-admin-console-tutorial/ic794998.png "serviços")

1. Clique em **perfis de autenticação**.

    ![Perfis de autenticação](./media/mimecast-admin-console-tutorial/ic794999.png "perfis de autenticação")
    
1. Clique em **novo perfil de autenticação**.

    ![Novos perfis de autenticação](./media/mimecast-admin-console-tutorial/ic795000.png "novos perfis de autenticação")

1. Na **perfil de autenticação** secção, execute os seguintes passos:

    ![Perfil de autenticação](./media/mimecast-admin-console-tutorial/ic795015.png "perfil de autenticação")
    
    a. Na **Descrição** caixa de texto, escreva um nome para a sua configuração.
    
    b. Selecione **impor a autenticação de SAML para a consola de administração de Mimecast**.
    
    c. Como **fornecedor**, selecione **Azure Active Directory**.
    
    d. Colar **ID de entidade de SAML**, que copiou do portal do Azure para o **URL de emissor** caixa de texto.
    
    e. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **URL de início de sessão** caixa de texto.

    f. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **URL de fim de sessão** caixa de texto.
    
    >[!NOTE]
    >O valor de URL de início de sessão e o valor de URL de fim de sessão são para a consola de administração Mimecast o mesmo.
    
    g. Abra o certificado de base 64 transferido a partir do portal do Azure no bloco de notas, remova a primeira linha ("*--*") e a última linha ("*--*"), copie os conteúdos restantes-lo em seu área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade (metadados)** caixa de texto.
    
    h. Selecione **permitir o início de sessão único em**.
    
    i. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/mimecast-admin-console-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/mimecast-admin-console-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/mimecast-admin-console-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Criar um utilizador de teste da consola de administração de Mimecast

Para habilitar os utilizadores do Azure AD iniciar sessão na consola de administração de Mimecast, tem de ser aprovisionados na consola de administração de Mimecast. No caso da consola de administração de Mimecast, aprovisionamento é uma tarefa manual.

* É necessário registrar um domínio, antes de poder criar os utilizadores.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **consola de administração de Mimecast** como administrador.
1. Aceda a **diretórios \> interno**.
   
   ![Diretórios](./media/mimecast-admin-console-tutorial/ic795003.png "diretórios")
1. Clique em **registar o novo domínio**.
   
   ![Registar o novo domínio](./media/mimecast-admin-console-tutorial/ic795004.png "registar o novo domínio")
1. Depois de ter sido criado o seu novo domínio, clique em **novo endereço**.
   
   ![Novo endereço](./media/mimecast-admin-console-tutorial/ic795005.png "novo endereço")
1. Na caixa de diálogo novo endereço, execute os seguintes passos:
   
   ![Guarde](./media/mimecast-admin-console-tutorial/ic795006.png "guardar")
   
   a. Tipo de **endereço de E-Mail**, **nome Global**, **palavra-passe**, e **Confirmar palavra-passe** atributos de um válido do Azure AD da conta pretende Aprovisionar em caixas de texto relacionadas.

   b. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de consola de administração de Mimecast ou APIs fornecidas pela consola de administração de Mimecast para aprovisionar contas de utilizador do Azure AD. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à consola de administração de Mimecast.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida à consola de administração de Mimecast, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **consola de administração de Mimecast**.

    ![A ligação da consola de administração de Mimecast na lista de aplicações](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da consola de administração de Mimecast no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de consola de administração de Mimecast.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-admin-console-tutorial/tutorial_general_203.png

