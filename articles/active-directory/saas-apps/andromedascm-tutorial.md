---
title: 'Tutorial: Integração do Active Directory do Azure com Andromeda | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Andromeda.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fd26129a6ab8fb6082f9465be71eadcafa292db
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165202"
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda"></a>Tutorial: Integração do Active Directory do Azure com Andromeda

Neste tutorial, saiba como integrar Andromeda com o Azure Active Directory (Azure AD).

Integrar Andromeda no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Andromeda.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Andromeda (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Andromeda, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Andromeda logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Andromeda da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-andromeda-from-the-gallery"></a>Adicionando Andromeda da Galeria
Para configurar a integração do Andromeda com o Azure AD, terá de adicionar Andromeda a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Andromeda a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Andromeda**, selecione **Andromeda** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Andromeda na lista de resultados](./media/andromedascm-tutorial/tutorial_andromedascm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Andromeda com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Andromeda a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Andromeda deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Andromeda, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Andromeda](#create-an-andromeda-test-user)**  - para ter um equivalente da Eduarda Almeida na Andromeda que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Andromeda.

**Para configurar o Azure AD início de sessão único com Andromeda, execute os seguintes passos:**

1. No portal do Azure, sobre o **Andromeda** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/andromedascm-tutorial/tutorial_andromedascm_samlbase.png)

3. Sobre o **Andromeda domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Andromeda domínio e URLs únicas início de sessão em informações](./media/andromedascm-tutorial/tutorial_andromedascm_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantURL>.ngcxpress.com/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantURL>.ngcxpress.com/SAMLConsumer.aspx`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Andromeda domínio e URLs únicas início de sessão em informações](./media/andromedascm-tutorial/tutorial_andromedascm_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantURL>.ngcxpress.com/SAMLLogon.aspx`
     
    > [!NOTE] 
    > O valor anterior não é o valor real. Atualizar o valor com o identificador de real, a URL de resposta e o URL de início de sessão que é explicado mais tarde no tutorial.

5. Aplicação de Andromeda espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único attb](./media/andromedascm-tutorial/tutorial_andromedascm_attribute.png)

    > [!Important]
    > Limpar as definições de espaço de nomes ao configurá-las.
    
6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | -------------- | -------------------- |    
    | função        | Função de aplicação específica |
    | tipo        | Tipo de Aplicação |
    | Empresa       | CompanyName    |

    > [!NOTE]
    > Não existem valores reais. Estes valores são apenas para fins de demonstração, utilize as funções da organização.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/andromedascm-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/andromedascm-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

7. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/andromedascm-tutorial/tutorial_andromedascm_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/andromedascm-tutorial/tutorial_general_400.png)
    
9. Sobre o **Andromeda configuração** secção, clique em **configurar Andromeda** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_configure.png)

10. Início de sessão no site da sua empresa Andromeda como administrador.

11. Na parte superior da barra de menus, clique em **administrador** e navegue até à **administração**.

    ![Administrador de Andromeda](./media/andromedascm-tutorial/tutorial_andromedascm_admin.png)

12. No lado esquerdo da barra de ferramentas em **Interfaces** secção, clique em **configuração de SAML**.

    ![Andromeda saml](./media/andromedascm-tutorial/tutorial_andromedascm_saml.png)

13. Sobre o **configuração SAML do** secção de página, execute os seguintes passos:

    ![Andromeda config](./media/andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. Verifique **ativar o SSO com SAML**.

    b. Sob **informações Andromeda** secção, copie a **SP identidade** valor e cole-o no **identificador** caixa de texto de **Andromeda domínio e URLs** secção.

    c. Copiar o **URL de consumidor** valor e cole-o no **URL de resposta** caixa de texto de **Andromeda domínio e URLs** secção.

    d. Cópia a **URL de início de sessão** valor e cole-o para o **URL de início de sessão** caixa de texto de **Andromeda domínio e URLs** secção.

    e. Sob **fornecedor de identidade de SAML** secção, escreva o nome do IDP.

    f. Na **início de sessão único no ponto final** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que, quando copiou do portal do Azure.

    g. Abra o transferido **certificado codificado Base64** partir do portal do Azure no bloco de notas, cole-o no **X 509 certificado** caixa de texto.
    
    h. Mapear os seguintes atributos com o respetivo valor para facilitar o início de sessão do SSO do Azure AD. O **ID de utilizador** atributo é necessário para iniciar sessão. Para o aprovisionamento, **E-Mail**, **empresa**, **UserType**, e **função** são necessários. Nesta secção, vamos definir atributos de mapeamento (nome e os valores) que correlacionam para aquelas definidas no portal do Azure

    ![Andromeda attbmap](./media/andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/andromedascm-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/andromedascm-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/andromedascm-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/andromedascm-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-andromeda-test-user"></a>Criar um utilizador de teste Andromeda

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Andromeda. Andromeda suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Andromeda se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente Andromeda](https://www.ngcsoftware.com/support/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Andromeda.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Andromeda, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Andromeda**.

    ![A ligação de Andromeda na lista de aplicações](./media/andromedascm-tutorial/tutorial_andromedascm_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Andromeda no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Andromeda.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/andromedascm-tutorial/tutorial_general_01.png
[2]: ./media/andromedascm-tutorial/tutorial_general_02.png
[3]: ./media/andromedascm-tutorial/tutorial_general_03.png
[4]: ./media/andromedascm-tutorial/tutorial_general_04.png

[100]: ./media/andromedascm-tutorial/tutorial_general_100.png

[200]: ./media/andromedascm-tutorial/tutorial_general_200.png
[201]: ./media/andromedascm-tutorial/tutorial_general_201.png
[202]: ./media/andromedascm-tutorial/tutorial_general_202.png
[203]: ./media/andromedascm-tutorial/tutorial_general_203.png
