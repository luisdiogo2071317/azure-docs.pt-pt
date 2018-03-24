---
title: Como controlar o comportamento do utilizador com eventos no Application Insights do Azure AD B2C | Microsoft Docs
description: Guia passo a passo para ativar registos de eventos no Application Insights do Azure AD B2C percursos de utilizador através de políticas personalizadas - PREVIEW
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 1e8c4a53266072db71952ee35b15e5de54fabb95
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="tracking-user-behavior-inside-azure-ad-b2c-journeys-using-application-insights"></a>Controlar o comportamento do utilizador no interior de percursos de Azure AD B2C, utilizar o Application Insights

O Azure Active Directory B2C funciona bem com o Azure Application Insights.  Fornecem registos de eventos detalhados e personalizados para os percursos de utilizador criada personalizado.  Este guia mostra como começar a utilizar para que as possa: 
* obter informações sobre o comportamento do utilizador
* resolver problemas relacionados com as suas políticas de desenvolvimento ou na produção
* medida de desempenho
* Criar notificações do Application Insights

## <a name="how-it-works"></a>Como funciona
Foi adicionado um novo fornecedor para o Framework de experiência de identidade do Azure AD B2C: `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Envia dados de eventos diretamente para o Application Insights utilizando a chave de instrumentação fornecido para o Azure AD B2C.  Um perfil técnico utiliza este fornecedor para definir um evento do B2C.  O perfil Especifica o nome do evento, as afirmações que serão registadas e a chave de instrumentação.  Publicar um evento, o perfil técnico, em seguida, é adicionado como estou `orchestration step` ou como um `validation technical profile` no journey utilizador personalizado.  Os eventos podem ser unificados pelo Application Insights utilizando um ID de correlação para gravar uma sessão de utilizador.  Application Insights disponibiliza os eventos e a sessão dentro de segundos e apresenta muitos visualização, exportação e ferramentas analíticas.



## <a name="prerequisites"></a>Pré-requisitos
Concluir os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md).  Este guia pressupõe que está a ser utilizado o pacote de arranque de política personalizada.  No entanto não é necessária.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Passo 1. Crie um recurso do Application Insights e obter a chave de instrumentação

Application Insights é uma ferramenta poderosa. Quando a utilizar com o Azure AD B2C, é o único requisito criar um recurso e obter uma chave de instrumentação.  Application Insights tem de ser criadas no [portal do Azure.](https://portal.azure.com)

[Documentação completa para o Application Insights](https://docs.microsoft.com/azure/application-insights/)

1. Clique em `+ Create a resource` no portal do Azure, no seu inquilino da subscrição.  Este é o inquilino não o inquilino do Azure AD B2C.  
2. Procure e selecione `Application Insights`  
3. Crie um recurso de `Application Type` `ASP.NET web application` numa subscrição da sua preferência.
4. Depois de criado, abra o recurso do Application Insights e tenha em atenção o  `Instrumentation Key` 

![Descrição geral do Application Insights e chave de instrumentação](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Passo 2. Adicionar novas definições de ClaimType no ficheiro de extensão confiar Framework

### <a name="open-the-extension-file-from-the-starter-pack-and-add-the-following-elements-to-the-buildingblocks-node--the-extensions-filename-is-typically-yourtenantonmicrosoftcom-b2c1atrustframeworkextensionsxml"></a>Abra o ficheiro de extensão do pacote de arranque e adicione os seguintes elementos para o `<BuildingBlocks>` nós.  O nome de ficheiro de extensões é normalmente `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

```xml

    <ClaimsSchema>
      <ClaimType Id="EventType">
        <DisplayName>EventType</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="PolicyId">
        <DisplayName>PolicyId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="Culture">
        <DisplayName>Culture</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="CorrelationId">
        <DisplayName>CorrelationId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <!--Additional claims used for passing claims to Application Insights Provider -->
      <ClaimType Id="federatedUser">
        <DisplayName>federatedUser</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="parsedDomain">
        <DisplayName>Parsed Domain</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>The domain portion of the email address.</UserHelpText>
      </ClaimType>
      <ClaimType Id="userInLocalDirectory">
        <DisplayName>userInLocalDirectory</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
    </ClaimsSchema>

```

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Passo 3. Adicionar novos perfis técnicos que utilizam o fornecedor do Application Insights

