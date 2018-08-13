---
title: 'Tutorial: Integração do Azure Active Directory com o Cisco Webex | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 73e20afdcacec76482f8ebf01bf2cef2105912a6
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005529"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Integração do Azure Active Directory com o Cisco Webex

Neste tutorial, saiba como integrar o Cisco Webex com o Azure Active Directory (Azure AD).

Integrar o Cisco Webex no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Cisco Webex.
- Pode permitir que os utilizadores automaticamente obter a sessão iniciada no Cisco Webex com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Cisco Webex, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma subscrição do Cisco Webex única início de sessão-no-ativada

> [!NOTE]
> Não é recomendado utilizar um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Cisco Webex da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="add-cisco-webex-from-the-gallery"></a>Adicionar Cisco Webex a partir da Galeria
Para configurar a integração do Cisco Webex com o Azure AD, terá de adicionar Cisco Webex a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Cisco Webex a partir da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Cisco Webex**. 

5. Selecione **Cisco Webex** do painel de resultados. Em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Cisco Webex na lista de resultados](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Cisco Webex com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o utilizador de contraparte no Cisco Webex for para um utilizador no Azure AD. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e um utilizador relacionado na Cisco Webex.

Na Cisco Webex, atribua o valor **nome de utilizador** o mesmo valor como **nome de utilizador** no Azure AD. Agora que estabeleceu a ligação entre os dois usuários. 

Para configurar e testar o Azure AD início de sessão único com o Cisco Webex, conclua os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
3. [Criar um utilizador de teste do Cisco Webex](#create-a-cisco-webex-test-user) ter um equivalente da Eduarda Almeida na Cisco Webex que está ligado à representação de utilizador do Azure AD.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação Cisco Webex.

**Para configurar o Azure AD início de sessão único com o Cisco Webex, siga os passos seguintes:**

1. No portal do Azure, sobre o **Cisco Webex** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Para ativar o início de sessão único, na **início de sessão único** caixa de diálogo a **modo** na lista pendente, selecione **baseado em SAML logon**.
 
    ![Caixa de diálogo de início de sessão único](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. Numa janela do browser web diferente, inicie sessão no site da sua empresa Cisco Webex como um administrador.

4. Clique em **definições** da esquerda do menu.

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. Na página Definições de desloque-se sob a **autenticação** secção, clique em **modificar**.

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Selecione **integrar um fornecedor de identidade 3rd party. (Avançado) ** e vá para a próxima tela.

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. Sobre o **exportar metadados de diretório** página, clique em **transfira o ficheiro de metadados** para transferir o ficheiro de metadados.

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. No portal do Azure, sob o **Cisco Webex domínio e URLs** secção, carregue o transferido **ficheiro de metadados do fornecedor de serviços** e configurar a aplicação, efetuando os seguintes passos:

    a. Clique em **carregamento de ficheiro de metadados**.

    ![Cisco Webex domínio e URLs únicas início de sessão em informações](./media/cisco-webex-tutorial/tutorial_ciscowebex_upload.png)

    b. Clique em **logótipo da pasta** para selecionar o ficheiro de metadados e clique em **carregar**.

    ![Cisco Webex domínio e URLs únicas início de sessão em informações](./media/cisco-webex-tutorial/tutorial_ciscowebex_uploadconfig.png)

    c. Após a conclusão com êxito de carregamento **ficheiro de metadados do fornecedor de serviços** a **identificador** e **URL de resposta** valores ficam automaticamente preenchido no **Cisco Webex Domínio e URLs** secção caixa de texto, conforme mostrado abaixo:

    ![Cisco Webex domínio e URLs únicas início de sessão em informações](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    d. Na **URL de início de sessão** caixa, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.webex.com/`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o início de sessão real no URL. Contacte [equipa de suporte de cliente do Cisco Webex](https://www.webex.co.in/support/support-overview.html) obter esses valores.

9. Aplicação do Cisco Webex espera que as asserções SAML para conter atributos específicos. Configure os seguintes atributos para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_ciscowebex_07.png) 

10. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    |  Nome do Atributo  | Valor do Atributo |
    | --------------- | -------------------- |    
    |   FirstName    | User.givenName |
    |   Apelido    | User.Surname |
    |   UID    | User.Mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

11. Sobre o **certificado de assinatura SAML** secção, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

12. Selecione **Guardar**.

    ![Configurar o início de sessão único botão Save](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
13. Na página administrador de site de empresa da Cisco Webex, utilize a opção de browser de ficheiro para localizar e carregar o ficheiro de metadados do Azure AD. Em seguida, selecione **necessita de certificado assinado por uma autoridade de certificado nos metadados (mais seguro)** e aceda ao ecrã seguinte. 

    ![Configurar o início de sessão único](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

14. Selecione **Testar ligação de SSO**e quando abre um novo separador do browser, autenticar com o Azure AD ao iniciar sessão.

15. Retorno para o **gerenciamento de colaboração de nuvem do Cisco** separador do browser. Se o teste foi concluída com êxito, selecione **esse teste foi concluída com êxito. Ativar a opção de início de sessão único** e clique em **guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **Add** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-cisco-webex-test-user"></a>Criar um utilizador de teste do Cisco Webex

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Cisco Webex. Cisco Webex suporta just-in-time de aprovisionamento e o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial) sobre como configurar o aprovisionamento automático de utilizadores.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar o utilizador Eduarda Almeida para utilizar o Azure início de sessão único, concedendo-lhes acesso a Cisco Webex.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Cisco Webex, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Em seguida, vá para a vista de diretório e, em seguida, para **aplicações empresariais**.  

2. Selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

3. Na lista de aplicações, selecione **Cisco Webex**.

    ![A ligação de Cisco Webex na lista de aplicações](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** no **utilizadores** lista.

6. Na **utilizadores e grupos** caixa de diálogo, clique nas **selecione** botão.

7. Selecione o **atribua** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico de Cisco Webex no painel de acesso, automaticamente obter conectado à sua aplicação Cisco Webex.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

