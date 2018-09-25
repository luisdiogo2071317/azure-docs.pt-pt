---
title: 'Tutorial: Integração do Azure Active Directory com o FreshDesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: ce302db74f831e67b576e4c0001f21473fd7f2e0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037529"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Integração do Azure Active Directory com o FreshDesk

Neste tutorial, saiba como integrar o FreshDesk no Azure Active Directory (Azure AD).

Integrar o FreshDesk no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao FreshDesk
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o FreshDesk (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o FreshDesk, terá dos seguintes itens:

- Uma subscrição do Azure
- Um FreshDesk início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar FreshDesk a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-freshdesk-from-the-gallery"></a>Adicionar FreshDesk a partir da Galeria

Para configurar a integração do FreshDesk no Azure AD, terá de adicionar FreshDesk a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar FreshDesk a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **FreshDesk**. Selecione **FreshDesk** no painel de resultados e, em seguida, selecione a **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o FreshDesk, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no FreshDesk a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no FreshDesk deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **endereço de e-mail** no FreshDesk.

Para configurar e testar o Azure AD início de sessão único com o FreshDesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do FreshDesk](#creating-a-freshdesk-test-user)**  - para ter um equivalente da Eduarda Almeida no FreshDesk a que está ligado à representação de ela do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de FreshDesk.

**Para configurar o Azure AD início de sessão único com o FreshDesk, execute os seguintes passos:**

1. No portal do Azure, sobre o **FreshDesk** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, como **modo** selecione **baseado em SAML início de sessão** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Sobre o **FreshDesk domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.freshdesk.com` ou qualquer outro valor Freshdesk sugeriu.

    > [!NOTE]
    > Tenha em atenção que isto não é o valor real. Terá de atualizar o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente do FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) para obter este valor.

4. Seu aplicativo espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso. O valor predefinido **identificador de utilizador** é **user.userprincipalname** mas **FreshDesk** espera que isso seja mapeado com o endereço de e-mail do utilizador. Para que pode usar **user.mail** atributo da lista ou utilize o valor do atributo adequado com base na configuração da sua organização.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_attribute.png)

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

    > [!NOTE]
    > Se tiver quaisquer problemas, veja este [link](https://support.freshdesk.com/support/discussions/topics/317543).

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_general_400.png)

7. Instale **OpenSSL** no seu sistema, se não tiver instalado no seu sistema.

8. Open **linha de comandos** e execute os seguintes comandos:

    a. Introduza `openssl x509 -inform DER -in FreshDesk.cer -out certificate.crt` valor no prompt de comando.

    > [!NOTE]
    > Aqui **FreshDesk.cer** é o certificado que transferiu do portal do Azure.

    b. Introduza `openssl x509 -noout -fingerprint -sha256 -inform pem -in certificate.crt` valor no prompt de comando. 
    
    > [!NOTE]
    > Aqui **certificate.crt** é o certificado de saída que é gerado no passo anterior.

    c. Copiar o **Thumbprint** valor e cole-o bloco de notas. Remover dois pontos do Thumbprint e obter o valor do Thumbprint final.

9. Na **configuração do FreshDesk** secção, clique em **configurar FreshDesk** para abrir a janela de início de sessão de configurar. Copie o SAML único início de sessão no URL do serviço e o URL de fim de sessão a partir da **referência rápida** secção.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

10. Numa janela do browser web diferente, inicie sessão no site da sua empresa Freshdesk como um administrador.

11. Selecione o **ícone de definições** e, no **segurança** secção, execute os seguintes passos:

    ![Início de sessão único](./media/freshdesk-tutorial/IC776770.png "início de sessão único")
  
    a. Para **único início de sessão (SSO)**, selecione **no**.

    b. Selecione **SAML SSO**.

    c. Na **URL de início de sessão de SAML** caixa de texto, colar **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure.

    d. Na **URL de fim de sessão** caixa de texto, colar **URL de fim de sessão** valor, que copiou do portal do Azure.

    e. Na **impressão digital do certificado de segurança** caixa de texto, colar **Thumbprint** valor que obteve anteriormente depois de remover a vírgula.
  
    f. Clique em **Guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_01.png) 

2. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

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

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso à caixa.

![Atribuir utilizador][200]

**Para atribuir a Eduarda Almeida para o FreshDesk, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **FreshDesk**.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do FreshDesk no painel de acesso, deve obter a página de início de sessão para obter com sessão iniciada para a sua aplicação de FreshDesk.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png