Perfis técnicas podem ser consideradas funções na estrutura de experiência de identidade do Azure AD B2C.  Cinco perfis técnico é definido neste exemplo, para abrir uma sessão e publicar eventos.

| Perfil técnica       | Tarefa |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | conjunto comum de parâmetros para ser incluída em todos os perfis de técnicos de informações do Azure     | 
| JourneyContextForInsights   | Abre-se a sessão na aplicação Insights e envia um ID de correlação |
| AzureInsights-SignInRequest     | cria um evento de "Início de sessão" com um conjunto de afirmações quando foi recebido um pedido de início de sessão      | 
| AzureInsights-UserSignup | cria um evento chamado "UserSignup" quando a opção de inscrição foi acionada pelo utilizador no journey início de sessão/inscrição     | 
| AzureInsights-SignInComplete | regista a conclusão com êxito de uma autenticação quando foi enviado um token para a aplicação da entidade confiadora     | 

Adicione os perfis para o ficheiro de extensão do pacote de arranque adicionando estes elementos para o `<ClaimsProviders>` nós.  O nome de ficheiro de extensões é normalmente `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Alterar o `Instrumentation Key` no `ApplicationInsights-Common` perfil técnica para o GUID fornecido pelo recurso do Application Insights.

```xml
<ClaimsProvider>
      <DisplayName>Application Insights</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="JourneyContextForInsights">
          <DisplayName>Application Insights</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="CorrelationId" />
          </OutputClaims>
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInRequest">
          <InputClaims>
            <!-- 
                            An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider
                            to create an event with the specified value.
                        -->
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInComplete">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
            <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
            <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-UserSignup">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignup" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-Common">
          <DisplayName>Alternate Email</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
            <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
            <!-- 
                            A boolean indicating whether delevoper mode is enabled. This controls how events are buffered. In a development environment
                            with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights.
                        -->
            <Item Key="DeveloperMode">false</Item>
            <!-- 
                            A boolean indicating whether telemetry should be enabled or not.
                        -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!--
                            Properties of an event are added using the syntax {property:NAME} where NAME is the name of the property being added
                            to the event. DefaultValue can be either a static value or one resolved by one of the supported DefaultClaimResolvers.
                        -->
            <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
            <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
            <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Passo 4. Adicione os perfis técnicos para o Application Insights como orchestration os passos num Journey de utilizador existente.

Chamar `JournyeContextForInsights` como passo 1 da orquestração

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Chamar `Azure-Insights-SignInRequest` como orchestration passo 2 para controlar o que foi recebido um pedido sessão-na/sessão-up.

```xml
<!-- Track that we have received a sign in request -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Imediatamente **antes** o `SendClaims` orchestration passo, adicione um novo passo que chame `Azure-Insights-UserSignup`. Que é acionado quando o botão de inscrição tem sido clicado na journey início de sessão/inscrição.

```xml
        <!-- Handles the user clicking the sign up link in the local account sign in page -->
        <OrchestrationStep Order="9" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
              <Value>newUser</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
            <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
              <Value>newUser</Value>
              <Value>false</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AzureInsights-UserSignup" />
          </ClaimsExchanges>
```

Imediatamente após o `SendClaims` passo de orquestração, chamada `Azure-Insights-SignInComplete`.   Este passo irá refletir um journey concluída com êxito.

```xml
        <!-- Track that we have successfully sent a token -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Depois de adicionar as novo etapas da orquestração, renumber os passos sequencialmente sem ignorar a qualquer números inteiros de 1 para N


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Passo 5. Carregar o ficheiro modificado extensões, execute a política e ver eventos no Application Insights

Guardar e carregar o novo ficheiro de extensões do framework de confiança.  Em seguida, chame a política de entidade Confiadora da aplicação ou utilize `Run Now` na interface do Azure AD B2C.  Em segundos, os eventos estará disponíveis no Application Insights.

