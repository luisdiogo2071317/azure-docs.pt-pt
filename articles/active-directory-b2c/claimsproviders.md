---
title: ClaimsProviders - Azure Active Directory B2C | Documentos da Microsoft
description: Especifique o elemento de ClaimsProvider de uma política personalizada no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a9159ade6e16c1d14149197e85cee8720dd98b09
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381491"
---
# <a name="claimsproviders"></a>ClaimsProviders 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Um fornecedor de afirmações contém um conjunto de [perfis técnicos](technicalprofiles.md). Cada fornecedor de afirmações tem de ter um ou mais perfis técnicos que determinam os pontos finais e os protocolos necessários para comunicar com o fornecedor de afirmações. Um fornecedor de afirmações pode ter vários perfis técnicos. Por exemplo, vários perfis técnicos poderão ser definidos porque o fornecedor de afirmações suporta vários protocolos, vários pontos de extremidade com diferentes capacidades ou libera afirmações diferentes em níveis diferentes de garantia. Pode ser aceitável para libertar afirmações confidenciais no percurso do utilizador de um, mas não em outro.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProvider>
  ...
</ClaimsProviders>
```

O **ClaimsProviders** elemento contém o seguinte elemento:

| Elemento | Ocorrências | Descrição |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1: n | Um fornecedor de afirmações acreditada que pode ser otimizado em várias jornadas de utilizador. |

## <a name="claimsprovider"></a>ClaimsProvider

O **ClaimsProvider** elemento contém elementos subordinados:

| Elemento | Ocorrências | Descrição |
| ------- | ---------- | ----------- |
| Domain | 0:1 | Uma cadeia de caracteres que contém o nome de domínio para o fornecedor de afirmações. Por exemplo, se o seu fornecedor de afirmações inclui o perfil técnico do Facebook, o nome de domínio é Facebook.com. Este nome de domínio é utilizado para todos os perfis técnicos definidos no fornecedor de afirmações, a menos que substituído pelo perfil de técnico. |
| Nome a Apresentar | 0:1 | Uma cadeia que contém o nome do fornecedor de afirmações que pode ser apresentado aos utilizadores. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Um conjunto de perfis técnicos suportado pelo fornecedor de afirmação |

**ClaimsProvider** organiza seu técnico perfis relacionados com o fornecedor de afirmações. O exemplo seguinte mostra o fornecedor de afirmações do Azure Active Directory com os perfis de técnicos do Azure Active Directory: 

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...    
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

O exemplo seguinte mostra o fornecedor de afirmações do Facebook com o **Facebook OAUTH** perfil técnico.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
 
