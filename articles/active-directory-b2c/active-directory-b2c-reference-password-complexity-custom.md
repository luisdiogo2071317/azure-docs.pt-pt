---
title: Complexidade de palavra-passe em políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Como configurar requisitos de complexidade de palavras-passe na política personalizada.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c6b8312a08d1d92bccf70e7d3dda5f01811b4f87
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848532"
---
# <a name="configure-password-complexity-in-custom-policies"></a>Configurar a complexidade de palavra-passe em políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo é uma descrição avançada de como funciona a complexidade de palavra-passe e está ativado com as políticas personalizadas do Azure AD B2C.

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>O Azure AD B2C: Configurar os requisitos de complexidade de palavras-passe

O Azure Active Directory B2C (Azure AD B2C) oferece suporte a alterar os requisitos de complexidade de palavras-passe fornecidas por um usuário final quando criar uma conta.  Por predefinição, o Azure AD B2C utiliza **forte** palavras-passe.  O Azure AD B2C também oferece suporte a opções de configuração para controlar a complexidade de palavras-passe que os clientes podem utilizar.  Este artigo fala sobre como configurar a complexidade de palavra-passe no políticas personalizadas.  Também é possível usar [configurar a complexidade de palavra-passe em políticas incorporadas](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Pré-requisitos

Um inquilino de B2C do Azure AD configurado para concluir uma conta local sessão-inscrição/início de sessão, conforme descrito em [introdução ao](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-complexity-in-custom-policy"></a>Como configurar a complexidade de palavra-passe numa política personalizada

Para configurar a complexidade da palavra-passe numa política personalizada, a estrutura geral da política personalizada tem de incluir um `ClaimsSchema`, `Predicates`, e `InputValidations` elemento dentro de `BuildingBlocks`.

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

A finalidade desses elementos é o seguinte:

- Cada `Predicate` elemento define uma verificação de validação da cadeia de caracteres básico que retorna VERDADEIRO ou FALSO.
- O `InputValidations` elemento tem um ou mais `InputValidation` elementos.  Cada `InputValidation` é construída utilizando uma série de `Predicate` elementos. Esse elemento permite-lhe efetuar agregações booleanas (semelhante à `and` e `or`).
- O `ClaimsSchema` define que afirmações está sendo validada.  Em seguida, ele define quais `InputValidation` regra é utilizada para validar essa afirmação.

### <a name="defining-a-predicate-element"></a>Definir um elemento de predicado

Os predicados de tem dois tipos de método: IsLengthRange ou MatchesRegex. Vamos examinar um exemplo de cada.  Em primeiro lugar, temos um exemplo de MatchesRegex, que é utilizado para corresponder a uma expressão regular.  Neste exemplo, corresponde a cadeia de caracteres que contém números.

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

Próxima Vamos examinar um exemplo de IsLengthRange.  Esse método obtém um comprimento de cadeia de caracteres mínimo e máximo.

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

Utilize o `HelpText` atributo para fornecer uma mensagem de erro para os utilizadores finais se falhar a verificação.  Essa cadeia de caracteres pode ser localizada com o [funcionalidade de personalização de idioma](active-directory-b2c-reference-language-customization.md).

### <a name="defining-an-inputvalidation-element"></a>Definir um elemento de InputValidation

Uma `InputValidation` é uma agregação de `PredicateReferences`. Cada `PredicateReferences` tem de ser verdadeira para que o `InputValidation` tenha êxito.  No entanto, dentro do `PredicateReferences` uso de elemento chamado de um atributo `MatchAtLeast` para especificar quantos `PredicateReference` verificações tem de devolver verdadeiras.  Opcionalmente, define um `HelpText` atributo para substituir a mensagem de erro definido no `Predicate` elementos por ele referenciados.

```XML
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
```

### <a name="defining-a-claimsschema-element"></a>Definir um elemento de ClaimsSchema

Os tipos de afirmação `newPassword` e `reenterPassword` são considerados especial, pelo que não altere os nomes.  A interface do Usuário valida o usuário reentered corretamente a palavra-passe durante a criação da conta com base nesses `ClaimType` elementos.  Para localizar o mesmo `ClaimType` elementos, procure no TrustFrameworkBase.xml em seu pacote de iniciante.  O que há de novo neste exemplo é a substituição destes elementos para definir um `InputValidationReference`. O `ID` atributo deste elemento novo está a apontar para o `InputValidation` elemento que definimos.

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a>Juntar tudo

Este exemplo mostra como todas as peças se encaixam para formar uma política de trabalho.  Para utilizar este exemplo:

1. Siga as instruções em pré-requisitos [introdução ao](active-directory-b2c-get-started-custom.md) para transferir, configurar e carregar TrustFrameworkBase.xml e TrustFrameworkExtensions.xml
1. Crie um ficheiro de SignUporSignIn.xml com o conteúdo de exemplo nesta secção.
1. Atualizar SignUporSignIn.xml substituindo `yourtenant` com o nome do seu inquilino do Azure AD B2C.
1. Carregue o ficheiro de política de SignUporSignIn.xml pela última vez.

Este exemplo contém uma validação de palavras-passe de pin e outro para as palavras-passe fortes:

- Procure `PINpassword`. Isso `InputValidation` elemento valida um pin de qualquer comprimento.  Não é utilizado no momento, porque não é referenciado no `InputValidationReference` elemento dentro de `ClaimType`. 
- Procure `PasswordValidation`. Isso `InputValidation` elemento valida uma palavra-passe é 8 a 16 carateres e contém 3 de 4 de números, letras maiúsculas, minúsculos ou símbolos.  Esta é referenciada em `ClaimType`.  Por conseguinte, esta regra está a ser imposta nesta política.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="yourtenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin">
 <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">[a-z]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">[A-Z]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">[0-9]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">[!@#$%^*()]+</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
      <InputValidation Id="PINpassword">
        <PredicateReferences Id="PINGroup">
          <PredicateReference Id="PIN" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
  </BuildingBlocks>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```
