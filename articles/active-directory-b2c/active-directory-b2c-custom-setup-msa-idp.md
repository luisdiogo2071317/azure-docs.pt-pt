---
title: Adicionar conta da Microsoft (MSA) como um fornecedor de identidade com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Exemplo com a Microsoft como fornecedor de identidade através do protocolo do OpenID Connect (OIDC).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6a981f112c97ee35b476c92f6f698e68a12a1363
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336826"
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>O Azure Active Directory B2C: Adicionar a conta Microsoft (MSA) como um fornecedor de identidade com as políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para os utilizadores da conta Microsoft (MSA) através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Pré-requisitos
Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:

1.  Criar uma aplicação da conta Microsoft.
2.  Adicionar a chave da aplicação conta Microsoft para o Azure AD B2C
3.  Adicionar o fornecedor de afirmações para uma política
4.  Registar o fornecedor de afirmações Account Microsoft para um percurso do utilizador
5.  Carregar a política para um Azure AD B2C de inquilino e testá-lo

## <a name="create-a-microsoft-account-application"></a>Criar uma aplicação da conta Microsoft
Para utilizar a conta Microsoft como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação da conta Microsoft e fornecer os parâmetros certos. Precisa de uma conta Microsoft. Se não tiver uma, visite [ https://www.live.com/ ](https://www.live.com/).

1.  Vá para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) e inicie sessão com as credenciais da conta Microsoft.
2.  Clique em **adicionar uma aplicação**.

    ![Microsoft conta - adicionar uma aplicação](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Fornecer um **Name** para a sua aplicação, **e-mail de contacto**, desmarque a opção **nos ajudam a começar** e clique em **criar**.

    ![Conta Microsoft - Registre-se a aplicação](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Copie o valor da **Id da aplicação**. Precisa, mesmo para configurar a conta Microsoft como fornecedor de identidade no seu inquilino.

    ![Conta Microsoft - copiar o valor do Id de aplicação](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Clique em **adicionar plataforma**

    ![Microsoft conta - adicionar plataforma](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Na lista de plataforma, escolha **Web**.

    ![Conta da Microsoft - na lista de plataforma escolha Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Introduza `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` no **URIs de redirecionamento** campo. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c).

    ![Conta da Microsoft - URLs de redirecionamento de conjunto](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Clique em **gerar nova palavra-passe** sob a **segredos da aplicação** secção. Copie a nova palavra-passe apresentada no ecrã. Precisa, mesmo para configurar a conta Microsoft como fornecedor de identidade no seu inquilino. Esta palavra-passe é uma credencial de segurança importantes.

    ![Microsoft conta - gerar nova palavra-passe](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Conta Microsoft - copiar a nova palavra-passe](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Marque a caixa que diz **suporte do Live SDK** sob a **opções avançadas** secção. Clique em **Guardar**.

    ![Conta Microsoft - suporte do Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>Adicionar a chave da aplicação conta Microsoft para o Azure AD B2C
Federação com contas Microsoft requer um segredo do cliente para a conta Microsoft à confiança do Azure AD B2C em nome do aplicativo. Tem de armazenar seu secert de aplicativo de conta Microsoft no inquilino do Azure AD B2C:   

1.  Aceda ao seu inquilino do Azure AD B2C e selecione **definições do B2C** > **Framework de experiência de identidade**
2.  Selecione **chaves de política** para exibir as chaves disponíveis no seu inquilino.
3.  Clique em **+ adicionar**.
4.  Para **opções**, utilize **Manual**.
5.  Para **Name**, utilize `MSASecret`.  
    O prefixo `B2C_1A_` podem ser adicionados automaticamente.
6.  Na **segredo** , introduza o segredo da aplicação Microsoft do https://apps.dev.microsoft.com
7.  Para **utilização de chave**, utilize **assinatura**.
8.  Clique em **Criar**.
9.  Confirme que criou a chave `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Adicionar um fornecedor de afirmações na sua política de extensão
Se pretender que os utilizadores iniciem sessão com a Microsoft Account, terá de definir Account Microsoft como um fornecedor de afirmações. Em outras palavras, terá de especificar um ponto de final do Azure AD B2C se comunica com. O ponto final fornece um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado.

Definir Account Microsoft como um fornecedor de afirmações, adicionando `<ClaimsProvider>` nó em seu arquivo de política de extensão:

1.  Abra o ficheiro de política de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho. Se precisar de um editor de XML, [Experimente o Visual Studio Code](https://code.visualstudio.com/download), um editor simples de várias plataformas.
2.  Encontrar o `<ClaimsProviders>` secção
3.  Adicionar o seguinte fragmento XML sob o `ClaimsProviders` elemento:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Substitua `client_id` valor com o seu Id de cliente da aplicação de Account Microsoft

5.  Guarde o ficheiro.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registre-se que Account Microsoft afirmações do fornecedor de início de sessão se ou iniciar sessão no percurso do utilizador

Neste momento, o fornecedor de identidade tiver sido configurado, mas não está disponível em qualquer uma dos ecrãs de início de sessão-inscrição/início de sessão. Agora precisa adicionar o fornecedor de identidade do Microsoft Account ao seu utilizador `SignUpOrSignIn` percurso do utilizador. Para tornar disponível, criamos um duplicado de um percurso do utilizador modelo existente.  Em seguida, adicionamos o fornecedor de identidade Account Microsoft:

> [!NOTE]
>
>Se copiou anteriormente o `<UserJourneys>` elemento do ficheiro de base da sua política de para o ficheiro de extensão `TrustFrameworkExtensions.xml`, pode ignorar esta secção.

1.  Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
2.  Encontrar o `<UserJourneys>` elemento e copie todo o conteúdo do `<UserJourneys>` nó.
3.  Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
4.  Cole o conteúdo inteiro do `<UserJourneys>` nó que copiou como subordinado do `<UserJourneys>` elemento.

### <a name="display-the-button"></a>Exibir o botão
O `<ClaimsProviderSelections>` elemento define a lista de opções de seleção do fornecedor de afirmações e sua ordem.  `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade numa página de início de sessão-inscrição/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para a conta Microsoft, um novo botão exibido quando um utilizador que chegam na página. Para adicionar este elemento:

1.  Encontrar o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"` no percurso do utilizador que copiou.
2.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
3.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação
Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com Account Microsoft para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações Account Microsoft:

1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Certifique-se a `Id` tem o mesmo valor que `TargetClaimsExchangeId` na secção anterior
>   * Certifique-se de `TechnicalProfileReferenceId` ID está definido para o perfil técnico que criou anteriormente (MSA-OIDC).

## <a name="upload-the-policy-to-your-tenant"></a>Carregar a política para o seu inquilino
1.  Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e abra a **do Azure AD B2C** painel.
2.  Selecione **arquitetura de experiências de identidade**.
3.  Abra o **todas as políticas** painel.
4.  Selecione **carregar política**.
5.  Verifique **substituir a política, se existir** caixa.
6.  **Carregar** TrustFrameworkExtensions.xml e certifique-se de que ele não falha a validação

## <a name="test-the-custom-policy-by-using-run-now"></a>Testar a política personalizada com executar agora

1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
> [!NOTE]
>
>**Executar agora** requer, pelo menos, um aplicativo para ser foi pré-registado no inquilino. Para saber como registar aplicações, veja o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.
2.  Open **B2C_1A_signup_signin**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deve ser capaz de iniciar sessão com conta Microsoft.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[Opcional] Registar o fornecedor de afirmações Account Microsoft para percurso do utilizador de edição de perfil
Pode querer adicionar o fornecedor de identidade do Microsoft Account também ao seu utilizador `ProfileEdit` percurso do utilizador. Para tornar disponível, repetimos os dois últimos passos:

### <a name="display-the-button"></a>Exibir o botão
1.  Abra o ficheiro de extensão da sua política (por exemplo, TrustFrameworkExtensions.xml).
2.  Encontrar o `<UserJourney>` nó que inclui `Id="ProfileEdit"` no percurso do utilizador que copiou.
3.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
4.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação
1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testar a política de edição de perfil personalizada com o executar agora
1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
2.  Open **B2C_1A_ProfileEdit**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deve ser capaz de iniciar sessão com conta Microsoft.

## <a name="download-the-complete-policy-files"></a>Transferir os ficheiros de política concluída
Opcional: Recomendamos que crie o seu cenário com sua própria política personalizada do percorrer a ficheiros depois de concluir o guia de introdução políticas personalizadas em vez de utilizar estes ficheiros de exemplo.  [Ficheiros de política de exemplo para referência](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
