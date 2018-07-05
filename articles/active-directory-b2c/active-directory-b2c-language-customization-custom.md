---
title: Personalização de idioma em políticas personalizadas do Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como utilizar localizar o conteúdo em políticas personalizadas para vários idiomas.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6269ac65e5db20521346d5312bcbadd0905c36e2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440569"
---
# <a name="language-customization-in-custom-policies"></a>Personalização de idioma em políticas personalizadas

> [!NOTE]
> Esta funcionalidade está em pré-visualização pública.
> 

Em políticas personalizadas, a personalização de idioma funciona tal como em políticas incorporadas.  Consulte o incorporado [documentação](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) que descreve o comportamento no como uma linguagem é escolhida com base nos parâmetros e definições do browser.

## <a name="enable-supported-languages"></a>Ativar suportada de idiomas
Se localidades da interface do usuário não foi especificado e o navegador do usuário solicita uma dessas linguagens, os idiomas suportados são apresentados ao utilizador.  

Idiomas com suporte são definidos no `<BuildingBlocks>` no seguinte formato:

```XML
<BuildingBlocks>
  <Localization>
    <SupportedLanguages DefaultLanguage="en">
      <SupportedLanguage>qps-ploc</SupportedLanguage>
      <SupportedLanguage>en</SupportedLanguage>
    </SupportedLanguages>
  </Localization>
</BuildingBlocks>
```

Idioma predefinido e idiomas suportados se comportam da mesma forma como são as políticas incorporadas.

## <a name="enable-custom-language-strings"></a>Ativar as cadeias de caracteres do idioma personalizado

Criação de cadeias de caracteres do idioma personalizado requer duas etapas:
1. Editar o `<ContentDefinition>` para a página especificar um ID de recurso para os idiomas desejados
2. Criar o `<LocalizedResources>` com IDs correspondentes no seu `<BuildingBlocks>`

Lembre-se de que pode colocar um `<ContentDefinition>` e `<BuildingBlock>` no seu ficheiro de extensão ou o ficheiro de política da entidade confiadora, dependendo se pretende que as alterações para estar em todas as suas políticas inheriting ou não.

### <a name="edit-the-contentdefinition-for-the-page"></a>Editar ContentDefinition para a página

Para cada página que pretende localizar, pode especificar no `<ContentDefinition>` que recursos de linguagem para procurar por cada código de idioma.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

Neste exemplo, francês (fr) e cadeias de caracteres personalizadas do inglês (en) são adicionadas para a página de inscrição ou início de sessão unificada.  O `LocalizedResourcesReferenceId` para cada `LocalizedResourcesReference` é o mesmo que a Localidade, mas pode usar qualquer cadeia de caracteres como o ID.  Para cada combinação de idioma e página, tem de criar um correspondente `<LocalizedResources>` mostrado a seguir.


### <a name="create-the-localizedresources"></a>Criar a LocalizedResources

As substituições são contidas no seu `<BuildingBlocks>` e não há uma `<LocalizedResources>` para cada página e o idioma que especificou no `<ContentDefinition>` para cada página.  Cada substituição é especificada como um `<LocalizedString>` como no exemplo a seguir:

```XML
<BuildingBlocks>
  <Localization>
    <LocalizedResources Id="en">
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimsProvider" StringId="SignInWithLogonNameExchange">Local Account Sign-in</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="DisplayName">Username</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="UserHelpText">Username used for signing in.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="PatternHelpText">The username you provided is not valid.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_signin">Sign In Now</LocalizedString>
        <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidPassword">Your password is incorrect.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
  </Localization>
</BuildingBlocks>
```

Existem quatro tipos de elementos de cadeia de caracteres na página:

**ClaimsProvider** -etiquetas para os fornecedores de identidade (Facebook, Google, do Azure AD etc.) **ClaimType** -etiquetas para os atributos e seus correspondentes texto de ajuda ou erros de validação de campo **UxElement** - outros elementos na página que estão lá por padrão, como botões, ligações ou texto decadeiasdecaracteres**ErrorMessage** -as mensagens de erro de validação de formulário

Certifique-se de que o `StringId`s corresponder para a página que está a utilizar estas substituições, caso contrário, ele está bloqueado pela validação de diretivas no carregamento.  