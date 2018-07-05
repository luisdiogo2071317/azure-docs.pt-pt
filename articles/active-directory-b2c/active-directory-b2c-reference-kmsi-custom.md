---
title: Manter sessão iniciada no Azure Active Directory B2C | Documentos da Microsoft
description: Um tópico que demonstra como configurar a "manter sessão iniciada".
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6bad6e1f2b204f76b075652a9d3f27367a8de49f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441322"
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: Ativar o 'Manter-me conectado (KMSI)'  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O Azure AD B2C agora permite que a web e aplicativos nativos para ativar a funcionalidade 'Manter-me conectado (KMSI)'. Esta funcionalidade concede acesso a utilizadores a devolver à aplicação sem pedir confirmação para reintroduzir outro nome de utilizador e palavra-passe. Este acesso é revogado quando o utilizador terminar a sessão. 

Recomendamos a utilizadores, a verificação esta opção em computadores públicos. 

![img](images/kmsi.PNG)


## <a name="prerequisites"></a>Pré-requisitos

Um inquilino do Azure AD B2C, configurado para permitir que o local da conta sessão-inscrição/início de sessão, conforme descrito em [introdução ao](active-directory-b2c-get-started-custom.md).

## <a name="how-to-enable-kmsi"></a>Como ativar KMSI

Efetue as seguintes alterações na sua política de extensões do framework de confiança.

## <a name="adding-a-content-definition-element"></a>Adição de um elemento de definição de conteúdo 

O `BuildingBlocks` nó do seu ficheiro de extensão tem de incluir um `ContentDefinitions` elemento. 

1. Na `ContentDefinitions` secção, definir uma nova `ContentDefinition` com o ID `api.signuporsigninwithkmsi`.
2. A nova `ContentDefinition` tem de incluir um `LoadUri`, `RecoveryUri` e `DataUri` da seguinte forma.
3. URI de dados`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` é um identificador de compreensíveis de máquina que abre uma caixa de verificação KMSI nas páginas de início de sessão. Certifique-se de que não altere este valor. 

```XML
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.signuporsigninwithkmsi">
        <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
        <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        <Metadata>
          <Item Key="DisplayName">Signin and Signup</Item>
        </Metadata>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>                       
```



## <a name="add-a--local-account-sign-in-claims-provider"></a>Adicionar um fornecedor de afirmações de início de sessão de conta local 

Pode definir o início de sessão de conta Local como um fornecedor de afirmações para o `<ClaimsProvider>` nó do arquivo de extensão da política:

1. Abra o ficheiro de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho. 
2. Encontrar o `<ClaimsProviders>` secção. Se não existir, adicione-o sob o nó de raiz.
3. O pacote de iniciante da [introdução ao](active-directory-b2c-get-started-custom.md) vem com um fornecedor de afirmações de "Conta início de sessão Local". 
4. Caso contrário, adicione um novo `<ClaimsProvider>` nó da seguinte forma:

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
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
           </InputClaims>
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

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Criar um KMSI no percurso do utilizador ativada

Agora precisa adicionar o fornecedor de afirmações de início de sessão de conta Local para o seu percurso do utilizador. 

1. Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
2. Encontrar o `<UserJourneys>` elemento e copie todo o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"`.
3. Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
4. Cole a toda `<UserJourney>` nó que copiou como subordinado do `<UserJourneys>` elemento.
5. Mudar o nome o ID do percurso do utilizador novo (por exemplo, mudar o nome como `SignUpOrSignInWithKmsi`).
6. Por fim, no `OrchestrationStep 1` alterar o `ContentDefinitionReferenceId` para `api.signuporsigninwithkmsi` , definidas nos passos anteriores. Isto permite que a caixa de verificação no percurso do utilizador. 
7. Terminar de modificar o ficheiro de extensão. Guardar e carregar o ficheiro. Certifique-se de que todas as validações tenha êxito.

```XML
<UserJourneys>
    <UserJourney Id="SignUpOrSignInWithKmsi">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
              <Value>objectId</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <!-- This step reads any user attributes that we may not have received when in the token. -->
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

## <a name="create-a-relying-party-rp-file"></a>Crie um ficheiro da entidade confiadora de terceiros (RP)

Em seguida, atualize o ficheiro da entidade confiadora de terceiros (RP) que inicia o percurso do utilizador que criou:

1. Faça uma cópia do SignUpOrSignIn.xml no diretório de trabalho. Em seguida, mude o nome (por exemplo, SignUpOrSignInWithKmsi.xml).

2. Abra o ficheiro novo e a atualização do `PolicyId` atributo para `<TrustFrameworkPolicy>` com um valor exclusivo. Este é o nome da sua política (por exemplo, SignUpOrSignInWithKmsi).

3. Modificar a `ReferenceId` atributo `<DefaultUserJourney>` de acordo com o `Id` do novo percurso do utilizador que criou (por exemplo, SignUpOrSignInWithKmsi).

4. KMSI está configurado no `UserJourneyBehaviors`. 

5. **`KeepAliveInDays`** controla o tempo que o usuário permanece com sessão iniciada. No exemplo a seguir, sessão KMSI expira automaticamente depois de 14 dias, independentemente da frequência com que o utilizador efetua a autenticação silenciosa.

   Definição `KeepAliveInDays` valor como 0 desativa a funcionalidade KMSI. Por predefinição, este valor for 0

6. Se **`SessionExpiryType`** é *Rolling*, em seguida, a sessão KMSI é estendida por 14 dias sempre que o utilizador efetua a autenticação silenciosa.  Se *Rolling* é selecionado, recomendamos que mantenha o número de dias para mínimo. 

       <RelyingParty>
       <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
       <UserJourneyBehaviors>
         <SingleSignOn Scope="Tenant" KeepAliveInDays="14" />
         <SessionExpiryType>Absolute</SessionExpiryType>
         <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
       </UserJourneyBehaviors>
       <TechnicalProfile Id="PolicyProfile">
         <DisplayName>PolicyProfile</DisplayName>
         <Protocol Name="OpenIdConnect" />
         <OutputClaims>
           <OutputClaim ClaimTypeReferenceId="displayName" />
           <OutputClaim ClaimTypeReferenceId="givenName" />
           <OutputClaim ClaimTypeReferenceId="surname" />
           <OutputClaim ClaimTypeReferenceId="email" />
           <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
         </OutputClaims>
         <SubjectNamingInfo ClaimType="sub" />
       </TechnicalProfile>
       </RelyingParty>

7. Guardar as alterações e, em seguida, carregue o ficheiro.

8. Para testar a política personalizada que carregou, no portal do Azure, aceda ao painel de política e, em seguida, clique em **executar agora**.


## <a name="link-to-sample-policy"></a>Ligação à política de exemplo

Pode encontrar a política de exemplo [aqui](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








