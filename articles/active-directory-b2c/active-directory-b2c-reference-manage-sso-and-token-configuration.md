---
title: Gerir o SSO e personalização token com as políticas personalizadas no Azure Active Directory B2C | Microsoft Docs
description: Saiba mais sobre a gestão SSO e personalização token com as políticas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 43e392979c50d340a10575898edb25b119e1410b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712237"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>O Azure Active Directory B2C: Gerir o SSO e personalização token com as políticas personalizadas
Utilizar políticas personalizadas fornece o mesmo controlo sobre o token, a sessão e o único configurações de início de sessão (SSO) como através de políticas incorporadas.  Para saber o que cada definição, consulte a documentação [aqui](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuração de afirmações e durações token
Para alterar as definições no seu token durações, terá de adicionar um `<ClaimsProviders>` elemento no ficheiro de terceiros de entidade confiadora da política que pretende afetar.  O `<ClaimsProviders>` elemento é um elemento subordinado do `<TrustFrameworkPolicy>`.  Dentro de, terá de colocar as informações que afeta as durações token.  O XML aspeto neste exemplo:

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

**Durações de token de acesso** -a duração do token de acesso pode ser alterada modificando o valor de `<Item>` com a chave = "token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 3600 segundos (60 minutos).

**Duração do token ID** -duração do token o ID pode ser alterado modificando o valor de `<Item>` com a chave = "id_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 3600 segundos (60 minutos).

**Atualize a duração do token** -a duração do token de atualização pode ser alterada modificando o valor de `<Item>` com a chave = "refresh_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 1209600 segundos (14 dias).

**Atualizar o token duração de janela deslizante** - se de que pretende definir uma duração de janela deslizante para o seu token de atualização, modifique o valor `<Item>` com a chave = "rolling_refresh_token_lifetime_secs" em segundos.  O valor predefinido na incorporado é 7776000 (90 dias).  Se não quiser impor uma duração de janela deslizante, substitua a linha com:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**O emissor (iss) afirmação** - se de que pretende alterar a afirmação de emissor (iss), modifique o valor dentro do `<Item>` com a chave = "IssuanceClaimPattern".  Os valores aplicáveis são `AuthorityAndTenantGuid` e `AuthorityWithTfp`.

**Definição que representa o ID de política de afirmações** -as opções para a definição deste valor são TFP (confiança da política de framework) e ACR (referência de contexto de autenticação).  
Recomendamos que definir este como TFP, para efetuar este procedimento, certifique-se a `<Item>` com a chave = "AuthenticationContextReferenceClaimPattern" existe e o valor é `None`.
No seu `<OutputClaims>` item, adicione este elemento:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Para o ACR, remova o `<Item>` com a chave = "AuthenticationContextReferenceClaimPattern".

**O requerente (sub) afirmação** -esta opção é predefinida para ObjectID, se pretender que esta opção para alternar `Not Supported`, efetue o seguinte procedimento:

Substitua esta linha 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
com esta linha:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Comportamento de sessão e SSO

Para alterar o comportamento de sessão e configurações de SSO, tem de adicionar um `<UserJourneyBehaviors>` elemento dentro do `<RelyingParty>` elemento.  O `<UserJourneyBehaviors>` elemento tem de seguir de imediato o `<DefaultUserJourney>`.  Dentro do seu `<UserJourneyBehavors>` elemento deve ter o seguinte aspeto:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Início de sessão na configuração de (SSO) único** - para alterar a configuração início de sessão único, terá de modificar o valor de `<SingleSignOn>`.  Os valores aplicáveis são `Tenant`, `Application`, `Policy` e `Disabled`. 

**Aplicação Web duração de sessão (minutos)** - para alterar a aplicação web duração de sessão, terá de modificar o valor da `<SessionExpiryInSeconds>` elemento.  O valor predefinido na políticas incorporadas é segundos 86400 (1440 minutos).

**Tempo limite de sessão de aplicação Web** - para alterar o web app tempo limite da sessão, terá de modificar o valor de `<SessionExpiryType>`.  Os valores aplicáveis são `Absolute` e `Rolling`.