1. Abra o recurso do Application Insights no seu inquilino do Azure Active Directory.
2. Clique em `Events` no `USAGE` o submenu.
3. Definir `During` para `Last hour` e `By` para `3 minutes`.  Poderá ter de clicar em `Refresh` para ver os resultados

![Application Insights-eventos de utilização Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





##  <a name="next-steps"></a>PASSOS SEGUINTES

Adicione tipos de afirmação adicionais e eventos da sua viagem de utilizador para se ajustarem às suas necessidades.  Aqui está uma lista de possíveis afirmações utilizar resoluções de afirmações adicionais.

### <a name="culture-specific-claims"></a>Afirmações de cultura específica

```xml
Culture-specific Claims
            Referenced using {Culture:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="JourneyCultureDefaultClaimProcessor"/>
        public enum SupportedClaim
        {
             /// The two letter ISO code for the language i.e. en
            LanguageName
             
            /// The two letter ISO code for the region i.e. US
            RegionName,
 
            /// The RFC5646 language code i.e. en-US
            RFC5646,

            /// The LCID of language code i.e. 1033
            LCID
        }

```

### <a name="policy-specific-claims"></a>Específico da política de afirmações

```xml
Policy-specific Claims
Referenced using {Policy:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="PolicyDefaultClaimProcessor"/> 
        public enum SupportedClaim
        {
            /// The trustframework tenant id
            TrustFrameworkTenantId,
  
            /// The tenant id of the relying party
            RelyingPartyTenantId,
 
            /// The policy id of the policy
            PolicyId,
 
            /// The tenant object id of the policy
            TenantObjectId
}

```

### <a name="openidconnect-specific-claims"></a>Afirmações específicas OpenIDConnect

```xml
OpenIDConnect Specific Claims
Referenced using {OIDC:One of the property names below}
 
/// 
        /// An enumeration of the claims supported by the <see cref="OpenIdConnectDefaultClaimProcessor"/>

        public enum SupportedClaim
        {
            /// The OpenIdConnect prompt parameter
            Prompt,
 
            /// The OpenIdConnect login_hint parameter
            LoginHint,

            /// The OpenIdConnect domain_hint parameter
            DomainHint,
 
             /// The OpenIdConnect max_age parameter
            MaxAge,
 
            /// The OpenIdConnect client_id parameter
            ClientId,
 
            /// The OpenIdConnect username parameter
            Username,

            /// The OpenIdConnect password parameter
            Password,
 
            /// The OpenIdConnect resource type parameter
            Resource,
 
            /// The OpendIdConext acr_values parameter
             AuthenticationContextReferences
        }
 
```

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>Os parâmetros de protocolo não incluído com OIDC & OAuth2 pedidos

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Qualquer nome de parâmetro incluída como parte de um pedido OIDC ou OAuth2 pode ser mapeada para uma afirmação na journey do utilizador.  Em seguida, podem ser gravado no evento. Por exemplo, o pedido da aplicação pode incluir um parâmetro de cadeia de consulta com um nome de `app_session`, `loyalty_number` ou `any_string`.

Exemplo de pedido da aplicação:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
As afirmações, em seguida, podem ser adicionadas ao adicionar um elemento de afirmação de entrada para a utilização de eventos de Application Insights:
```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="app_session" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="loyalty_number" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Outras afirmações de sistema

Alguns afirmações sistema tem de ser adicionadas para a matriz de afirmações antes de serem disponíveis para gravar como eventos. O perfil técnico `SimpleUJContext` tem de ser chamado como um passo de orquestração ou um perfil de técnicas de validação antes destes afirmações estão disponíveis.

```xml
<ClaimsProvider>
    <DisplayName>User Journey Context Provider</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="SimpleUJContext">
                <DisplayName>User Journey Context Provide</DisplayName>
                <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="IP-Address" />
                    <OutputClaim ClaimTypeReferenceId="CorrelationId" />
                    <OutputClaim ClaimTypeReferenceId="DateTimeInUtc" />
                    <OutputClaim ClaimTypeReferenceId="Build" />
                 </OutputClaims>
            </TechnicalProfile>
        </TechnicalProfiles>
</ClaimsProvider>



