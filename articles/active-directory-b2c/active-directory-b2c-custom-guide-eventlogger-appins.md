---
title: Controlar o comportamento do utilizador com eventos no Application Insights do Azure Active Directory B2C | Documentos da Microsoft
description: Guia passo a passo para ativar registos de eventos no Application Insights do Azure AD B2C jornadas de utilizador ao utilizar políticas personalizadas (pré-visualização)
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/16/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c77feed3b86358c74f741b53aa03ecb454dc9a62
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337107"
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Controlar o comportamento do utilizador no Azure AD B2C Jornadas utilizando o Application Insights

O Azure Active Directory B2C (Azure AD B2C) funciona bem com o Azure Application Insights. Fornecem registos de eventos detalhados e personalizados para seu jornadas de utilizador personalizada criada. Este artigo mostra como começar a utilizar para que possa:

* Obtenha informações sobre o comportamento do utilizador.
* Resolver problemas relacionados com as suas políticas no desenvolvimento ou em produção.
* Medir o desempenho.
* Crie notificações do Application Insights.

> [!NOTE]
> Esta funcionalidade está em pré-visualização.

## <a name="how-it-works"></a>Como funciona

O Framework de experiência de identidade no Azure AD B2C agora inclui o provedor `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Enviar dados de eventos diretamente para o Application Insights utilizando a chave de instrumentação fornecida para o Azure AD B2C.

Um perfil técnico usa esse provedor para definir um evento do B2C.  O perfil Especifica o nome do evento, as afirmações que serão gravadas e a chave de instrumentação.  Para publicar um evento, o perfil técnico, em seguida, é adicionado como um `orchestration step` ou como um `validation technical profile` num percurso do utilizador personalizada.

O Application Insights pode unificar os eventos ao utilizar um ID de correlação para gravar uma sessão do utilizador. Application Insights disponibiliza o evento e a sessão dentro de segundos e apresenta muitos visualização, exportação e ferramentas analíticas.

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos na [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md). Este artigo pressupõe que está a utilizar o pacote de iniciante da política personalizada. Mas o pacote de iniciante não é necessário.

## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Passo 1. Criar um recurso do Application Insights e obtenha a chave de instrumentação

Quando estiver a utilizar o Application Insights com o Azure AD B2C, o único requisito é criar um recurso e obter uma chave de instrumentação. Criar um recurso no [portal do Azure.](https://portal.azure.com)

1. No portal do Azure, no seu inquilino de subscrição, selecione **+ criar um recurso**. Este inquilino não é o inquilino do Azure AD B2C.  
2. Procure e selecione **Application Insights**.  
3. Criar um recurso que utiliza **aplicação web ASP.NET** como **tipo de aplicação**, sob uma subscrição de sua preferência.
4. Depois de criar o recurso do Application Insights, abra-o e tenha em atenção a chave de instrumentação.

![Descrição geral do Application Insights e a chave de instrumentação](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Passo 2. Adicionar novas definições de ClaimType ao seu ficheiro de extensão da estrutura de confiança

Abra o ficheiro de extensão do pacote de iniciante e adicione os seguintes elementos para o `<BuildingBlocks>` nó. Normalmente, é o nome do ficheiro `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

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

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Passo 3. Adicionar novos perfis de técnicos que utilizam o fornecedor do Application Insights

Perfis técnicos podem ser considerados as funções na identidade experiência Framework do Azure AD B2C. Este exemplo define cinco perfis técnicos para abrir uma sessão e publicar eventos:

| Perfil técnico | Tarefa |
| ----------------- | -----|
| AzureInsights-Common | Cria um conjunto comum de parâmetros a serem incluídos em todos os perfis de técnicos de AzureInsights | 
| JourneyContextForInsights | Abre-se a sessão no Application Insights e envia um ID de correlação |
| AzureInsights-SignInRequest | Cria um `SignIn` eventos com um conjunto de afirmações quando recebido um pedido de início de sessão | 
| AzureInsights-UserSignup | Cria um evento de UserSignup quando o utilizador for acionada a opção Inscreva-se numa jornada de início de sessão-inscrição/início de sessão | 
| AzureInsights-SignInComplete | Regista a conclusão com êxito de uma autenticação, quando um token foi enviado para o aplicativo de parte confiável | 

