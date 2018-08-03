---
title: 'Tutorial: Integração do Azure Active Directory com o SAP HANA | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAP HANA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: e498b0ca4b9efe09c2fe2f2bfcdcb3cc68b9c2c4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430258"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Tutorial: Integração do Azure Active Directory com o SAP HANA

Neste tutorial, saiba como integrar o SAP HANA no Azure Active Directory (Azure AD).

Ao integrar o SAP HANA no Azure AD, obtém os seguintes benefícios:

- Pode controlar no Azure AD que tenha acesso ao SAP HANA.
- Pode permitir que os utilizadores automaticamente é feita para o SAP HANA com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP HANA, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição de SAP HANA que é o início de sessão único (SSO) ativado
- Uma instância HANA em execução no IaaS qualquer pública, no local, VMS do Azure ou instâncias grandes do SAP no Azure
- A interface da web de administração de XSA, bem como HANA Studio instalado na instância HANA

> [!NOTE]
> Recomendamos que não utilize um ambiente de produção do SAP HANA para testar os passos neste tutorial. Teste a integração primeiro no desenvolvimento ou ambiente de teste do aplicativo e, em seguida, utilize o ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- [Obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/) do Azure AD, se ainda não tiver um ambiente de avaliação do Azure AD.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SAP HANA da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="add-sap-hana-from-the-gallery"></a>Adicionar o SAP HANA a partir da Galeria
Para configurar a integração do SAP HANA no Azure AD, adicione SAP HANA a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o SAP HANA a partir da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar a nova aplicação, selecione o **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **SAP HANA**. Em seguida, selecione **SAP HANA** do painel de resultados. Por fim, selecione o **adicionar** botão para adicionar a aplicação. 

    ![A nova aplicação](./media/saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o SAP HANA com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o usuário equivalente no SAP HANA for para um utilizador no Azure AD. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado no SAP HANA.

No SAP HANA, dar a **nome de utilizador** valor o mesmo valor da **nome de utilizador** no Azure AD. Este passo estabelece a ligação entre os dois usuários.

Para configurar e testar o Azure AD início de sessão único com o SAP HANA, conclua os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
1. [Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
1. [Criar um utilizador de teste de SAP HANA](#creating-a-sap-hana-test-user) ter um equivalente da Eduarda Almeida no SAP HANA que está ligado a representação do Azure AD do utilizador.
1. [Atribua o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. [Testar início de sessão único](#testing-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de SAP HANA.

**Para configurar o Azure AD início de sessão único com o SAP HANA, siga os passos seguintes:**

1. No portal do Azure, sobre o **SAP HANA** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Na **início de sessão único** caixa de diálogo em **baseado em SAML início de sessão**, selecione **modo**.
 
    ![Caixa de diálogo de início de sessão único](./media/saphana-tutorial/tutorial_saphana_samlbase.png)

1. Na **SAP HANA domínio e URLs** secção, siga os passos seguintes:

    ![Informações de início de sessão de único domínio e URLs](./media/saphana-tutorial/tutorial_saphana_url.png)

    a. Na **identificador** , escreva o seguinte: `HA100` 

    b. Na **URL de resposta** caixa, escreva um URL com o seguinte padrão: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte os [equipa de suporte de cliente do SAP HANA](https://cloudplatform.sap.com/contact.html) obter esses valores. 

1. Na **certificado de assinatura SAML** secção, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Se o certificado não está ativo, em seguida, ativá-lo ao selecionar o **ativar o novo certificado** caixa de verificação no Azure AD. 

1. O aplicativo de SAP HANA espera que as asserções SAML num formato específico. Captura de ecrã seguinte mostra um exemplo desse formato. 

    Aqui mapeámos a **identificador de utilizador** com o **ExtractMailPrefix()** função da **user.mail**. Isso permite que o valor de prefixo de e-mail do utilizador, o que é o ID de utilizador exclusivo. Este ID de utilizador é enviado para o aplicativo de SAP HANA em cada resposta com êxito.

    ![Configurar o início de sessão único](./media/saphana-tutorial/attribute.png)

1. Na **atributos de utilizador** secção a **início de sessão único** caixa de diálogo caixa, siga os passos seguintes:

    a. Na **identificador de utilizador** na lista pendente, selecione **ExtractMailPrefix**.
    
    b. Na **correio** na lista pendente, selecione **user.mail**.

1. Selecione o botão **Guardar**.

    ![Configurar o início de sessão único botão Save](./media/saphana-tutorial/tutorial_general_400.png)
    
1. Para configurar o início de sessão único no lado do SAP HANA, inicie sessão na sua **consola de Web do HANA XSA** indo para o respetivo ponto final de HTTPS.

    > [!NOTE]
    > Na configuração predefinida, o URL redireciona o pedido para um início de sessão no ecrã, que requer as credenciais de um utilizador de base de dados do SAP HANA autenticado. O utilizador que inicia sessão tem de ter permissões para efetuar tarefas de administração de SAML.

1. Na Interface de Web XSA, aceda a **fornecedor de identidade de SAML**. Em seguida, selecione o **+** botão na parte inferior do ecrã, para apresentar o **adicionar informações do fornecedor de identidade** painel. Em seguida, siga os passos seguintes:

    ![Adicionar fornecedor de identidade](./media/saphana-tutorial/sap1.png)

    a. Na **adicionar informações do fornecedor de identidade** painel, cole o conteúdo do XML de metadados (que transferiu a partir do portal do Azure) na **metadados** caixa.

    ![Adicionar as definições do fornecedor de identidade](./media/saphana-tutorial/sap2.png)

    b. Se o conteúdo do documento XML é válido, o processo de análise extrai as informações que são necessárias para o **assunto, o ID de entidade e o emissor** campos na **dados gerais** área de ecrã. Ele também extrai as informações necessárias para os campos de URL no **destino** ecrã área, por exemplo, o **URL de Base e o URL de SingleSignOn (*)** campos.

    ![Adicionar as definições do fornecedor de identidade](./media/saphana-tutorial/sap3.png)

    c. No **Name** caixa da **dados gerais** área de ecrã, introduza um nome para o novo provedor de identidade de SAML SSO.

    > [!NOTE]
    > O nome do SAML IDP é obrigatório e tem de ser exclusivo. É apresentado na lista de IDPs de SAML disponível que é apresentado quando seleciona SAML como o método de autenticação para aplicações de SAP HANA XS para utilizar. Por exemplo, pode fazê-lo **autenticação** área da ferramenta de administração do artefacto de XS de ecrã.

1. Selecione **guardar** para guardar os detalhes do fornecedor de identidade SAML e adicionar o IDP SAML novo à lista de IDPs de SAML conhecidos.

    ![Botão Guardar](./media/saphana-tutorial/sap4.png)

1. No Studio HANA, nas propriedades do sistema a **Configuration** separador, filtrar as definições por **saml**. Em seguida, ajustar a **assertion_timeout** partir **10 seg.** para **seg 120**.

    ![definição de assertion_timeout](./media/saphana-tutorial/sap7.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação! Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e acessar o embedded documentação através da **configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded na [documentação do embedded do Azure AD](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. Na **portal do Azure**, no painel esquerdo, selecione a **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](./media/saphana-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/saphana-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, selecione **Add** na parte superior da caixa de diálogo.
 
    ![Botão Adicionar](./media/saphana-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** diálogo caixa, siga os passos seguintes:
 
    ![A caixa de diálogo de utilizador](./media/saphana-tutorial/create_aaduser_04.png) 

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe**e, em seguida, anote a palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-a-sap-hana-test-user"></a>Criar um utilizador de teste de SAP HANA

Para ativar a utilizadores do Azure AD iniciar sessão no SAP HANA, tem de aprovisioná-los no SAP HANA.
SAP HANA suporta o aprovisionamento de just-in-time, que está por ativada por predefinição.

Se precisar de criar manualmente um utilizador, siga os passos seguintes:

>[!NOTE]
>Pode alterar a autenticação externa que o utilizador utiliza. Eles podem autenticar com um sistema externo, como o Kerberos. Para obter informações detalhadas sobre identidades externas, contacte o seu [administrador de domínio](https://cloudplatform.sap.com/contact.html).

1. Abra o [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) como administrador e, em seguida, ativar o utilizador de DB para SAML SSO.

    ![Criar utilizador](./media/saphana-tutorial/sap5.png)

1. Selecione a caixa de verificação invisível à esquerda da vírgula **SAML**e, em seguida, selecione a **configurar** ligação.

1. Selecione **adicionar** para adicionar o IDP de SAML.  Selecione o IDP SAML apropriado e, em seguida, selecione **OK**.

1. Adicionar a **identidades externas** (no caso, BrittaSimon) ou escolha **qualquer**. Em seguida, selecione **OK**.

    >[!Note]
    >Se o **qualquer** não está selecionada a caixa de verificação, em seguida, o nome de utilizador no HANA tem de corresponder exatamente ao nome do utilizador, o UPN antes do sufixo de domínio. (Por exemplo, BrittaSimon@contoso.com torna-se BrittaSimon no HANA.)

1. Para fins de teste, atribuir todos **XS** funções para o utilizador.

    ![Atribuir funções](./media/saphana-tutorial/sap6.png)

    > [!TIP]
    > Deve dar permissões que são adequadas para seus casos de uso apenas.

1. Guarde o utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao SAP HANA.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida ao SAP HANA, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações. Ir para a vista de diretório e aceda a **aplicações empresariais**. Selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SAP HANA**.

    ![Atribuir utilizador](./media/saphana-tutorial/tutorial_saphana_app.png) 

1. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

1. Selecione o **adicionar** botão. Na **adicionar atribuição** caixa de diálogo, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** no **utilizadores** lista.

1. Clique nas **selecionar** botão no **utilizadores e grupos** caixa de diálogo.

1. Selecione o **atribua** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico do SAP HANA no painel de acesso, deve obter automaticamente a sessão iniciada na sua aplicação SAP HANA.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/saphana-tutorial/tutorial_general_01.png
[2]: ./media/saphana-tutorial/tutorial_general_02.png
[3]: ./media/saphana-tutorial/tutorial_general_03.png
[4]: ./media/saphana-tutorial/tutorial_general_04.png

[100]: ./media/saphana-tutorial/tutorial_general_100.png

[200]: ./media/saphana-tutorial/tutorial_general_200.png
[201]: ./media/saphana-tutorial/tutorial_general_201.png
[202]: ./media/saphana-tutorial/tutorial_general_202.png
[203]: ./media/saphana-tutorial/tutorial_general_203.png

