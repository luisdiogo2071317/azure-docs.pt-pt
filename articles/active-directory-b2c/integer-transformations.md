---
title: Exemplos de transformação de afirmações de número inteiro para a identidade experiência de estrutura de esquema do Azure Active Directory B2C | Documentos da Microsoft
description: Número inteiro de exemplos de transformação de afirmações para a identidade experiência de estrutura de esquema do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 988e25b6a5ef3f99ae7df9076a40e06b403bb029
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381519"
---
# <a name="integer-claims-transformations"></a>Transformações de afirmações de número inteiro

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo fornece exemplos para usar as transformações de afirmações de número inteiro do esquema de arquitetura de experiências de identidade no Azure Active Directory (Azure AD) B2C. Para obter mais informações, consulte [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Converte um tipo de dados longos num tipo de dados de cadeia de caracteres.

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | longitude | ClaimType para converter uma cadeia de caracteres. |
| outputClaim | outputClaim | cadeia | ClaimType produzido este ClaimsTransformation po vyvolání. |

Neste exemplo, o `numericUserId` afirmação com um tipo de valor de longa duração é convertida numa `UserId` a afirmação com um tipo de valor de cadeia de caracteres.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **inputClaim**: 12334 (longa)
- Afirmações de saída: 
    - **outputClaim**: "12334" (cadeia)

