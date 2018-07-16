---
title: 'Tutorial: Integração do Azure Active Directory da Palo Alto Networks - GlobalProtect | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e da Palo Alto Networks - GlobalProtect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 03bef6f2-3ea2-4eaa-a828-79c5f1346ce5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 6f395897687235f0956928fd0a5dccf00d4c7d12
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041046"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---globalprotect"></a>Tutorial: Integração do Azure Active Directory da Palo Alto Networks - GlobalProtect

Neste tutorial, saiba como integrar da Palo Alto Networks - GlobalProtect com o Azure Active Directory (Azure AD).

Integração da Palo Alto Networks - GlobalProtect com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso da Palo Alto Networks - GlobalProtect.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Palo Alto Networks - GlobalProtect (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com da Palo Alto Networks - GlobalProtect, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Redes da Palo Alto - GlobalProtect logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição da Palo Alto Networks - GlobalProtect a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-palo-alto-networks---globalprotect-from-the-gallery"></a>Adição da Palo Alto Networks - GlobalProtect a partir da Galeria
Para configurar a integração da Palo Alto Networks - GlobalProtect com o Azure AD, terá de adicionar da Palo Alto Networks - GlobalProtect partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar da Palo Alto Networks - GlobalProtect a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Palo Alto Networks - GlobalProtect**, selecione **Palo Alto Networks - GlobalProtect** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação .

    ![Da Palo Alto Networks - GlobalProtect na lista de resultados](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único da Palo Alto Networks - GlobalProtect com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte da Palo Alto Networks - GlobalProtect a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado da Palo Alto Networks - GlobalProtect deve ser estabelecido.

Da Palo Alto Networks - GlobalProtect, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único da Palo Alto Networks - GlobalProtect, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar da Palo Alto Networks - utilizador de teste GlobalProtect](#create-a-palo-alto-networks---globalprotect-test-user)**  - para ter um equivalente da Eduarda Almeida na Palo Alto Networks - GlobalProtect que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único nas redes da Palo Alto - GlobalProtect aplicação.

**Para configurar o Azure AD início de sessão único da Palo Alto Networks - GlobalProtect, execute os seguintes passos:**

1. No portal do Azure, sobre o **Palo Alto Networks - GlobalProtect** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_samlbase.png)

3. Sobre o **Palo Alto Networks - GlobalProtect domínio e URLs** secção, execute os seguintes passos:

    ![Da Palo Alto Networks - GlobalProtect domínio e URLs únicas início de sessão em informações](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<Customer Firewall URL>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [Palo Alto Networks - equipa de suporte de cliente GlobalProtect](https://support.paloaltonetworks.com/support) obter esses valores. 
 
4. Da Palo Alto Networks - GlobalProtect aplicativo espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_attribute.png)
    
5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | --- | --- |    
    | o nome de utilizador | user.userprincipalname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha. Podemos mapeou o valor com user.userprincipalname como exemplo, mas pode mapear com o valor adequado. 
    
    d. Clique em **Ok**


6. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/paloaltoglobalprotect-tutorial/tutorial_general_400.png)

8. Abra a interface de Usuário de administrador de Firewall de redes da Palo Alto como um administrador na outra janela do browser.

9. Clique em **dispositivo**.

    ![Configurar da Palo Alto início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Selecione **fornecedor de identidade de SAML** no painel de navegação esquerda da barra e clique em "Importar" para importar o ficheiro de metadados.

    ![Configurar da Palo Alto início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Executar as ações na janela de importação seguintes

    ![Configurar da Palo Alto início de sessão único](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. No **nome do perfil** caixa de texto, forneça um por exemplo, de nome GlobalProtect do Azure AD.
    
    b. Na **metadados de fornecedor de identidade**, clique em **procurar** e selecione o arquivo METADATA XML que transferiu a partir do portal do Azure
    
    c. Clique em **OK**

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/paloaltoglobalprotect-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/paloaltoglobalprotect-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/paloaltoglobalprotect-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/paloaltoglobalprotect-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-palo-alto-networks---globalprotect-test-user"></a>Criar da Palo Alto Networks - GlobalProtect utilizador de teste

Da Palo Alto Networks - GlobalProtect suporta Just-in-time aprovisionamento de utilizadores para que um usuário será automaticamente criado no sistema após a autenticação com êxito se já não existe. Não precisa de efetuar qualquer ação aqui. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Palo Alto Networks - GlobalProtect.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para Palo Alto Networks - GlobalProtect, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Palo Alto Networks - GlobalProtect**.

    ![Da Palo Alto Networks - GlobalProtect link na lista de aplicações](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoglobal_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica em da Palo Alto Networks - mosaico GlobalProtect no painel de acesso, deve obter automaticamente com sessão iniciada para as suas redes da Palo Alto - GlobalProtect aplicação.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoglobalprotect-tutorial/tutorial_general_203.png

