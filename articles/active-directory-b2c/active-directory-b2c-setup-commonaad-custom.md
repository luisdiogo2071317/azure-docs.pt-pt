---
title: Adicionar um fornecedor de identidade do multi-inquilino do Azure AD através de políticas personalizadas - Azure Active Directory B2C | Microsoft Docs
description: Adicionar um fornecedor de identidade do multi-inquilino do Azure AD através de políticas personalizadas - Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: parakhj
manager: alexsi
editor: parakhj
ms.assetid: 33c64001-5261-4ed9-8f46-b09839165250
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/14/2018
ms.author: parakhj
ms.openlocfilehash: cff5c1eed374683ad3e2c1f1a69f6f172f36c536
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>O Azure Active Directory B2C: Permitir aos utilizadores iniciar sessão para um fornecedor de identidade do multi-inquilino do Azure AD através de políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra como ativar o início de sessão para utilizadores que utilizam o ponto final comum para o Azure Active Directory (Azure AD) através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md).

>[!NOTE]
> Utilizamos "contoso.com" para o organizacional inquilino do Azure AD e "fabrikamb2c.onmicrosoft.com" do inquilino do Azure AD B2C nas instruções seguintes.

## <a name="prerequisites"></a>Pré-requisitos

Concluir os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:
     
1. Criar um Azure Active Directory B2C inquilino (Azure AD B2C).
2. Criar uma aplicação do Azure AD B2C.    
3. Registar duas aplicações de motor de política.  
4. Definição de segurança das chaves. 
5. Configurar o pacote de arranque.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>Passo 1. Criar uma aplicação multi-inquilino do Azure AD

Para ativar o início de sessão para utilizadores que utilizam o multi-inquilino ponto final do Azure AD, tem de ter uma aplicação de multi-inquilino registada em nenhum dos seus inquilinos do Azure AD. Neste artigo, vamos mostrar como criar uma aplicação multi-inquilino do Azure AD no seu inquilino do Azure AD B2C. Em seguida, ative o início de sessão para utilizadores através da utilização que multi-inquilino de aplicação do Azure AD.

