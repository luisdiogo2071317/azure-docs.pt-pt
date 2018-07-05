---
title: Adicionar o LinkedIn como um fornecedor de identidade do OAuth2 ao utilizar políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Um artigo de procedimento sobre como configurar uma aplicação do LinkedIn, utilizando o protocolo OAuth2 e as políticas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 334f696d79cf801facf7c5301b2240b69f7134f7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444383"
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a>O Azure Active Directory B2C: Adicionar LinkedIn como um fornecedor de identidade ao utilizar políticas personalizadas
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão dos utilizadores de uma conta do LinkedIn, utilizando [políticas personalizadas](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Pré-requisitos
Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

## <a name="step-1-create-a-linkedin-account-application"></a>Passo 1: Criar uma aplicação de conta do LinkedIn
Para utilizar o LinkedIn como um fornecedor de identidade no Azure Active Directory B2C (Azure AD B2C), tem de criar um aplicativo do LinkedIn e fornecer os parâmetros certos. Pode registrar um aplicativo do LinkedIn ao aceder a [página de inscrição do LinkedIn](https://www.linkedin.com/start/join).

1. Vá para o [gestão de aplicações do LinkedIn](https://www.linkedin.com/secure/developer?newapp=) Web site, inicie sessão com as credenciais da sua conta do LinkedIn e, em seguida, selecione **Criar aplicação**.

    ![LinkedIn conta - criar aplicação](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. Sobre o **criar uma nova aplicação** página, efetue o seguinte:

    a. Tipo de sua **nome da empresa**, um descritivo **nome** para a empresa e um **Descrição** da sua nova aplicação.

    b. Carregar seu **logótipo da aplicação**.

    c. Selecione um **utilização de aplicações**.

    d. Na **URL do site** caixa, cole **https://login.microsoftonline.com**.

    e. Tipo de sua **E-Mail empresariais** endereço e **telefone da empresa** número.

    f. Na parte inferior da página, leia e aceite os termos de utilização e, em seguida, selecione **submeter**.

    ![LinkedIn conta - configurar propriedades da aplicação](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. Selecione **autenticação**e, em seguida, tenha em atenção a **ID de cliente** e **segredo do cliente** valores.

4. Na **autorizado URLs de redirecionamento** caixa, cole **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp**. Substitua {*inquilino*} com o nome do seu inquilino (por exemplo, contosob2c.onmicrosoft.com). Certifique-se de que está a utilizar o esquema HTTPS. 

    ![Conta do LinkedIn - conjunto autorizado redirecionar URLs](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    >O segredo do cliente é uma credencial de segurança importantes. Não partilhe este segredo com qualquer pessoa ou distribuí-la com a sua aplicação.

5. Selecione **Adicionar**.

6. Selecione **definições**, altere a **estado da aplicação** para **Live**e, em seguida, selecione **atualização**.

    ![Conta do LinkedIn - estado da aplicação de conjunto](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>Passo 2: Adicionar a chave de aplicação do LinkedIn para o Azure AD B2C
Federação com contas do LinkedIn requer um segredo do cliente para a conta do LinkedIn à confiança do Azure AD B2C em nome do aplicativo. Para armazenar o segredo de aplicação do LinkedIn no seu inquilino do Azure AD B2C, efetue o seguinte:  

1. No seu inquilino do Azure AD B2C, selecione **definições do B2C** > **Framework de experiência de identidade**.

2. Para ver as chaves que estão disponíveis no seu inquilino, selecione **chaves de política**.

3. Selecione **Adicionar**.

4. Na **opções** caixa, selecione **carregar**.

5. Na **Name** , escreva **B2cRestClientCertificate**.  
    O prefixo *B2C_1A_* podem ser adicionados automaticamente.

6. Na **segredo** , introduza o seu segredo de aplicação do LinkedIn da [Portal de registo de aplicação](https://apps.dev.microsoft.com).

7. Para **utilização de chave**, selecione **encriptação**.

8. Selecione **Criar**. 

9. Confirme que criou o `B2C_1A_LinkedInSecret`chave.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Passo 3: Adicionar um fornecedor de afirmações na sua política de extensão
Se pretender que os utilizadores iniciem sessão utilizando a respetiva conta do LinkedIn, tem de definir o LinkedIn como um fornecedor de afirmações. Em outras palavras, tem de especificar os pontos de extremidade do Azure AD B2C se comunica com. Os pontos de extremidade fornecem um conjunto de afirmações que são utilizados pelo Azure AD B2C para verificar que um utilizador específico foi autenticado.

Definir o LinkedIn como um fornecedor de afirmações, adicionando um `<ClaimsProvider>` nó em seu arquivo de política de extensão:

1. No seu diretório de trabalho, abra a *TrustFrameworkExtensions.xml* ficheiro de política de extensão. 

2. Procure o `<ClaimsProviders>` elemento.

3. Na `<ClaimsProviders>` elemento, adicione o seguinte fragmento XML: 

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
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

4. Substitua a *client_id* valor com o ID de cliente de aplicação do LinkedIn.

5. Guarde o ficheiro.

## <a name="step-4-register-the-linkedin-account-claims-provider"></a>Passo 4: Registar o fornecedor de afirmações de conta do LinkedIn
Ter configurado o fornecedor de identidade. No entanto, ele ainda não está disponível em qualquer uma das janelas da inscrição ou início de sessão. Agora, tem de adicionar o fornecedor de identidade de conta do LinkedIn ao seu utilizador `SignUpOrSignIn` percurso do utilizador.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Passo 4.1: Fazer uma cópia do percurso do utilizador
Para disponibilizar o percurso do utilizador, crie um duplicado de um modelo do percurso de utilizador existente e, em seguida, adicionar o fornecedor de identidade do LinkedIn:

>[!NOTE]
>Se tiver copiado o `<UserJourneys>` elemento a partir do ficheiro de base da sua política para o *TrustFrameworkExtensions.xml* ficheiro de extensão, pode ignorar esta secção.

1. Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).

2. Procure o `<UserJourneys>` elemento, selecione todo o conteúdo do `<UserJourney>` nó e, em seguida, selecione **Cortar** para mover o texto selecionado para a área de transferência.

3. Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e procure o `<UserJourneys>` elemento. Se o elemento não existir, adicioná-lo.

4. Cole o conteúdo inteiro dos `<UserJourney>` nó, que é movido para a área de transferência no passo 2, para o `<UserJourneys>` elemento.

### <a name="step-42-display-the-button"></a>Passo 4.2: Exibir o "botão"
O `<ClaimsProviderSelections>` elemento define a lista de opções de seleção do fornecedor de afirmações e sua ordem. O `<ClaimsProviderSelection>` nó é semelhante a um botão de fornecedor de identidade numa página de inscrição ou início de sessão. Se adicionar um `<ClaimsProviderSelection>` nó para uma conta do LinkedIn, um novo botão é apresentado quando um utilizador que chegam na página. Para adicionar este elemento, faça o seguinte:

1. Procure o `<UserJourney>` nó que contém `Id="SignUpOrSignIn"` no percurso do utilizador que copiou.

2. Localize a `<OrchestrationStep>` nó que inclui `Order="1"`.

3. Na `<ClaimsProviderSelections>` elemento, adicione o seguinte fragmento XML:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Passo 4.3: Ligar o botão para uma ação
Agora que tem um botão no local, tem de o associar a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com a conta do LinkedIn para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações de conta do LinkedIn:

1. Procure o `<OrchestrationStep>` nó que contém `Order="2"` no `<UserJourney>` nó.

2. Na `<ClaimsExchanges>` elemento, adicione o seguinte fragmento XML:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* Certifique-se de que `Id` tem o mesmo valor que `TargetClaimsExchangeId` na secção anterior.
    >* Certifique-se de que o `TechnicalProfileReferenceId` ID está definido para o perfil técnico que criou anteriormente (LinkedIn-OAuth).

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Passo 5: Carregar a política para o seu inquilino
1. Na [portal do Azure](https://portal.azure.com), mude para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e, em seguida, selecione **do Azure AD B2C**.

2. Selecione **arquitetura de experiências de identidade**.

3. Selecione **todas as políticas**.

4. Selecione **carregar política**.

5. Selecione o **substituir a política, se existir** caixa de verificação.

6. Carregar o *TrustFrameworkBase.xml* e *TrustFrameworkExtensions.xml* ficheiros e certifique-se de que eles passarem na validação.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Passo 6: Testar a política personalizada com executar agora
1. Selecione **definições do Azure AD B2C**e, em seguida, selecione **Framework de experiência de identidade**.

    >[!NOTE]
    >Executar agora requer, pelo menos, um aplicativo para ser foi pré-registado no inquilino. Para saber como registar aplicações, veja o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.

2. Open **B2C_1A_signup_signin**, a política personalizada de terceiros (RP) da entidade confiadora que carregou e, em seguida, selecione **executar agora**.  
    Deve agora conseguir iniciar sessão com a conta do LinkedIn.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a>Passo 7: (Opcional) registar a conta do LinkedIn afirmações do fornecedor para o percurso do utilizador de edição de perfil
Pode também querer adicionar o fornecedor de identidade de conta do LinkedIn à sua `ProfileEdit` percurso do utilizador. Para tornar o utilizador jornada disponível, repita "o passo 4." Desta vez, selecione o `<UserJourney>` nó que contém `Id="ProfileEdit"`. Guardar e carregar para testar a sua política.

## <a name="optional-download-the-complete-policy-files"></a>(Opcional) Transferir os ficheiros de política concluída
Depois de concluir o [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) passo a passo, é recomendável que criar seu cenário com seus próprios arquivos de política personalizada. Para referência, nós fornecemos [arquivos de diretiva de exemplo](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).
