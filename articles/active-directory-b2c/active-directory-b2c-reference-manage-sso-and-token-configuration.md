---
title: Gerir o SSO e personalização de token com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre a gestão de SSO e personalização de token com as políticas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 811fb8b2de59c9d324ab4acb8b0f51b4cec80aee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441802"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>O Azure Active Directory B2C: Gerir o SSO e personalização de token com as políticas personalizadas
Com as políticas personalizadas fornece o mesmo controle sobre seu token, sessão e únicas configurações de início de sessão (SSO) como, por meio de políticas incorporadas.  Para saber o que cada definição, consulte a documentação [aqui](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuração de afirmações e durações de token
Para alterar as definições no seu durações de token, terá de adicionar um `<ClaimsProviders>` elemento no arquivo de terceiros entidade confiadora da política que pretende afetar.  O `<ClaimsProviders>` elemento é um elemento subordinado a `<TrustFrameworkPolicy>`.  Dentro dela, precisará de colocar as informações que afeta as durações de token.  O XML é semelhante a este exemplo:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Durações de token de acesso** -a duração do token de acesso pode ser alterada ao modificar o valor entre o `<Item>` com a chave = "token_lifetime_secs" em segundos.  O valor predefinido na incorporado é de 3600 segundos (60 minutos).

**Duração do token de ID** -duração do token o ID pode ser alterado ao modificar o valor entre o `<Item>` com a chave = "id_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é de 3600 segundos (60 minutos).

**Duração do token atualizado** -a duração do token de atualização pode ser alterada ao modificar o valor entre o `<Item>` com a chave = "refresh_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 1209600 segundos (14 dias).

**Atualizar token duração da janela deslizante** - se de que pretende definir uma duração de janela deslizante para seu token de atualização, modifique o valor entre `<Item>` com a chave = "rolling_refresh_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 7776000 (90 dias).  Se não pretender impor uma duração de janela deslizante, substitua essa linha com:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Afirmação do emissor (iss)** - se de que pretende alterar a afirmação do emissor (iss), modifique o valor entre o `<Item>` com a chave = "IssuanceClaimPattern".  Os valores aplicáveis são `AuthorityAndTenantGuid` e `AuthorityWithTfp`.

**Definição de afirmação que representa a ID de política** -as opções para a definição deste valor são TFP (política de estrutura de confiança) e ACR (referência de contexto de autenticação).  
É recomendável definir este tipo como TFP, para fazer isso, certifique-se a `<Item>` com a chave = "AuthenticationContextReferenceClaimPattern" existe e o valor é `None`.
No seu `<OutputClaims>` item, adicione este elemento:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Para o ACR, remova o `<Item>` com a chave = "AuthenticationContextReferenceClaimPattern".

**Afirmação do requerente** -esta opção é configurações como padrão para ObjectID, se gostaria de mudar isto para `Not Supported`, efetue o seguinte procedimento:

Substitua essa linha 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
com esta linha:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Comportamento da sessão e SSO

Para alterar seu comportamento de sessão e configurações de SSO, tem de adicionar uma `<UserJourneyBehaviors>` elemento dentro do `<RelyingParty>` elemento.  O `<UserJourneyBehaviors>` elemento tem de seguir imediatamente o `<DefaultUserJourney>`.  O interior da sua `<UserJourneyBehavors>` elemento deve ter o seguinte aspeto:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Na configuração (SSO) início de sessão único** - para alterar a configuração início de sessão única, precisa modificar o valor de `<SingleSignOn>`.  Os valores aplicáveis são `Tenant`, `Application`, `Policy` e `Disabled`. 

**Aplicação Web duração da sessão (minutos)** - para alterar a aplicação web a duração da sessão, precisa modificar o valor da `<SessionExpiryInSeconds>` elemento.  O valor predefinido na políticas incorporadas é 86400 segundos (1440 minutos).

**Tempo limite de sessão de aplicação Web** - para alterar o limite de sessão de aplicação web, precisa modificar o valor de `<SessionExpiryType>`.  Os valores aplicáveis são `Absolute` e `Rolling`.
