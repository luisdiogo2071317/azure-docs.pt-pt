---
title: 'Tutorial: Integração do Azure Active Directory com SuccessFactors | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SuccessFactors.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 8401462ed4bf5ef2ac1ff10cf3f6750cbed7b4e5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050806"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Tutorial: Integração do Azure Active Directory com SuccessFactors

Neste tutorial, saiba como integrar SuccessFactors com o Azure Active Directory (Azure AD).

Integrar SuccessFactors no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SuccessFactors.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SuccessFactors (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SuccessFactors, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SuccessFactors início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SuccessFactors da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-successfactors-from-the-gallery"></a>Adicionando SuccessFactors da Galeria
Para configurar a integração do SuccessFactors com o Azure AD, terá de adicionar SuccessFactors a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SuccessFactors a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **SuccessFactors**, selecione **SuccessFactors** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![SuccessFactors na lista de resultados](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com SuccessFactors com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SuccessFactors a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SuccessFactors deve ser estabelecido.

SuccessFactors, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com SuccessFactors, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste SuccessFactors](#create-a-successfactors-test-user)**  - para ter um equivalente da Eduarda Almeida na SuccessFactors que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo SuccessFactors.

**Para configurar o Azure AD início de sessão único com SuccessFactors, execute os seguintes passos:**

1. No portal do Azure, sobre o **SuccessFactors** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/successfactors-tutorial/tutorial_successfactors_samlbase.png)

3. Sobre o **SuccessFactors domínio e URLs** secção, execute os seguintes passos:

    ![SuccessFactors domínio e URLs únicas início de sessão em informações](./media/successfactors-tutorial/tutorial_successfactors_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente SuccessFactors](https://www.successfactors.com/en_us/support.html) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/successfactors-tutorial/tutorial_general_400.png)
    
6. Sobre o **SuccessFactors configuração** secção, clique em **configurar SuccessFactors** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/successfactors-tutorial/tutorial_successfactors_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no seu **portal de administração de SuccessFactors** como administrador.
    
8. Visite **segurança de aplicativos** e nativo **início de sessão único num recurso**. 

9. Coloque qualquer valor na **repor Token** e clique em **guardar Token** para ativar o SAML SSO.
   
    ![Configurar o início de sessão único no lado de aplicação][11]

    > [!NOTE] 
    > Este valor é utilizado como o comutador liga/desliga. Se qualquer valor é guardado, o SAML SSO está ON. Se um valor em branco é guardado o SAML SSO está OFF.

10. Nativo para captura de ecrã abaixo e efetue as seguintes ações:
   
    ![Configurar o início de sessão único no lado de aplicação][12]
   
    a. Selecione o **SAML SSO de v2** botão de opção
   
    b. Definir o **SAML declarando nome**(por exemplo, o emissor SAML + o nome da empresa).
   
    c. Na **URL de emissor** caixa de texto, colar a **ID de entidade de SAML** valor que copiou do portal do Azure.
   
    d. Selecione **resposta (cliente gerados/IdP/AP)** como **exigir que a assinatura obrigatória**.
   
    e. Selecione **habilitado** como **ativar o sinalizador SAML**.
   
    f. Selecione **não** como **assinatura do pedido de início de sessão (SF gerados/SP/RP)**.
   
    g. Selecione **perfil de Browser/pós** como **perfil SAML**.
   
    h. Selecione **não** como **impor período válido de certificado**.
   
    i. Copie o conteúdo do ficheiro de certificado transferido a partir do portal do Azure e, em seguida, cole-o para o **certificado de verificação SAML** caixa de texto.

    > [!NOTE] 
    > O conteúdo do certificado tem de ter começar etiquetas de certificado do certificado e de fim.

11. Navegue para a V2 de SAML e, em seguida, execute os seguintes passos:
   
    ![Configurar o início de sessão único no lado de aplicação][13]
   
    a. Selecione **Sim** como **suporta a fim de sessão Global iniciado por SP**.
   
    b. Na **Global URL do serviço fim de sessão (destino LogoutRequest)** caixa de texto, colar a **URL de fim de sessão** valor que copiou do portal do Azure de formulário.
   
    c. Selecione **não** como **exigir sp tem de encriptar todos os NameID elemento**.
   
    d. Selecione **não especificado** como **formato NameID**.
   
    e. Selecione **Sim** como **Enable sp iniciada pelo início de sessão (AuthnRequest)**.
   
    f. Na **pedido de envio como o emissor de toda a empresa** caixa de texto, colar **SAML único início de sessão no URL do serviço** valor que copiou do portal do Azure.

12. Execute estes passos para tornar os nomes de utilizador de início de sessão maiúsculas de minúsculas.
   
    ![Configurar o início de sessão único][29]
    
    a. Visite **definições da empresa**(perto da parte inferior).
   
    b. Selecione a caixa de verificação junto **ativar o nome de utilizador não-diferencia maiúsculas de minúsculas**.
   
    c.Click **guardar**.
   
    > [!NOTE] 
    > Se tentar ativar esta opção, o sistema verifica se cria um nome de início de sessão SAML duplicado. Por exemplo, se o cliente tem nomes de utilizador utilizador1 e o user1. Tirando as maiúsculas e minúsculas torna esses duplicados. O sistema dá-lhe uma mensagem de erro e não ativar a funcionalidade. O cliente tem de alterar uma dos nomes de utilizador, de modo que está escrito diferentes.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/successfactors-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/successfactors-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/successfactors-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/successfactors-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-successfactors-test-user"></a>Criar um utilizador de teste SuccessFactors

Para ativar a utilizadores do Azure AD iniciar sessão no SuccessFactors, tem de ser aprovisionados em SuccessFactors.  
No caso de SuccessFactors, o aprovisionamento é uma tarefa manual.

Para obter os utilizadores criados no SuccessFactors, terá de contactar o [equipa de suporte de SuccessFactors](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para SuccessFactors.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a SuccessFactors, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SuccessFactors**.

    ![A ligação de SuccessFactors na lista de aplicações](./media/successfactors-tutorial/tutorial_successfactors_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico SuccessFactors no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo SuccessFactors.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/successfactors-tutorial/tutorial_general_01.png
[2]: ./media/successfactors-tutorial/tutorial_general_02.png
[3]: ./media/successfactors-tutorial/tutorial_general_03.png
[4]: ./media/successfactors-tutorial/tutorial_general_04.png

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/successfactors-tutorial/tutorial_general_05.png
[15]: ./media/successfactors-tutorial/tutorial_general_06.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/successfactors-tutorial/tutorial_general_100.png

[200]: ./media/successfactors-tutorial/tutorial_general_200.png
[201]: ./media/successfactors-tutorial/tutorial_general_201.png
[202]: ./media/successfactors-tutorial/tutorial_general_202.png
[203]: ./media/successfactors-tutorial/tutorial_general_203.png

