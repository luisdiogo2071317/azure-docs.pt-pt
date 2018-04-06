---
title: Controlar o comportamento do utilizador utilizando os eventos no Application Insights do Azure AD B2C | Microsoft Docs
description: Guia passo a passo para ativar registos de eventos no Application Insights do percursos de utilizador do Azure AD B2C ao utilizar políticas personalizadas (pré-visualização)
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 28c4cefdd7604dbddf6887dcf494ecea65d658f1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Controlar o comportamento de utilizador no Azure AD B2C percursos utilizando o Application Insights

O Azure Active Directory B2C (Azure AD B2C) funciona bem com o Azure Application Insights. Fornecem registos de eventos detalhados e personalizados para os percursos de utilizador personalizadas criadas. Este artigo mostra como começar a utilizar para que as possa: 
* Obter informações sobre o comportamento do utilizador.
* Resolver problemas relacionados com as suas políticas de desenvolvimento ou na produção.
* Medida de desempenho.
* Crie notificações do Application Insights.

> [!NOTE]
> Esta funcionalidade está em pré-visualização.

## <a name="how-it-works"></a>Como funciona
A estrutura de experiência de identidade no Azure AD B2C inclui agora o fornecedor `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`. Envia dados de eventos diretamente para o Application Insights, utilizando a chave de instrumentação fornecida para o Azure AD B2C. 

Um perfil técnico utiliza este fornecedor para definir um evento do B2C. O perfil Especifica o nome do evento, as afirmações que serão registadas e a chave de instrumentação. Para publicar um evento, o perfil técnico, em seguida, é adicionado como um *passo orchestration* ou como um *perfil técnica de validação* no journey utilizador personalizado. 

Application Insights podem unificar os eventos utilizando um ID de correlação para gravar uma sessão de utilizador. Application Insights disponibiliza os eventos e a sessão em segundos e apresenta muitos visualização, exportação e ferramentas analíticas.



## <a name="prerequisites"></a>Pré-requisitos
Execute os passos do [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md). Este artigo pressupõe que está a utilizar o pacote de arranque de política personalizada. Mas o pacote de arranque não é necessário.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Passo 1. Crie um recurso do Application Insights e obter a chave de instrumentação

