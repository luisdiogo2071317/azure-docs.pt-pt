---
title: 'Tutorial: Integração do Azure Active Directory com Sansan | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Sansan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c8b5a84c853a974ede77e716b77f0a5007775ef7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231217"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Tutorial: Integração do Azure Active Directory com Sansan

Neste tutorial, irá aprender a integrar Sansan com o Azure Active Directory (Azure AD).

Integrar Sansan com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Sansan
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Sansan (Single Sign-On) com as respetivas contas do Azure AD
- Pode gerir as contas numa localização central - portal do Azure

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Sansan, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Sansan-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Sansan a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-sansan-from-the-gallery"></a>Adicionar Sansan a partir da Galeria
Para configurar a integração de Sansan com o Azure AD, terá de adicionar Sansan a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Sansan a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![Aplicações][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Sansan**.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/tutorial_sansan_search.png)

5. No painel de resultados, selecione **Sansan**e, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD de sessão único-
Nesta secção, configure e teste do Azure AD-início de sessão único com Sansan com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Sansan é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Sansan tem de ser estabelecida.

No Sansan, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Sansan, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Sansan](#creating-a-sansan-test-user)**  - para ter um homólogo de Britta Simon Sansan que está ligada a representação do Azure AD do utilizador.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Sansan.

**Para configurar o Azure AD-início de sessão único com Sansan, execute os seguintes passos:**

1. No portal do Azure, no **Sansan** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar o início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. No **Sansan domínio e os URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_url.png)

    No **URL de início de sessão** caixa de texto, escreva um URL utilizando os seguintes padrões: 
    
    | Ambiente | do IdP |
    |:--- |:--- |
    | Web de PC |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Aplicação móvel nativa |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Definições do browser móveis |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão real. Contacte [equipa de suporte de cliente Sansan](https://www.sansan.com/form/contact) para obter estes valores. 
     
4. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_general_400.png)

6. Aplicação de Sansan espera vários **identificadores** e **URLs de resposta** para suportar vários ambientes (PC web, aplicação móvel nativo, as definições do browser móvel), que podem ser configurados através do PowerShell script. Os passos detalhados são explicados abaixo.

7. Para configurar várias **identificadores** e **URLs de resposta** Sansan aplicação utilizando o script do PowerShell, execute os seguintes passos:

    ![Configurar o parâmetro obj Single Sign-On](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Vá para o **propriedades** página do **Sansan** aplicação e copie o **ID de objeto** utilizando **cópia** botão e cole-o bloco de notas.

    b. O **ID de objeto**, que copiou do portal do Azure será utilizado como **ServicePrincipalObjectId** no script de PowerShell utilizado mais tarde no tutorial. 

    c. Agora, abra uma linha de comandos elevada do Windows PowerShell.
    
    >[!NOTE] 
    > Tem de instalar o módulo de AzureAD (utilize o comando `Install-Module -Name AzureAD`). Se lhe for pedido para instalar um módulo NuGet ou do novo módulo do Azure Active Directory V2 PowerShell, escreva Y e prima ENTER.

    d. Executar `Connect-AzureAD` e inicie sessão com uma conta de utilizador de Administrador Global.

    e. Utilize o seguinte script para atualizar vários URLs para uma aplicação:

    ```poweshell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. Após a conclusão com êxito de script do PowerShell, o resultado do script será como esta, conforme mostrado abaixo e os valores do URL for atualizados, mas não irá obter refletidas no portal do Azure. 

    ![Configurar o script de Single Sign-On](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. No **Sansan configuração** secção, clique em **configurar Sansan** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Para configurar o início de sessão único em **Sansan** lado, terá de enviar o transferido **certificado**, **Sign-Out URL**, **ID de entidade de SAML**, e **Único início de sessão no URL do serviço SAML** para [equipa de suporte de Sansan](https://www.sansan.com/form/contact). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

>[!NOTE]
>Definição de browser de computador também funciona para aplicação móvel e móvel browser juntamente com PC web. 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_02.png) 

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_03.png) 

4. No **utilizador** diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, tipo **BrittaSimon**.

    b. No **nome de utilizador** caixa de texto, tipo de **endereço de correio eletrónico** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e anote o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sansan-test-user"></a>Criar um utilizador de teste Sansan

Nesta secção, vai criar um utilizador chamado Britta Simon Sansan. Aplicação de Sansan tem do utilizador a ser aprovisionados na aplicação antes de efetuar o SSO. 

>[!NOTE]
>Se precisar de criar manualmente um utilizador ou do lote de utilizadores, terá de contactar o [equipa de suporte de Sansan](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Sansan.

![Atribua o utilizador][200] 

**Para atribuir Britta Simon a Sansan, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Sansan**.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribua o utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribua o utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Sansan no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Sansan.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

