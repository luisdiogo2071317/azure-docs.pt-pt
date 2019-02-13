---
title: 'Tutorial: Integração do Active Directory do Azure com Sansan | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Sansan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a4e640ba87adf54363611a708b1fec9b00b99e3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168228"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Tutorial: Integração do Active Directory do Azure com Sansan

Neste tutorial, saiba como integrar Sansan com o Azure Active Directory (Azure AD).

Integrar Sansan no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Sansan
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Sansan (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Sansan, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Sansan logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Sansan da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sansan-from-the-gallery"></a>Adicionando Sansan da Galeria
Para configurar a integração do Sansan com o Azure AD, terá de adicionar Sansan a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Sansan a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Sansan**.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/tutorial_sansan_search.png)

5. No painel de resultados, selecione **Sansan**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Sansan com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Sansan a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Sansan deve ser estabelecido.

Sansan, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Sansan, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Sansan](#creating-a-sansan-test-user)**  - para ter um equivalente da Eduarda Almeida na Sansan que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Sansan.

**Para configurar o Azure AD início de sessão único com Sansan, execute os seguintes passos:**

1. No portal do Azure, sobre o **Sansan** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. Sobre o **Sansan domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com os seguintes padrões: 
    
    | Ambiente | do IdP |
    |:--- |:--- |
    | Web de PC |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Aplicação móvel nativa |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Definições do browser para dispositivos móveis |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real. Contacte [equipa de suporte de cliente Sansan](https://www.sansan.com/form/contact) obter esses valores. 
     
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_general_400.png)

6. Aplicação Sansan espera várias **identificadores** e **URLs de resposta** para oferecer suporte a vários ambientes (web de PC, aplicação móvel nativo, definições do browser para dispositivos móveis), que podem ser configurados com o PowerShell script. Os passos detalhados são explicados abaixo.

7. Para configurar várias **identificadores** e **URLs de resposta** para aplicação de Sansan com o script do PowerShell, execute os seguintes passos:

    ![Configurar o início de sessão único obj](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Vá para o **propriedades** página do **Sansan** aplicação e copie o **ID de objeto** usando **cópia** botão e cole-o no bloco de notas.

    b. O **ID de objeto**, que copiou do portal do Azure vai ser utilizado como **ServicePrincipalObjectId** no script de PowerShell utilizado mais tarde no tutorial. 

    c. Agora, abra uma linha de comandos elevada do Windows PowerShell.
    
    >[!NOTE] 
    > Tem de instalar o módulo AzureAD (utilize o comando `Install-Module -Name AzureAD`). Se lhe for pedido para instalar um módulo de NuGet ou o novo módulo do Azure Active Directory V2 PowerShell, digite s e pressione ENTER.

    d. Executar `Connect-AzureAD` e inicie sessão com uma conta de utilizador de Administrador Global.

    e. Utilize o seguinte script para atualizar várias URLs para uma aplicação:

    ```powershell
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

8. Após a conclusão com êxito do script do PowerShell, o resultado do script será semelhante a esta, conforme mostrado abaixo e os valores de URL for atualizados, mas eles não sejam refletidos no portal do Azure. 

    ![Configurar o script de início de sessão único](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. Sobre o **Sansan configuração** secção, clique em **configurar Sansan** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Para configurar o início de sessão único num **Sansan** lado, terá de enviar o transferido **certificado**, **URL de fim de sessão**, **ID de entidade de SAML**, e **SAML único início de sessão no URL do serviço** ao [equipa de suporte de Sansan](https://www.sansan.com/form/contact). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

>[!NOTE]
>Configuração de PC do navegador também funciona para aplicações móveis e o browser para dispositivos móveis, juntamente com a web do PC. 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sansan-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sansan-test-user"></a>Criar um utilizador de teste Sansan

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Sansan. Aplicação de Sansan tem do utilizador a ser aprovisionado no aplicativo antes de fazer o SSO. 

>[!NOTE]
>Se precisar de criar manualmente um utilizador ou do batch de utilizadores, terá de contactar o [equipa de suporte de Sansan](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Sansan.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Sansan, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Sansan**.

    ![Configurar o início de sessão único](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Sansan no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Sansan.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

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

