---
title: Alteração de palavra-passe self-service no Azure Active Directory B2C | Documentos da Microsoft
description: Um tópico que demonstra como configurar a alteração de palavra-passe self-service para os consumidores no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 491b3988a6581387c71b4214907e689119fcb979
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344994"
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>O Azure Active Directory B2C: Configurar a alteração de palavra-passe em políticas personalizadas  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Com a funcionalidade de alteração de palavra-passe, consumidores de início de sessão iniciada (usando contas locais) podem alterar as palavras-passe sem ter de provar sua autenticidade por verificação de e-mail, conforme descrito no [fluxo de reposição de palavra-passe self-service.](active-directory-b2c-reference-sspr.md) Se a sessão expirar, o tempo que o consumidor obtém a palavra-passe de fluxo de alteração, é pedido ao utilizador para iniciar sessão novamente. 

## <a name="prerequisites"></a>Pré-requisitos

Um inquilino de B2C do Azure AD configurado para concluir uma conta local sessão-inscrição/início de sessão, conforme descrito em [introdução ao](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-change-in-custom-policy"></a>Como configurar a alteração de palavra-passe numa política personalizada

Para configurar a alteração de palavra-passe numa política personalizada do efetuar as seguintes alterações na sua política de extensões de estrutura de confiança, 

## <a name="define-a-claimtype-oldpassword"></a>Definir um ClaimType 'oldPassword'

A estrutura geral da política personalizada tem de incluir um `ClaimsSchema`e definir um novo `ClaimType` oldPassword conforme mostrado a seguir, 

```XML
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="oldPassword">
        <DisplayName>Old Password</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>Enter password</UserHelpText>
        <UserInputType>Password</UserInputType>
      </ClaimType>
    </ClaimsSchema>
  </BuildingBlocks>
```

A finalidade desses elementos é o seguinte:

- O `ClaimsSchema` define que afirmações está sendo validada.  Neste caso, a "palavra-passe antiga' vai ser validado. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Adicionar um fornecedor de afirmações de alteração de palavra-passe com seus elementos com suporte

Alterar a palavra-passe será de fornecedor de afirmações

1. Autenticar o utilizador em relação a palavra-passe antiga
2. E se 'nova palavra-passe' corresponde a "Confirme a nova palavra-passe", este valor é armazenado no arquivo de dados do B2C e, por conseguinte, a palavra-passe é alterada com êxito. 

![img](images/passwordchange.jpg)

Adicione o fornecedor de afirmações seguintes na sua política de extensões. 

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="login-NonInteractive">
          <Metadata>
           <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
           <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="login-NonInteractive-PasswordChange">
          <DisplayName>Local Account SignIn</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
            <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
            <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
            <Item Key="ProviderName">https://sts.windows.net/</Item>
            <Item Key="METADATA">https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/.well-known/openid-configuration</Item>
            <Item Key="authorization_endpoint">https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/token</Item>
            <Item Key="response_types">id_token</Item>
            <Item Key="response_mode">query</Item>
            <Item Key="scope">email openid</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
            <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
            <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
            <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
            <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Local Account Password Change</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
          <DisplayName>Change password (username)</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
          </CryptographicKeys>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
            <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
```



### <a name="add-the-application-ids-to-your-custom-policy"></a>Adicionar os IDs de aplicação à sua política personalizada

Adicionar os IDs de aplicação para o arquivo de extensões (`TrustFrameworkExtensions.xml`):

1. O arquivo de extensões (TrustFrameworkExtensions.xml), localize o elemento `<TechnicalProfile Id="login-NonInteractive">` e `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Substitua todas as instâncias de `IdentityExperienceFrameworkAppId` com o ID da aplicação a arquitetura de experiências de identidade, conforme descrito em [introdução](active-directory-b2c-get-started-custom.md). Segue-se um exemplo:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Substitua todas as instâncias de `ProxyIdentityExperienceFrameworkAppId` com o ID da aplicação a arquitetura de experiências de identidade de Proxy, conforme descrito em [introdução](active-directory-b2c-get-started-custom.md).

4. Guarde o ficheiro de extensões.



## <a name="create-a-password-change-user-journey"></a>Criar um percurso de utilizador de alteração de palavra-passe

```XML
 <UserJourneys>
    <UserJourney Id="PasswordChange">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

Terminar de modificar o ficheiro de extensão. Guardar e carregar o ficheiro. Certifique-se de que todas as validações tenha êxito.



## <a name="create-a-relying-party-rp-file"></a>Crie um ficheiro da entidade confiadora de terceiros (RP)

Em seguida, atualize o ficheiro da entidade confiadora de terceiros (RP) que inicia o percurso do utilizador que criou:

1. Faça uma cópia do ProfileEdit.xml no diretório de trabalho. Em seguida, mude o nome (por exemplo, PasswordChange.xml).
2. Abra o ficheiro novo e a atualização do `PolicyId` atributo para `<TrustFrameworkPolicy>` com um valor exclusivo. Este é o nome da sua política (por exemplo, PasswordChange).
3. Modificar a `ReferenceId` atributo `<DefaultUserJourney>` de acordo com o `Id` do novo percurso do utilizador que criou (por exemplo, PasswordChange).
4. Guardar as alterações e, em seguida, carregue o ficheiro.
5. Para testar a política personalizada que carregou, no portal do Azure, aceda ao painel de política e, em seguida, clique em **executar agora**.




## <a name="link-to-password-change-sample-policy"></a>Ligação à política de exemplo de alteração de palavra-passe

Pode encontrar a política de exemplo [aqui](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










