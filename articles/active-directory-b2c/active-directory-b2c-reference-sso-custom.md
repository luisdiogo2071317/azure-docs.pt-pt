---
title: Gerenciamento de sessões SSO com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como gerir sessões SSO com as políticas personalizadas no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/20/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 351b48f2e2766b4974a5a41b5e95acfbd63dbfc9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443227"
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>O Azure AD B2C: Único início de sessão (SSO) gerenciamento de sessões

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O Azure AD B2C permite aos administradores controlar como o Azure AD B2C interage com um utilizador depois do utilizador já foi autenticado. Isso é feito por meio do gerenciamento de sessões SSO. Por exemplo, o administrador pode controlar se a seleção de fornecedores de identidade é apresentada, ou se os detalhes da conta local precisam de ser introduzido novamente. Este artigo descreve como configurar as definições de SSO para o Azure AD B2C.

## <a name="overview"></a>Descrição geral

Gerenciamento de sessões SSO tem duas partes. A primeira lida com as interações do usuário diretamente com o Azure AD B2C e os outros negócios com as interações do usuário com terceiros, como o Facebook. O Azure AD B2C não substituir ou ignorar sessões SSO que podem ser mantidas por partes externas. Em vez disso, a rota através do Azure AD B2C para ir para a parte externa é "lembrada", evitando a necessidade de reprompt ao utilizador selecionar o respetivo fornecedor de identidade social ou empresarial. A decisão de SSO ultimate permanece com a parte externa.

## <a name="how-does-it-work"></a>Como funciona?

Gerenciamento de sessões SSO utiliza a mesma semântica como qualquer outro perfil técnico em políticas personalizadas. Quando um passo de orquestração é executado, o perfil técnico associado com a etapa é consultado para um `UseTechnicalProfileForSessionManagement` referência. Se existir, o provedor de sessão SSO referenciado, em seguida, é verificado para ver se o utilizador é um participante de sessão. Se o fornecedor de SSO de sessão com a por isso, é utilizado para repreencher a sessão. Da mesma forma, quando a execução de um passo de orquestração estiver concluída, o fornecedor é utilizado para armazenar as informações da sessão se foi especificado um fornecedor de sessão SSO.

O Azure AD B2C tem definido um número de fornecedores de sessões SSO, que pode ser utilizado:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

Classes de gestão de SSO são especificados usando o `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` elemento de um perfil técnico.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Como o nome dita, este fornecedor não faz nada. Este fornecedor pode ser utilizado para suprimir o comportamento SSO para um perfil técnico específico.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Este fornecedor pode ser utilizado para armazenar afirmações numa sessão. Este fornecedor, normalmente, é referenciado num perfil técnico para gestão de contas locais. 

> [!NOTE]
> Ao usar o DefaultSSOSessionProvider para armazenar afirmações numa sessão, precisa garantir que quaisquer afirmações que precisam ser retornado ao aplicativo ou utilizados pela pré-condições nos passos subsequentes, são armazenados na sessão ou aumentado mediante uma leitura do perfil de utilizadores em diretório. Isto irá garantir que sua jornada de autenticação não falhará em declarações em falta.

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

Para adicionar afirmações na sessão, utilize o `<PersistedClaims>` elemento do perfil técnico. Quando o fornecedor é utilizado repreencher a sessão, o persistente afirmações são adicionadas a matriz de afirmações. `<OutputClaims>` é utilizado para recuperação das afirmações a partir da sessão.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Esse provedor é utilizado para suprimir o ecrã "Escolher o fornecedor de identidade". Normalmente, é referenciada num perfil técnico configurado para um fornecedor de identidade externo, como o Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Este fornecedor é utilizado para gerir as sessões do Azure AD B2C SAML entre aplicativos, bem como fornecedores de identidade SAML externos.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Existem dois itens de metadados para o perfil técnico:

| Item | Valor Predefinido | Valores Possíveis | Descrição
| --- | --- | --- | --- |
| IncludeSessionIndex | true | Verdadeiro/Falso | Indica ao Provedor que o índice de sessão deve ser armazenado. |
| RegisterServiceProviders | true | Verdadeiro/Falso | Indica se o fornecedor deverá registar todos os fornecedores de serviços SAML emitidos uma asserção. |

Ao utilizar o fornecedor para armazenar uma sessão de fornecedor de identidade SAML, os itens acima devem ambos ser falsos. Ao utilizar o fornecedor para armazenar a sessão de SAML do B2C, os itens acima devem ser true ou omitido que os padrões sejam verdadeiros.

>[!NOTE]
> Fim de sessão de sessão SAML requer o `SessionIndex` e `NameID` para concluir.

## <a name="next-steps"></a>Passos Seguintes

Adoramos comentários e sugestões! Se tiver quaisquer problemas com este tópico, publique no Stack Overflow usando a marca ["azure-ad-b2c"](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Para pedidos de funcionalidades, votar na nossa [fórum de comentários](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

