---
title: 'Tutorial: Integração do Active Directory do Azure com o Cisco Spark | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Cisco Spark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87161c79bc58387d97863581675bb49f1e09160f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191559"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Tutorial: Integração do Active Directory do Azure com o Cisco Spark

Neste tutorial, saiba como integrar o Cisco Spark no Azure Active Directory (Azure AD).
Integrar o Cisco Spark no Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso ao Cisco Spark.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada ao Spark Cisco (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Cisco Spark, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Cisco Spark logon único habilitado subscrição

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* O Spark Cisco suporta **SP** iniciada SSO

## <a name="adding-cisco-spark-from-the-gallery"></a>Adicionar Cisco Spark a partir da Galeria

Para configurar a integração do Cisco Spark no Azure AD, terá de adicionar Cisco Spark a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Cisco Spark a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **Cisco Spark**, selecione **Cisco Spark** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![Cisco Spark na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Spark Cisco, com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único funcionar, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Cisco Spark deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Cisco Spark, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar Cisco Spark início de sessão único](#configure-cisco-spark-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
3. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Criar utilizador de teste do Cisco Spark](#create-cisco-spark-test-user)**  - para ter um equivalente da Eduarda Almeida no Spark, Cisco que está ligada à representação de utilizador do Azure AD.
6. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único com o Cisco Spark, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **Cisco Spark** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![Cisco Spark domínio e URLs únicas início de sessão em informações](common/sp-identifier.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL como: `https://web.ciscospark.com/#/signin`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://idbroker.webex.com/<companyname>`

    > [!NOTE]
    > Este valor de identificador não é real. Atualize este valor com o identificador real. Contacte [equipa de suporte de cliente do Cisco Spark](https://support.ciscospark.com/) obter esses valores. Também pode consultar os padrões mostrados a **configuração básica de SAML** secção no portal do Azure.

5. Aplicação de Spark do Cisco espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](common/edit-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Name |  Atributo de origem|
    | ---------------|--------- |
    | uid | user.userprincipalname |

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Deixe o **espaço de nomes** em branco.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Guardar**.

7. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **XML de metadados de Federação**  entre as opções de determinado de acordo com seus requisitos e guarde-o no seu computador.

    ![O link de download de certificado](common/metadataxml.png)

8. Sobre o **configurar Cisco Spark** secção, copie os URLs apropriados de acordo com seus requisitos.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de início de sessão

    b. Azure Ad Identifier

    c. URL de fim de sessão

### <a name="configure-cisco-spark-single-sign-on"></a>Configurar Cisco Spark início de sessão único

1. Inicie sessão no [gerenciamento de colaboração de nuvem do Cisco](https://admin.ciscospark.com/) com as suas credenciais de administrador completo.

2. Selecione **configurações** e, no **autenticação** secção, clique em **modificar**.

    ![Configurar o início de sessão único](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
  
3. Selecione **integrar um fornecedor de identidade 3rd party. (Avançado)**  e vá para a próxima tela.

4. Sobre o **importar metadados de Idp** página, qualquer um dos arrastar e soltar o arquivo de metadados do Azure AD para a página ou utilize a opção de browser de ficheiro para localizar e carregar o ficheiro de metadados do Azure AD. Em seguida, selecione **necessita de certificado assinado por uma autoridade de certificado nos metadados (mais seguro)** e clique em **próxima**.

    ![Configurar o início de sessão único](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

5. Selecione **Testar ligação de SSO**e quando abre um novo separador do browser, autenticar com o Azure AD ao iniciar sessão.

6. Retorno para o **gerenciamento de colaboração de nuvem do Cisco** separador do browser. Se o teste foi concluída com êxito, selecione **esse teste foi concluída com êxito. Ativar a opção de início de sessão único** e clique em **próxima**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](common/users.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Novo utilizador botão](common/new-user.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![A caixa de diálogo de utilizador](common/user-properties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao Cisco Spark.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **Cisco Spark**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicações, selecione **Cisco Spark**.

    ![A ligação de Spark do Cisco, na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-cisco-spark-test-user"></a>Criar utilizador de teste do Cisco Spark

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Cisco Spark. Nesta secção, vai criar um usuário chamado Eduarda Almeida no Cisco Spark.

1. Vá para o [gerenciamento de colaboração de nuvem do Cisco](https://admin.ciscospark.com/) com as suas credenciais de administrador completo.

2. Clique em **usuários** e, em seguida **gerir utilizadores**.
   
    ![Configurar o início de sessão único](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Na **gerir utilizadores** janela, selecione **manualmente adicionar ou modificar utilizadores** e clique em **seguinte**.

4. Selecione **nomes e o endereço de E-Mail**. Em seguida, preencha a caixa de texto da seguinte forma:

    ![Configurar o início de sessão único](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. Na **nome próprio** caixa de texto, nome de utilizador, como o tipo **Eduarda**.

    b. Na **sobrenome** caixa de texto, tipo último nome de utilizador, como **Simon**.

    c. Na **endereço de E-Mail** caixa de texto, como o tipo de endereço de e-mail do utilizador **britta.simon@contoso.com**.

5. Clique no sinal para adicionar a Eduarda Almeida. Clique depois em **Seguinte**.

6. Na **adicionar serviços para os utilizadores** janela, clique em **guardar** e, em seguida **concluir**.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Spark do Cisco, no painel de acesso, deve ser automaticamente conectado para o Spark Cisco para o qual configura o SSO. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
