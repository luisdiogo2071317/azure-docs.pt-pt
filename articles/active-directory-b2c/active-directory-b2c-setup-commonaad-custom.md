---
title: Adicionar um fornecedor de identidade do multi-inquilino do Azure AD com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Adicione um fornecedor de identidade do multi-inquilino do Azure AD com as políticas personalizadas - Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/14/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2a8a23245a17c9a80c70860588a8312dbbb5e926
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446080"
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>O Azure Active Directory B2C: Permitir que os utilizadores iniciem sessão a um fornecedor de identidade do multi-inquilino do Azure AD com as políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para utilizadores com o ponto de extremidade do multi-inquilino do Azure Active Directory (Azure AD) através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md). Isto permite aos utilizadores a partir de múltiplos inquilinos do Azure AD para iniciar sessão no Azure AD B2C sem configurar um fornecedor de técnico para cada inquilino. No entanto, os convidados membros em qualquer um desses inquilinos **não irá** conseguir iniciar sessão. Para isso, precisará [configurar individualmente cada inquilino](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
> Utilizamos "contoso.com" o organizacional para inquilino do Azure AD e "fabrikamb2c.onmicrosoft.com" nome do inquilino do Azure AD B2C nas instruções seguintes.

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:
     
1. Criar um Azure Active Directory B2C inquilino (Azure AD B2C).
1. Criar uma aplicação do Azure AD B2C.    
1. A registar as duas aplicações de motor de política.  
1. A configurar chaves. 
1. A configurar o pacote de iniciante.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>Passo 1. Criar uma aplicação de multi-inquilino do Azure AD

Para ativar o início de sessão para utilizadores com o multi-inquilino de ponto final do Azure, tem de ter uma aplicação de multi-inquilino registada dos seus inquilinos do Azure AD. Neste artigo, mostraremos como criar uma aplicação multi-inquilino do Azure AD no seu inquilino do Azure AD B2C. Em seguida, ative o início de sessão para os usuários por meio do uso desse multi-inquilino de aplicação do Azure AD.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Na barra superior, selecione a sua conta. Partir do **Directory** lista, escolha o inquilino do Azure AD B2C para registar a aplicação do Azure AD (fabrikamb2c.onmicrosoft.com).
1. Selecione **mais serviços** no painel esquerdo e procure "Registos de aplicações".
1. Selecione **Novo registo de aplicação**.
1. Introduza um nome para a sua aplicação (por exemplo, `Azure AD B2C App`).
1. Selecione **Aplicação/API Web** para o tipo de aplicação.
1. Para **URL de início de sessão**, introduza o URL seguinte, onde `yourtenant` é substituída pelo nome do inquilino do Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >O valor para "yourtenant" tem de estar todo em minúsculo no **URL de início de sessão**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Guarde o ID da aplicação.
1. Selecione a aplicação criada recentemente.
1. Sob o **configurações** painel, selecione **propriedades**.
1. Definir **vários inquilinos** ao **Sim**.
1. Sob o **configurações** painel, selecione **chaves**.
1. Criar uma nova chave e guarde-o. Irá utilizá-lo nos passos na secção seguinte.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>Passo 2. Adicionar a chave do Azure AD para o Azure AD B2C

Tem de registar a chave da aplicação nas definições do Azure AD B2C. Para efetuar este procedimento:

1. Vá para o menu de definições para o Azure AD B2C
1. Clique em **arquitetura de experiências de identidade** > **chaves da política**.
1. Selecione **+ adicionar**.
1. Selecione ou introduza estas opções:
   * Selecione **Manual**.
   * Para **Name**, escolha um nome que corresponde ao nome do seu inquilino do Azure AD (por exemplo, `AADAppSecret`).  O prefixo `B2C_1A_` é adicionado automaticamente o nome da sua chave.
   * Cole a chave de aplicação do **segredo** caixa.
   * Selecione **assinatura**.
1. Selecione **Criar**.
1. Confirme que criou a chave `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>Passo 3. Adicionar um fornecedor de afirmações na sua política base

Se pretender que os utilizadores iniciem sessão através da utilização do Azure AD, terá de definir do Azure AD como um fornecedor de afirmações. Em outras palavras, terá de especificar um ponto de final do Azure AD B2C irá comunicar com. O ponto final irá fornecer um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado. 

Pode definir do Azure AD como um fornecedor de afirmações ao Azure AD para adicionar o `<ClaimsProvider>` nó do arquivo de extensão da política:

1. Abra o ficheiro de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho.
1. Encontrar o `<ClaimsProviders>` secção. Se não existir, adicione-o sob o nó de raiz.
1. Adicionar um novo `<ClaimsProvider>` nó da seguinte forma:

```XML
<ClaimsProvider>
  <Domain>commonaad</Domain>
  <DisplayName>Common AAD</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Common-AAD">
      <DisplayName>Multi-Tenant AAD</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <!-- Update the Client ID below to the Application ID -->
        <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="scope">openid</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        
        <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

        <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
        <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->

      </Metadata>
      <CryptographicKeys>
      <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
        <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
      </CryptographicKeys>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
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

1. Sob o `<ClaimsProvider>` nó, atualize o valor para `<Domain>` para um valor exclusivo que pode ser utilizado para distinguir de outros fornecedores de identidade.
1. Sob o `<TechnicalProfile>` nó, atualize o valor para `<DisplayName>`. Este valor será apresentado no botão de início de sessão no seu ecrã de início de sessão.
1. Atualize o valor para `<Description>`.
1. Definir `<Item Key="client_id">` para o ID de aplicação do registo de aplicação de mulity inquilino do Azure AD.

### <a name="step-31-restrict-access-to-a-specific-list-of-azure-ad-tenants"></a>Passo 3.1 restringir o acesso a uma lista específica de inquilinos do Azure AD

> [!NOTE]
> Usando `https://sts.windows.net` como o valor de **ValidTokenIssuerPrefixes** permitirá que todos os utilizadores do Azure AD iniciar sessão na sua aplicação.

Terá de atualizar a lista de emissores de token válidas e restringir o acesso à lista específica de inquilinos do Azure AD, os utilizadores podem iniciar sessão. Para obter os valores, precisará examinar os metadados para cada um dos específico inquilinos do Azure AD que deseja que os utilizadores iniciem sessão a partir. O formato dos dados é semelhante ao seguinte: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, onde `yourAzureADtenant` é o seu nome de inquilino do Azure AD (contoso.com ou qualquer outro inquilino do Azure AD).
1. Abra o browser e aceda ao URL de metadados.
1. No browser, procure o objeto "emissor" e copie o seu valor. Deve ter um aspeto semelhante ao seguinte: `https://sts.windows.net/{tenantId}/`.
1. Cole o valor para o `ValidTokenIssuerPrefixes` chave. Pode adicionar várias, separando-as com uma vírgula. Um exemplo disso será inserido um comentário no exemplo acima de XML.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>Passo 4. Registar o fornecedor de afirmações de conta do Azure AD

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Passo 4.1 fazer uma cópia do percurso do utilizador

Agora precisa adicionar o fornecedor de identidade do Azure AD para um dos seus jornadas de utilizador. Neste momento, o fornecedor de identidade tiver sido configurado, mas não está disponível em qualquer uma dos ecrãs de início de sessão-inscrição/início de sessão.

Para tornar disponível, iremos criar um duplicado de um percurso do utilizador modelo existente e, em seguida, modificá-lo para que ele também tem o fornecedor de identidade do Azure AD:

1. Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
1. Encontrar o `<UserJourneys>` elemento e copie todo o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"`.
1. Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
1. Cole a toda `<UserJourney>` nó que copiou como subordinado do `<UserJourneys>` elemento.
1. Mudar o nome o ID do percurso do utilizador novo (por exemplo, mudar o nome como `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Passo 4.2 apresentar o "botão"

O `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade numa tela de início de sessão-inscrição/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para o Azure AD, um novo botão exibido quando um utilizador que chegam na página. Para adicionar este elemento:

1. Encontrar o `<OrchestrationStep>` nó que inclui `Order="1"` no percurso do utilizador que criou.
1. Adicione o seguinte:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Definir `TargetClaimsExchangeId` para um valor adequado. Recomendamos que siga a mesma Convenção outras pessoas:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Ligação de passo 4.3 no botão para uma ação

Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com o Azure AD para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações do AD do Azure:

1. Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
1. Adicione o seguinte:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Atualização `Id` para o mesmo valor que `TargetClaimsExchangeId` na secção anterior.
1. Atualização `TechnicalProfileReferenceId` para o ID do perfil técnico de que criou anteriormente (comum-AAD).

## <a name="step-5-create-a-new-rp-policy"></a>Passo 5: Criar uma nova política RP

Agora precisa atualizar o ficheiro da entidade confiadora de terceiros (RP) que irá iniciar o percurso do utilizador que acabou de criar:
 
1. Faça uma cópia do SignUpOrSignIn.xml no diretório de trabalho e mude o nome (por exemplo, renomeá-lo para SignUpOrSignInWithAAD.xml).  
1. Abra o ficheiro novo e a atualização do `PolicyId` atributo para `<TrustFrameworkPolicy>` com um valor exclusivo (por exemplo, SignUpOrSignInWithAAD). Este será o nome da política (por exemplo, B2C\_1A\_SignUpOrSignInWithAAD). 
1. Modificar a `ReferenceId` atributo `<DefaultUserJourney>` de acordo com o ID do percurso do utilizador novo que criou (SignUpOrSignUsingAzureAD). 
1. Guardar as alterações e carregar o ficheiro. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Passo 6: Carregar a política para o seu inquilino

1. Na [portal do Azure](https://portal.azure.com), mude para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e, em seguida, selecione **do Azure AD B2C**.
1. Selecione **arquitetura de experiências de identidade**.
1. Selecione **todas as políticas**.
1. Selecione **carregar política**.
1. Selecione o **substituir a política, se existir** caixa de verificação.
1. Carregar o `TrustFrameworkExtensions.xml` arquivo e o arquivo RP (por exemplo, `SignUpOrSignInWithAAD.xml`) e certifique-se de que eles passarem na validação.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Passo 7: Testar a política personalizada com executar agora

1. Selecione **definições do Azure AD B2C**e, em seguida, selecione **Framework de experiência de identidade**.
    > [!NOTE]
    > Executar agora requer, pelo menos, um aplicativo para ser foi pré-registado no inquilino. Para saber como registar aplicações, veja o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.

1. Abra a política personalizada de terceiros (RP) da entidade confiadora que carregou (*B2C\_1A\_SignUpOrSignInWithAAD*) e, em seguida, selecione **executar agora**.
1. Deve agora conseguir iniciar sessão com a conta do Azure AD.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(Opcional) Passo 8: Registar o fornecedor de afirmações de conta do Azure AD para o percurso do utilizador de edição de perfil

Pode também querer adicionar o fornecedor de identidade de conta do Azure AD para sua `ProfileEdit` percurso do utilizador. Para disponibilizar o percurso do utilizador, repita os passos 4 a 6. Desta vez, selecione o `<UserJourney>` nó que contém `Id="ProfileEdit"`. Guardar e carregar para testar a sua política.

## <a name="troubleshooting"></a>Resolução de problemas

Para diagnosticar problemas, leia sobre [resolução de problemas](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Passos Seguintes

Fornecer feedback para [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