Adicionar os perfis para o ficheiro de extensão do pacote de iniciante ao adicionar estes elementos para o `<ClaimsProviders>` nó.  Normalmente, é o nome do ficheiro `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Alterar a chave de instrumentação no `ApplicationInsights-Common` perfil técnico para o GUID que fornece o recurso do Application Insights.

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
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
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
        <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights. -->
        <Item Key="DeveloperMode">false</Item>
        <!-- A Boolean that indicates whether telemetry should be enabled or not. -->
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Passo 4. Adicionar os perfis técnicos para o Application Insights como orquestração os passos num percurso do utilizador existente

Chamar `JournyeContextForInsights` como passo de orquestração 1:

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Chamar `Azure-Insights-SignInRequest` como orquestração passo 2 para controlar o que foi recebido um pedido início de sessão-em/inscrever-se:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Imediatamente *antes de* a `SendClaims` orquestração passo, adicione um novo passo que chama `Azure-Insights-UserSignup`. Ele é acionado quando o usuário seleciona o botão inscrever-se numa jornada de início de sessão-inscrição/início de sessão.

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

Imediatamente após a `SendClaims` passo de orquestração, chamada `Azure-Insights-SignInComplete`. Este passo reflete uma jornada concluída com êxito.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="11" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Renumber, depois de adicionar novos passos de orquestração, os passos sequencialmente sem ignorar qualquer inteiros de 1 a N.


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Passo 5. Carregar o ficheiro de extensões modificado, executar a política e exibir os eventos no Application Insights

Guardar e carregue o novo ficheiro de extensão do framework de confiança. Em seguida, chamar a política de terceiros entidade confiadora da aplicação ou utilize `Run Now` na interface do Azure AD B2C. Em segundos, os seus eventos estão disponíveis no Application Insights.

1. Abra o **Application Insights** recursos no seu inquilino do Azure Active Directory.
2. Selecione **utilização** > **eventos**.
3. Definir **durante** ao **última hora** e **por** para **3 minutos**.  Poderá ter de selecionar **atualizar** ver os resultados.

![Application Insights-eventos de utilização Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

##  <a name="next-steps"></a>Passos Seguintes

Adicione tipos de afirmação e eventos para o seu percurso do utilizador para se ajustar às suas necessidades. Aqui está uma lista de declarações possíveis, utilizar resoluções de afirmações adicionais

### <a name="culture-specific-claims"></a>Declarações específicas de cultura

```xml
Referenced using: {Culture:One of the property names below}
```

| Afirmação | Definição | Exemplo |
| ----- | -----------| --------|
| LanguageName | As duas letras código ISO para o idioma | en |
| RegionName | As duas letras código ISO para a região | EUA |
| RFC5646 | O código de idioma RFC5646 | pt-PT |
| LCID   | O LCID de código de idioma | 22 |

### <a name="policy-specific-claims"></a>Declarações específicas de política

```xml
Referenced using {Policy:One of the property names below}
```

| Afirmação | Definição | Exemplo |
| ----- | -----------| --------|
| TrustFrameworkTenantId | O id de inquilino trustframework | N/A |
| RelyingPartyTenantId | O id de inquilino da entidade confiadora | N/A |
| PolicyId | O id de política da política | N/A |
| TenantObjectId | O id de objeto do inquilino da política | N/A |

### <a name="openid-connect-specific-claims"></a>Declarações específicas do OpenID Connect

```xml
Referenced using {OIDC:One of the property names below}
```

| Afirmação | Parâmetro de OpenIdConnect | Exemplo |
| ----- | ----------------------- | --------|
| linha de comandos | linha de comandos | N/A |
| LoginHint |  login_hint | N/A |
| DomainHint | domain_hint | N/A |
|  : MaxAge | max_age | N/A |
| ID de cliente | client_id | N/A |
| Nome de utilizador | login_hint | N/A |
|  Recurso | Recurso| N/A |
| AuthenticationContextReferences | acr_values | N/A |

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>Parâmetros de protocolo não incluídos com pedidos OIDC & OAuth2

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Qualquer nome de parâmetro incluída como parte de um pedido de OIDC ou OAuth2 pode ser mapeada para uma declaração no percurso do utilizador. Em seguida, pode registar no evento. Por exemplo, o pedido da aplicação pode incluir um parâmetro de cadeia de caracteres de consulta com o nome `app_session`, `loyalty_number` ou `any_string`.

Este é um pedido de exemplo da aplicação:

```
https://sampletenant.b2clogin.com/tfp/sampletenant.onmicrosoft.com/B2C_1A_signup_signin/oauth2/v2.0/authorize?client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Em seguida, pode adicionar as declarações, adicionando um `Input Claim` elemento para o evento do Application Insights. Propriedades de um evento são adicionadas através da sintaxe {: nome da propriedade}, em que o nome é propriedade a ser adicionada ao evento. Por exemplo:

```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Outras afirmações de sistema

Algumas declarações de sistema tem de ser adicionadas para a matriz de afirmações antes de estarem disponíveis para registrar eventos. O perfil técnico `SimpleUJContext` tem de ser chamado como um passo de orquestração ou um perfil de técnicas de validação antes essas declarações estão disponíveis.

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
```