Quando estiver a utilizar o Application Insights com o Azure AD B2C, é o único requisito criar um recurso e obter uma chave de instrumentação. Criar um recurso de [portal do Azure](https://portal.azure.com).

1. No portal do Azure, no seu inquilino de subscrição, selecione **+ criar um recurso**. Este é o inquilino não o inquilino do Azure AD B2C.  
2. Procure e selecione **Application Insights**.  
3. Crie um recurso que utiliza **aplicação web ASP.NET** como **tipo de aplicação**, numa subscrição da sua preferência.
4. Depois de criar o recurso do Application Insights, abra-o e tenha em atenção a chave de instrumentação. 

![Application Insights descrição geral e instrumentação da chave](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

Para obter mais informações, consulte o [completa documentação para o Application Insights](https://docs.microsoft.com/azure/application-insights/).

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Passo 2. Adicionar novas definições de ClaimType ao seu ficheiro de extensão de estrutura de confiança

Abra o ficheiro de extensão do pacote de arranque e adicione os seguintes elementos para o `<BuildingBlocks>` nós. O nome de ficheiro é normalmente `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

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
      <!-- Additional claims used for passing claims to the Application Insights provider. -->
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

Perfis técnicos podem ser consideradas as funções no identidade experiência Framework do Azure AD B2C. Neste exemplo define os cinco perfis técnicos para abrir uma sessão e publicar eventos:

| Perfil técnica       | Tarefa |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | Cria um conjunto comum de parâmetros a ser incluído em todas as `AzureInsights` perfis técnicas     | 
| JourneyContextForInsights   | Abre-se a sessão no Application Insights e envia um ID de correlação |
| AzureInsights-SignInRequest     | Cria um `SignIn` eventos com um conjunto de afirmações quando foi recebido um pedido de início de sessão      | 
| AzureInsights-UserSignup | Cria um `UserSignup` eventos quando o utilizador aciona a opção de inscrição num journey sessão-up/início de sessão     | 
| AzureInsights-SignInComplete | regista a conclusão com êxito de uma autenticação quando foi enviado um token para a aplicação da entidade confiadora     | 

Adicione os perfis para o ficheiro de extensão do pacote de arranque adicionando estes elementos para o `<ClaimsProviders>` nós. O nome de ficheiro é normalmente `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

> [!IMPORTANT]
> Alterar a chave de instrumentação no `ApplicationInsights-Common` perfil técnica para o GUID que fornece o recurso do Application Insights.

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
            <!-- An input claim with PartnerClaimType="eventName" is required. The Application Insights provider uses it to create an event with the specified value. -->
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
            <!-- The Application Insights instrumentation key that will be used for logging the events. -->
            <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
            <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to Application Insights.   -->
            <Item Key="DeveloperMode">false</Item>
            <!-- A Boolean that indicates whether telemetry should be enabled or not.   -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is the name of the property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
            <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
            <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
            <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Passo 4. Adicionar os perfis técnicos para o Application Insights como orchestration os passos num journey de utilizador existente

Chamar `JournyeContextForInsights` como passo orchestration 1:

```xml
<!-- Initialize a session with Application Insights. -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Chamar `Azure-Insights-SignInRequest` como orchestration passo 2 para controlar o que foi recebido um pedido sessão-na/sessão-up:

```xml
<!-- Track that we have received a sign-in request. -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Imediatamente *antes* o `SendClaims` orchestration passo, adicione um novo passo que chame `Azure-Insights-UserSignup`. Que é acionado quando o utilizador seleciona o botão inscrição um journey sessão-up/início de sessão.

```xml
        <!-- Handles the user selecting the sign-up link in the local account sign-in page. -->
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

Imediatamente após o `SendClaims` passo de orquestração, chamada `Azure-Insights-SignInComplete`. Este passo reflete um journey concluída com êxito.

```xml
        <!-- Track that we have successfully sent a token. -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Renumber, depois de adicionar as novo etapas da orquestração, os passos sequencialmente sem ignorar a qualquer números inteiros de 1 para N.


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Passo 5. Carregar o ficheiro modificado extensões, execute a política e ver eventos no Application Insights

Guardar e carregar o novo ficheiro de extensão de estrutura de confiança. Em seguida, chame a política da entidade confiadora da aplicação ou utilize **executar agora** na interface do Azure AD B2C. Em segundos, os eventos estão disponíveis no Application Insights.

1. Abra o recurso do Application Insights no seu inquilino do Azure Active Directory.
2. Selecione **eventos** no **utilização** o submenu.
3. Definir **durante** para **última hora** e **por** para **3 minutos**. Poderá ter de selecionar **atualizar** para ver os resultados.

![Gráfico de eventos de utilização do Application Insights](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





## <a name="next-steps"></a>Passos Seguintes

Adicione tipos de afirmação e eventos da sua viagem de utilizador para se ajustarem às suas necessidades. Eis uma lista de possíveis afirmações, utilizando as resoluções de afirmações adicionais.

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

### <a name="openid-connect-specific-claims"></a>Afirmações de OpenID Connect específico

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

### <a name="non-protocol-parameters-included-with-oidc-and-oauth2-requests"></a>Os parâmetros de protocolo não incluído com OIDC e OAuth2 pedidos

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Qualquer nome de parâmetro incluída como parte de um pedido OIDC ou OAuth2 pode ser mapeada para uma afirmação na journey do utilizador. Em seguida, pode registar no evento. Por exemplo, o pedido da aplicação pode incluir um parâmetro de cadeia de consulta com um nome de `app_session`, `loyalty_number`, ou `any_string`.

Eis um exemplo de pedido da aplicação:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Em seguida, pode adicionar as afirmações adicionando um `InputClaim` elemento para que o evento do Application Insights:
```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="app_session" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="loyalty_number" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Outras afirmações de sistema

Alguns afirmações sistema tem de ser adicionadas para a matriz de afirmações antes de que estão disponível para gravar como eventos. O perfil técnico `SimpleUJContext` tem de ser chamado como um passo de orquestração ou um perfil de técnicas de validação antes destes afirmações estão disponíveis.

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



