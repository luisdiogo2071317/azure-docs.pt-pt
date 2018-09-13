---
title: Como alterar a duração do token é predefinido para uma aplicação personalizada desenvolvida | Documentos da Microsoft
description: Como atualizar as políticas de duração do Token para a sua aplicação que está a desenvolver no Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 059920c710b202e22a22f8431c536c5dfa19f2b9
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724200"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Como alterar as predefinições de duração do token para uma aplicação personalizada desenvolvida

O Azure AD Premium permite que os desenvolvedores de aplicativos e administradores de inquilinos para configurar a duração de tokens emitidos para clientes não-confidenciais. Políticas de duração do token são definidas numa base de ao nível do inquilino ou os recursos que está sendo acessados.

 * Para definir uma política de duração do token, tem de transferir os [módulo do Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).

 * Executar o **Connect-AzureAD-confirmar** comando.

 * Eis um exemplo de política que define o token de atualização de único fator a idade máxima. Crie a política: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Check-Out do [duração do token configurar](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) documento para aprender a criar outro personalizado.

## <a name="next-steps"></a>Passos Seguintes
[Configurar a duração do Token](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Referência de tokens do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

