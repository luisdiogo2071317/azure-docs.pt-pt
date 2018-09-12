---
title: Exemplos de transformação de afirmações geral para a identidade experiência de estrutura de esquema do Azure Active Directory B2C | Documentos da Microsoft
description: Exemplos de transformação de afirmações geral para a identidade experiência de estrutura de esquema do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a13cb0360a33c301129f2975ce67580204602d9a
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381487"
---
# <a name="general-claims-transformations"></a>Transformações de afirmações geral

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo fornece exemplos para a utilização de transformações de afirmações geral do esquema de arquitetura de experiências de identidade no Azure Active Directory (Azure AD) B2C. Para obter mais informações, consulte [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Verifica se o **inputClaim** existe ou não e define **outputClaim** como verdadeira ou falsa em conformidade.

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | InputClaim |Qualquer | A afirmação de entrada cuja existência tem de ser verificado. |
| outputClaim | outputClaim | boolean | ClaimType produzido este ClaimsTransformation po vyvolání. |

Utilize que este afirmações de transformação para verificar se uma afirmação existe ou contém qualquer valor. O valor de retorno é um valor booleano que indica se a afirmação existe. Exemplo a seguir verifica se existe o endereço de e-mail.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **inputClaim**: someone@contoso.com
- Afirmações de saída: 
    - **outputClaim**: VERDADEIRO

## <a name="hash"></a>Hash

O texto sem formatação fornecido com o salt e um segredo de hash.

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | texto sem formatação | cadeia | A afirmação de entrada sejam encriptados |
| InputClaim | Salt | cadeia | O parâmetro de salt. Pode criar um aleatório de valor, usando `CreateRandomString` transformação de afirmações. |
| InputParameter | randomizerSecret | cadeia | Aponta para um existente do Azure AD B2C **chaves de política**. Para criar um novo: no seu inquilino do Azure AD B2C, selecione **definições do B2C > Framework de experiência de identidade**. Selecione **chaves de política** para exibir as chaves que estão disponíveis no seu inquilino. Selecione **Adicionar**. Para **opções**, selecione **Manual**. Forneça um nome (prefixo B2C_1A_ podem ser adicionados automaticamente.). Na caixa de segredo, introduza qualquer segredo que pretende utilizar, como 1234567890. Para utilização de chave, selecione **segredo**. Selecione **Criar**. |
| outputClaim | outputClaim | boolean | Po vyvolání o ClaimType produzido depois de esta transformação de afirmações. A afirmação configurada no `plaintext` inputClaim. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **texto sem formatação**: MyPass@word1
    - **Salt**: 487624568
    - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Afirmações de saída: 
    - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U =



