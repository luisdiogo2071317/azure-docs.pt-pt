---
title: Configurar o início de sessão com uma conta do LinkedIn, no Azure Active Directory B2C, com as políticas personalizadas | Documentos da Microsoft
description: Configure o início de sessão com uma conta do Google com as políticas personalizadas no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7700ef24deb82afcb2093c8fd27bcbe7f6f6420c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190348"
---
# <a name="set-up-sign-in-with-a-linkedin-account-using-custom-policies-in-azure-active-directory-b2c"></a>Configurar o início de sessão com uma conta do LinkedIn com as políticas personalizadas no Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para os utilizadores de uma conta do LinkedIn, utilizando [políticas personalizadas](active-directory-b2c-overview-custom.md) no Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Pré-requisitos

- Conclua os passos na [introdução às políticas personalizadas no Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Se ainda não tiver uma conta do LinkedIn, criar um de cada a [página de inscrição do LinkedIn](https://www.linkedin.com/start/join).
- Um aplicativo do LinkedIn exige que forneça uma imagem de logótipo de pixel de 80 X 80 para representar a aplicação.

## <a name="create-an-application"></a>Criar uma aplicação

Para utilizar o LinkedIn como um fornecedor de identidade no Azure AD B2C, terá de criar uma aplicação do LinkedIn.

1. Inicie sessão para o [gestão de aplicações do LinkedIn](https://www.linkedin.com/secure/developer?newapp=) Web site com as suas credenciais de conta do LinkedIn.
2. Selecione **Criar aplicação**.
3. Introduza o seu **nome da empresa**, uma **nome da aplicação**e um **descrição da aplicação**.
4. Carregar o **logótipo da aplicação** que criou.
5. Escolher uma **utilização de aplicações** na lista fornecida.
6. Para **URL do site**, introduza `https://your-tenant.b2clogin.com`.  Substitua `your-tenant` com o nome do seu inquilino do Azure AD B2C. Por exemplo, contoso.b2clogin.com.
7. Introduza o seu **E-Mail empresariais** endereço e **telefone da empresa** número.
8. Na parte inferior da página, leia e aceite os termos de utilização e, em seguida, selecione **submeter**.
9. Selecione **autenticação**e, em seguida, registe o **ID de cliente** e **segredo do cliente** valores a utilizar mais tarde.
10. Na **autorizado URLs de redirecionamento**, introduza `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp`. Substitua `your-tenant-name` com o nome do seu inquilino. Tem de utilizar todas as letras minúsculas, ao introduzir o nome do seu inquilino, mesmo que o inquilino está definido com letras maiúsculas no Azure AD B2C. 
11. Selecione **atualização**.
12. Selecione **definições**, altere a **estado da aplicação** para **Live**e, em seguida, selecione **atualização**.

## <a name="create-a-policy-key"></a>Criar uma chave de política

Tem de armazenar o segredo do cliente que registou anteriormente no seu inquilino do Azure AD B2C.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
3. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
4. Na página de descrição geral, selecione **arquitetura de experiências de identidade - pré-visualização**.
5. Selecione **chaves de política** e, em seguida, selecione **Add**.
6. Para **opções**, escolha `Manual`.
7. Introduza um **nome** para a chave de política. Por exemplo, `LinkedInSecret`. O prefixo `B2C_1A_` é adicionado automaticamente o nome da sua chave.
8. Na **segredo**, introduza o seu segredo de cliente que registou anteriormente.
9. Para **utilização de chave**, selecione `Signature`.
10. Clique em **Criar**.

## <a name="add-a-claims-provider"></a>Adicionar um fornecedor de afirmações

Se pretender que os utilizadores iniciem sessão com uma conta do LinkedIn, terá de definir a conta como um fornecedor de afirmações com Azure AD B2C possam comunicar através de um ponto de extremidade. O ponto final fornece um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado. 

Pode definir uma conta do LinkedIn como um fornecedor de afirmações ao adicioná-la para o **ClaimsProviders** elemento no arquivo de extensão da sua política.

1. Abra o *TrustFrameworkExtensions.xml*.
2. Encontrar o **ClaimsProviders** elemento. Se não existir, adicione-o sob o elemento raiz.
3. Adicionar um novo **ClaimsProvider** da seguinte forma:

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

4. Substitua o valor de **client_id** com o ID de cliente que registou anteriormente.
5. Guarde o ficheiro.

### <a name="upload-the-extension-file-for-verification"></a>Carregar o ficheiro de extensão de verificação

Agora, configurou sua política para que o Azure AD B2C sabe como se comunicar com a sua conta do LinkedIn. Tente carregar o ficheiro de extensão da sua política só para confirmar que ele não tem quaisquer problemas até agora.

1. Sobre o **políticas personalizadas** página no seu inquilino do Azure AD B2C, selecione **carregar política**.
2. Ativar **substituir a política, se existir**e, em seguida, procure e selecione o *TrustFrameworkExtensions.xml* ficheiro.
3. Clique em **Carregar**.

## <a name="register-the-claims-provider"></a>Registar o fornecedor de afirmações

Neste momento, o fornecedor de identidade tiver sido configurado, mas não está disponível em qualquer um dos ecrãs de inscrição ou início de sessão. Para tornar disponível, pode criar um duplicado de um percurso do utilizador modelo existente e, em seguida, modificá-lo para que ele também tem o fornecedor de identidade do LinkedIn.

1. Abra o *TrustFrameworkBase.xml* ficheiro do pacote de iniciante.
2. Localize e copie o conteúdo inteiro dos **UserJourney** elemento que inclui `Id="SignUpOrSignIn"`.
3. Abra o *TrustFrameworkExtensions.xml* e localize a **UserJourneys** elemento. Se o elemento não existir, adicione um.
4. Colar o conteúdo inteiro do **UserJourney** elemento que copiou como subordinado da **UserJourneys** elemento.
5. Mudar o nome o ID do percurso do utilizador. Por exemplo, `SignUpSignInLinkedIn`.

### <a name="display-the-button"></a>Exibir o botão

O **ClaimsProviderSelection** elemento é semelhante a um botão do fornecedor de identidade numa tela de inscrição ou início de sessão. Se adicionar um **ClaimsProviderSelection** elemento de uma conta do LinkedIn, um novo botão exibido quando um utilizador que chegam na página.

1. Encontrar o **OrchestrationStep** elemento que inclui `Order="1"` no percurso do utilizador que criou.
2. Sob **ClaimsProviderSelects**, adicione o seguinte elemento. Defina o valor da **TargetClaimsExchangeId** para um valor adequado, por exemplo `LinkedInExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação

Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com uma conta do LinkedIn para receber um token.

1. Encontrar o **OrchestrationStep** que inclua `Order="2"` no percurso do utilizador.
2. Adicione as seguintes **ClaimsExchange** elemento certificar-se de que usar o mesmo valor para **Id** que utilizou para **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    ```
    
    Atualize o valor de **TechnicalProfileReferenceId** para o **Id** do perfil técnico que criou anteriormente. Por exemplo, `LinkedIn-OAUTH`.

3. Guardar a *TrustFrameworkExtensions.xml* de ficheiro e carregá-lo novamente para a verificação.

## <a name="create-an-azure-ad-b2c-application"></a>Criar uma aplicação do Azure AD B2C

Comunicação com o Azure AD B2c ocorre por meio de um aplicativo que criar no seu inquilino. Esta secção lista os passos opcionais que pode seguir para criar uma aplicação de teste, se ainda não o tiver feito.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
3. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
4. Selecione **aplicativos**e, em seguida, selecione **Add**.
5. Introduza um nome para o aplicativo, por exemplo *testapp1*.
6. Para **aplicação Web / Web API**, selecione `Yes`e, em seguida, introduza `https://jwt.ms` para o **URL de resposta**.
7. Clique em **Criar**.

## <a name="update-and-test-the-relying-party-file"></a>Atualizar e testar o ficheiro da entidade confiadora de terceiros

Atualize o ficheiro da entidade confiadora de terceiros (RP) que inicia o percurso do utilizador que criou.

1. Faça uma cópia deles *SignUpOrSignIn.xml* no diretório de trabalho e renomeá-lo. Por exemplo, mude o nome para *SignUpSignInLinkedIn.xml*.
2. Abra o ficheiro novo e atualize o valor do **PolicyId** atributo **TrustFrameworkPolicy** com um valor exclusivo. Por exemplo, `SignUpSignInLinkedIn`.
3. Atualize o valor de **PublicPolicyUri** com o URI para a política. Por exemplo,`http://contoso.com/B2C_1A_signup_signin_linkedin`
4. Atualize o valor do **ReferenceId** atributo **DefaultUserJourney** de acordo com o ID do percurso do utilizador novo que criou (SignUpSignLinkedIn).
5. Guardar as alterações, carregue o ficheiro e, em seguida, selecione a nova política na lista.
6. Certifique-se de que a aplicação do Azure AD B2C que criou está selecionada na **selecione aplicativo** campo e, em seguida, testá-lo ao clicar em **executar agora**.
