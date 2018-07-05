---
title: Adicionar Google + como um fornecedor de identidade de OAuth2 através de políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Exemplo usando o Google + como fornecedor de identidade utilizando o protocolo OAuth2.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 19b7f708d43907ac45450a64f988b2a517293511
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446712"
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>O Azure Active Directory B2C: Adicionar Google + como um fornecedor de identidade de OAuth2 através de políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este guia mostra como ativar o início de sessão para os utilizadores da conta do Google + através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:

1.  Criando um aplicativo de conta do Google +.
2.  Adicionar a Google + conta chave da aplicação para o Azure AD B2C
3.  Adicionar o fornecedor de afirmações para uma política
4.  Registar o Google + conta fornecedor de afirmações para um percurso do utilizador
5.  Carregar a política para um Azure AD B2C de inquilino e testá-lo

## <a name="create-a-google-account-application"></a>Criar uma aplicação de conta do Google +
Para utilizar Google + como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação do Google + e fornecer os parâmetros certos. Pode registrar uma Google + aplicação aqui: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Vá para o [consola de programadores da Google](https://console.developers.google.com/) e inicie sessão com as credenciais da conta Google +.
2.  Clique em **criar projeto**, introduza um **nome do projeto**e, em seguida, clique em **criar**.

3.  Clique nas **menu de projetos**.

    ![Google + conta - Selecione projeto](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Clique nas **+** botão.

    ![Google + conta - criar novo projeto](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Introduza um **nome do projeto**e, em seguida, clique em **criar**.

    ![Google + conta - novo projeto](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Aguarde até que o projeto está pronto e clique nas **menu de projetos**.

    ![Google + conta - Aguarde até que o novo projeto está pronto a utilizar](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Clique no nome do seu projeto.

    ![Google + conta - Selecione o novo projeto](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Clique em **Gestor de API** e, em seguida, clique em **credenciais** na navegação à esquerda.
9.  Clique nas **ecrã de consentimento do OAuth** separador na parte superior.

    ![Google + conta - ecrã de consentimento do OAuth definido](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Selecione ou especifique válido **endereço de E-Mail**, forneça um **nome do produto**e clique em **guardar**.

    ![Google + - as credenciais de aplicação](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Clique em **novas credenciais** e, em seguida, escolha **ID de cliente OAuth**.

    ![Google + - criar novas credenciais de aplicação](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Sob **tipo de aplicação**, selecione **aplicação Web**.

    ![Google + - tipo de selecionar aplicação](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Fornecer um **Name** para a sua aplicação, introduza `https://login.microsoftonline.com` no **JavaScript autorizados origens** campo, e `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **autorizado URIs de redirecionamento** campo. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com). O **{inquilino}** valor diferencia maiúsculas de minúsculas. Clique em **Criar**.

    ![Google + - fornecer JavaScript autorizados origens e URIs de redirecionamento](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Copie os valores da **Id de cliente** e **segredo do cliente**. Terá de ambos para configurar o Google + como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.

    ![Google + - copie os valores de segredo de cliente e o Id de cliente](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Adicionar a Google + conta chave da aplicação para o Azure AD B2C
Federação com o Google + contas requer um segredo do cliente para a conta do Google + à confiança do Azure AD B2C em nome do aplicativo. Tem de armazenar seu segredo de aplicação do Google + no inquilino do Azure AD B2C:  

1.  Aceda ao seu inquilino do Azure AD B2C e selecione **definições do B2C** > **Framework de experiência de identidade**
2.  Selecione **chaves de política** para exibir as chaves disponíveis no seu inquilino.
3.  Clique em **+ adicionar**.
4.  Para **opções**, utilize **Manual**.
5.  Para **Name**, utilize `GoogleSecret`.  
    O prefixo `B2C_1A_` podem ser adicionados automaticamente.
6.  Na **segredo** , introduza o seu segredo de aplicação do Google da [consola de programadores da Google](https://console.developers.google.com/) que copiou anteriormente.
7.  Para **utilização de chave**, utilize **assinatura**.
8.  Clique em **Criar**.
9.  Confirme que criou a chave `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Adicionar um fornecedor de afirmações na sua política de extensão

Se pretender que os utilizadores iniciem sessão com a conta do Google +, terá de definir a conta do Google + como um fornecedor de afirmações. Em outras palavras, terá de especificar um ponto de final do Azure AD B2C se comunica com. O ponto final fornece um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado.

Definir a conta do Google + como um fornecedor de afirmações, adicionando `<ClaimsProvider>` nó em seu arquivo de política de extensão:

1.  Abra o ficheiro de política de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho. Se precisar de um editor de XML, [Experimente o Visual Studio Code](https://code.visualstudio.com/download), um editor simples de várias plataformas.
2.  Encontrar o `<ClaimsProviders>` secção
3.  Adicione o seguinte fragmento XML sob o `ClaimsProviders` elemento e substitua `client_id` valor com o Google + conta cliente ID da sua aplicação antes de guardar o ficheiro.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registar o Google + conta fornecedor de afirmações para inscrever ou iniciar sessão no percurso do utilizador

O fornecedor de identidade tiver sido configurado.  No entanto, não está disponível em qualquer uma dos ecrãs de início de sessão-inscrição/início de sessão. Adicionar o Google + conta fornecedor de identidade para o seu utilizador `SignUpOrSignIn` percurso do utilizador. Para tornar disponível, criamos um duplicado de um percurso do utilizador modelo existente.  Em seguida, adicionamos o Google + conta fornecedor de identidade:

>[!NOTE]
>
>Se tiver copiado o `<UserJourneys>` elemento do ficheiro de base da sua política de para o ficheiro de extensão (TrustFrameworkExtensions.xml), pode ignorar esta secção.

1.  Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
2.  Encontrar o `<UserJourneys>` elemento e copie todo o conteúdo do `<UserJourneys>` nó.
3.  Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
4.  Cole o conteúdo inteiro do `<UserJourney>` nó que copiou como subordinado do `<UserJourneys>` elemento.

### <a name="display-the-button"></a>Exibir o botão
O `<ClaimsProviderSelections>` elemento define a lista de opções de seleção do fornecedor de afirmações e sua ordem.  `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade numa página de início de sessão-inscrição/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para a conta do Google +, um novo botão exibido quando um utilizador que chegam na página. Para adicionar este elemento:

1.  Encontrar o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"` no percurso do utilizador que copiou.
2.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
3.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação
Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com a conta do Google + receber um token.

1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Certifique-se a `Id` tem o mesmo valor que `TargetClaimsExchangeId` na secção anterior
> * Certifique-se de `TechnicalProfileReferenceId` ID está definido para o perfil técnico que criou anteriormente (Google-OAUTH).

## <a name="upload-the-policy-to-your-tenant"></a>Carregar a política para o seu inquilino
1.  Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e abra a **do Azure AD B2C** painel.
2.  Selecione **arquitetura de experiências de identidade**.
3.  Abra o **todas as políticas** painel.
4.  Selecione **carregar política**.
5.  Verifique **substituir a política, se existir** caixa.
6.  **Carregar** TrustFrameworkExtensions.xml e certifique-se de que ele não falha a validação

## <a name="test-the-custom-policy-by-using-run-now"></a>Testar a política personalizada com executar agora
1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.

    >[!NOTE]
    >
    >    **Executar agora** requer, pelo menos, um aplicativo para ser foi pré-registado no inquilino. 
    >    Para saber como registar aplicações, veja o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.


2.  Open **B2C_1A_signup_signin**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deve ser capaz de iniciar sessão com a conta do Google +.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[Opcional] Registar o Google + conta fornecedor de afirmações para percurso do utilizador de edição de perfil
Pode querer adicionar o Google + conta fornecedor de identidade também ao seu utilizador `ProfileEdit` percurso do utilizador. Para tornar disponível, repetimos os dois últimos passos:

### <a name="display-the-button"></a>Exibir o botão
1.  Abra o ficheiro de extensão da sua política (por exemplo, TrustFrameworkExtensions.xml).
2.  Encontrar o `<UserJourney>` nó que inclui `Id="ProfileEdit"` no percurso do utilizador que copiou.
3.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
4.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação
1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="upload-the-policy-to-your-tenant"></a>Carregar a política para o seu inquilino
1.  Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e abra a **do Azure AD B2C** painel.
2.  Selecione **arquitetura de experiências de identidade**.
3.  Abra o **todas as políticas** painel.
4.  Selecione **carregar política**.
5.  Verifique os **substituir a política, se existir** caixa.
6.  **Carregar** TrustFrameworkExtensions.xml e certifique-se de que ele não falha a validação.

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testar a política de edição de perfil personalizada com o executar agora

1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
2.  Open **B2C_1A_ProfileEdit**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deve ser capaz de iniciar sessão com a conta do Google +.

## <a name="download-the-complete-policy-files"></a>Transferir os ficheiros de política concluída
Opcional: Recomendamos que crie o seu cenário com sua própria política personalizada do percorrer a ficheiros depois de concluir o guia de introdução políticas personalizadas em vez de utilizar estes ficheiros de exemplo.  [Ficheiros de política de exemplo para referência](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