>[!NOTE]
> Se pretender que os utilizadores do Azure AD **e os utilizadores com contas Microsoft** para iniciar sessão, ignore esta secção e, em vez disso, registar uma aplicação no [portal de programador do Microsoft](https://apps.dev.microsoft.com).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Na barra superior, selecione a sua conta. Do **diretório** lista, escolha o inquilino do Azure AD B2C para registar a aplicação do Azure AD (fabrikamb2c.onmicrosoft.com).
2. Selecione **mais serviços** no painel esquerdo e procure "Registos de aplicação".
3. Selecione **Novo registo de aplicação**.
4. Introduza um nome para a sua aplicação (por exemplo, `Azure AD B2C App`).
5. Selecione **Aplicação/API Web** para o tipo de aplicação.
6. Para **URL de início de sessão**, introduza o seguinte URL onde `yourtenant` é substituído pelo nome do seu inquilino do Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >O valor para "yourtenant" deve ser todo em minúsculas no **URL de início de sessão**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Guardar o ID da aplicação.
1. Selecione a aplicação criada recentemente.
1. Sob o **definições** painel, selecione **propriedades**.
1. Definir **tenanted de várias** para **Sim**.
1. Sob o **definições** painel, selecione **chaves**.
1. Criar uma nova chave e guarde-o. Irá utilizá-lo nos passos na secção seguinte.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>Passo 2. Adicionar a chave do Azure AD para o Azure AD B2C

Tem de registar a chave de aplicação nas definições do Azure AD B2C. Para efetuar este procedimento:

1. Vá para o menu de definições para o Azure AD B2C
1. Clique em **identidade experiência Framework** > **política chaves**.
1. Selecione **+ adicionar**.
1. Selecione ou introduza estas opções:
   * Selecione **Manual**.
   * Para **nome**, escolha um nome que corresponde ao nome do seu inquilino do Azure AD (por exemplo, `AADAppSecret`).  O prefixo `B2C_1A_` é adicionado automaticamente para o nome da sua chave.
   * Colar a chave de aplicação no **segredo** caixa.
   * Selecione **assinatura**.
5. Selecione **Criar**.
6. Confirme que criou a chave `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>Passo 3. Adicionar um fornecedor de afirmações na política de base

Se pretender que os utilizadores para iniciar sessão utilizando o Azure AD, terá de definir do Azure AD como um fornecedor de afirmações. Por outras palavras, tem de especificar um ponto final que o Azure AD B2C irão comunicar com. O ponto final irá fornecer um conjunto de afirmações que são utilizados pelo Azure AD B2C para verificar que um utilizador específico foi autenticado. 

Pode definir do Azure AD como um fornecedor de afirmações, adicionando o Azure AD para o `<ClaimsProvider>` no ficheiro de extensão da sua política de nó:

1. Abra o ficheiro de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho.
1. Localizar o `<ClaimsProviders>` secção. Se não existir, adicione-a sob o nó de raiz.
1. Adicione um novo `<ClaimsProvider>` nó da seguinte forma:

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
        
        <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. If you would like only specific tenants to be able to sign in, uncomment the line below and update the GUIDs. -->
        <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item> -->

        <!-- The commented key below specifies that users from any tenant can sign-in. Comment or remove the line below if using the line above. -->
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
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
1. Definir `<Item Key="client_id">` para o ID da aplicação do registo de aplicação de mulity inquilino do Azure AD.

### <a name="step-31-optional-restrict-access-to-specific-list-of-azure-ad-tenants"></a>Passo 3.1 [opcional] restringir acesso à lista específica de inquilinos do Azure AD
Poderá querer atualizar a lista de emissores de token válidas e restringir o acesso à lista específica de inquilinos do Azure AD os utilizadores podem iniciar sessão. Para obter os valores, terá de observe os metadados para cada um dos especificada inquilinos do Azure AD que gostaria de a indicar aos utilizadores iniciar sessão a partir. O formato dos dados assemelha ao seguinte: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, onde `yourAzureADtenant` é o nome de inquilino do Azure AD (contoso.com ou qualquer outro inquilino do Azure AD).
1. Abra o browser e avance para o URL de metadados.
1. No browser, procure o objeto 'emissor' e copie o valor. Deve ser semelhante ao seguinte: `https://sts.windows.net/{tenantId}/`.
1. Colar o valor para o `ValidTokenIssuerPrefixes` chave. Pode adicionar várias, separando-as com uma vírgula. Um exemplo desta situação é comentado no exemplo acima de XML.

> [!NOTE]
> Utilizar `https://sts.windows.net` como um valor de prefixo irá permitir que todos os utilizadores do Azure AD iniciar sessão na sua aplicação.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>Passo 4. Registar o fornecedor de afirmações de conta do Azure AD

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Passo 4.1 fazer uma cópia de journey do utilizador

Agora tem de adicionar o fornecedor de identidade do Azure AD para uma das suas percursos de utilizador. Neste momento, o fornecedor de identidade tiver sido configurado, mas não está disponível em nenhum dos ecrãs de sessão-up/início de sessão.

Para tornar disponível, iremos criar um duplicado de um existente journey de utilizador do modelo e, em seguida, modificá-lo para que ela também tem o fornecedor de identidade do Azure AD:

1. Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
1. Localizar o `<UserJourneys>` elemento e copie todo o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"`.
1. Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione uma.
1. Cole a toda a `<UserJourney>` nó que copiou como subordinado do `<UserJourneys>` elemento.
1. Mudar o nome de ID de journey de utilizador novo (por exemplo, mudar o nome como `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Passo 4.2 apresentar no botão""

O `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade num ecrã de início de sessão-up/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para o Azure AD, um novo botão aparece quando um utilizador lands na página. Para adicionar este elemento:

1. Localizar o `<OrchestrationStep>` nó que inclui `Order="1"` no journey utilizador que criou.
1. Adicione o seguinte:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Definir `TargetClaimsExchangeId` para um valor adequado. Recomendamos seguindo a mesma Convenção como outros:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Passo 4.3 ligação no botão para uma ação

Agora que tem um botão no local, terá de ligá-la a uma ação. Neste caso, é a ação para o Azure AD B2C comunicar com o Azure AD para receber um token. O botão de ligação para uma ação associando o perfil técnico para o seu fornecedor de afirmações do Azure AD:

1. Localizar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nós.
1. Adicione o seguinte:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Atualização `Id` para o mesmo valor da `TargetClaimsExchangeId` na secção anterior.
1. Atualização `TechnicalProfileReferenceId` para o ID do perfil técnico que criou anteriormente (comuns-AAD).

## <a name="step-5-create-a-new-rp-policy"></a>Passo 5: Criar uma nova política RP

Agora tem de atualizar o ficheiro de terceiros (RP) entidade confiadora que iniciará a journey de utilizador que acabou de criar:
 
1. Fazer uma cópia de SignUpOrSignIn.xml no seu diretório de trabalho e mude o nome (por exemplo, mude o nome para SignUpOrSignInWithAAD.xml).  
1. Abra o ficheiro novo e a atualização a `PolicyId` atributo para `<TrustFrameworkPolicy>` com um valor exclusivo (por exemplo, SignUpOrSignInWithAAD). Este será o nome da sua política (por exemplo, B2C\_1A\_SignUpOrSignInWithAAD). 
1. Modificar o `ReferenceId` atributo em `<DefaultUserJourney>` para corresponder ao ID do novo journey de utilizador que criou (SignUpOrSignUsingAzureAD). 
1. Guardar as alterações e carregar o ficheiro. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Passo 6: Carregar a política para o seu inquilino

1. No [portal do Azure](https://portal.azure.com), mude para o [contexto do seu inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e, em seguida, selecione **do Azure AD B2C**.
2. Selecione **identidade experiência Framework**.
3. Selecione **todas as políticas**.
4. Selecione **carregar política**.
5. Selecione o **substituir a política se existir** caixa de verificação.
6. Carregar o `TrustFrameworkExtensions.xml` ficheiro e o ficheiro RP (por exemplo, `SignUpOrSignInWithAAD.xml`) e certifique-se de que aprovado na validação.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Passo 7: Testar a política personalizada utilizando o executar agora

1. Selecione **definições do Azure AD B2C**e, em seguida, selecione **identidade experiência Framework**.
    > [!NOTE]
    > Executar agora requer, pelo menos, uma aplicação para preregistered no inquilino. Para aprender a registar aplicações, consulte o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.

1. Abra a entidade confiadora (RP) de terceiros personalizada de política que carregou (*B2C\_1A\_SignUpOrSignInWithAAD*) e, em seguida, selecione **executar agora**.
1. Agora deverá conseguir iniciar sessão utilizando a conta do Azure AD.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(Opcional) Passo 8: Registar o fornecedor de afirmações de conta do Azure AD para o journey de utilizador de edição de perfil

Pode também querer adicionar o fornecedor de identidade de conta do Azure AD para o `ProfileEdit` journey de utilizador. Para disponibilizar o journey de utilizador, repita os passos 4 a 6. De momento, selecione o `<UserJourney>` nó que contém `Id="ProfileEdit"`. Guardar, carregar e testar a sua política.

## <a name="troubleshooting"></a>Resolução de problemas

Para diagnosticar problemas, leia sobre [resolução de problemas](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Passos Seguintes

Fornecer comentários para [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
