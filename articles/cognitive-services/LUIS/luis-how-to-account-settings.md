---
title: Gerir definições da sua conta em LUIS | Microsoft Docs
description: Utilize o site de LUIS para gerir definições da sua conta.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 1f22112a38bf32af03ffaf0493db16839b3fe794
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749968"
---
# <a name="manage-your-luis-account"></a>Gerir a sua conta LUIS
Duas peças-chave de informações para uma conta de LUIS são a conta de utilizador e a chave de criação. As informações de início de sessão são geridas no [account.microsoft.com](https://account.microsoft.com). É gerida a partir da sua chave de criação de [LUIS][LUIS] Web site **definições** página. 

## <a name="authoring-key"></a>Chave de criação

Este único, a região específicos criação chave, no **definições** página, permite-lhe criar todas as suas aplicações do [LUIS][LUIS] Web site, bem como o [ APIs de criação](https://aka.ms/luis-authoring-api). Para efeitos práticos, a chave de criação é permitida para tornar um [limitado](luis-boundaries.md) consulta o número de ponto final de cada mês. 

![Página de definições de LUIS](./media/luis-how-to-account-settings/account-settings.png)

A chave de criação é utilizada para todas as aplicações que possui, bem como todas as aplicações que está listados como um funcionário.

## <a name="authoring-key-regions"></a>Regiões de chaves de criação
A chave de criação é específica para o [criação região](luis-reference-regions.md#publishing-regions). A chave não funciona numa região diferente. 

## <a name="reset-authoring-key"></a>Repor a chave de criação
Se a chave de criação for comprometida, reponha a chave. A chave é reposta em todas as suas aplicações no [LUIS] Web site. Se criar as suas aplicações através de APIs de criação, terá de alterar o valor de `Ocp-Apim-Subscription-Key` para a nova chave. 

## <a name="delete-account"></a>Eliminar conta
Consulte [armazenamento de dados e remoção](luis-concept-data-storage.md#accounts) para obter informações sobre que dados são eliminados quando eliminar a conta. 

## <a name="azure-active-directory-tenant-user"></a>Utilizador de inquilino do Azure Active Directory
LUIS utiliza o fluxo de consentimento do padrão do Azure Active Directory (Azure AD). 

O administrador de inquilino deve trabalhar diretamente com o utilizador que precisa de acesso concedido a utilizar LUIS no Azure AD. 

Em primeiro lugar, o utilizador inicia sessão LUIS e vê a caixa de diálogo de pop-up que necessita de aprovação do administrador. O utilizador entra em contacto com o administrador inquilino antes de continuar. 

Segundo, o administrador de inquilino inicia sessão LUIS e vê um diálogo de pop-up de fluxo de consentimento. Esta é a caixa de diálogo que o administrador tem de conceder permissão para o utilizador. Depois do administrador aceita a permissão, o utilizador é capaz de continuar com LUIS.

Se o administrador inquilino não irá iniciar sessão no LUIS, o administrador pode aceder ao mesmo [consentimento](https://account.activedirectory.windowsazure.com/r#/applications) para LUIS. 

![Permissão do Azure Active Directory por Web site da aplicação](./media/luis-how-to-account-settings/tenant-permissions.png)

Se o administrador inquilino quiser apenas determinados utilizadores para utilizar LUIS, consulte este [blogue de identidade](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Contas de utilizador com várias mensagens de correio eletrónico para os colaboradores
Se adicionar colaboradores para uma aplicação LUIS, especifica o endereço de e-mail exato que um funcionário tem de utilizar LUIS como um funcionário. Enquanto o Azure Active Directory (Azure AD) permite um único utilizador ter mais do que uma conta de e-mail-no alternadamente, LUIS exige que o utilizador iniciar sessão com o endereço de e-mail especificado na lista do colaborador. 


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [criação chave](luis-concept-keys.md#authoring-key). 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
