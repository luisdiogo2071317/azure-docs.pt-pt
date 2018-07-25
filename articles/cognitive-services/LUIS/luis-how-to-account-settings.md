---
title: Gerir as definições de conta do LUIS | Documentos da Microsoft
description: Utilize o site do LUIS para gerir as definições de conta.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 963a7f8c196702ea899ddfe31e6187a15eb5f683
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223213"
---
# <a name="manage-account-and-authoring-key"></a>Gerir conta e chave de criação
As duas informações cruciais para uma conta de LUIS são a conta de utilizador e a chave de criação. Suas informações de início de sessão são gerenciadas ao [account.microsoft.com](https://account.microsoft.com). A chave de criação é gerenciada a partir da [LUIS](luis-reference-regions.md) site **definições** página. 

## <a name="authoring-key"></a>Chave de criação

Esse único e específico da região de criação chave, no **definições** página, permite-lhe criar todas as suas aplicações a partir os [LUIS](luis-reference-regions.md) Web site, bem como o [APIs de criação](https://aka.ms/luis-authoring-api). Como uma conveniência, a chave de criação tem permissão para efetuar uma [limitado](luis-boundaries.md) consulta o número de ponto final de cada mês. 

![Página de definições de LUIS](./media/luis-how-to-account-settings/account-settings.png)

A chave de criação é utilizada para todas as aplicações que é proprietário, bem como todas as aplicações que está listado como um colaborador.

## <a name="authoring-key-regions"></a>Regiões de chave de criação
A chave de criação é específica para o [criação região](luis-reference-regions.md#publishing-regions). A chave não funciona numa região diferente. 

## <a name="reset-authoring-key"></a>Repor a chave de criação
Se a sua chave de criação for comprometido, repor a chave. A chave é reposta em todas as suas aplicações no [LUIS](luis-reference-regions.md) Web site. Se cria seus aplicativos por meio das APIs de criação, tem de alterar o valor de `Ocp-Apim-Subscription-Key` para a nova chave. 

## <a name="delete-account"></a>Eliminar conta
Ver [armazenamento de dados e remoção](luis-concept-data-storage.md#accounts) para obter informações sobre quais dados são eliminados quando eliminar a sua conta. 

## <a name="azure-active-directory-tenant-user"></a>Utilizador de inquilino do Azure Active Directory
LUIS usa o fluxo de consentimento do padrão do Azure Active Directory (Azure AD). 

O administrador de inquilinos deve trabalhar diretamente com o utilizador que precisa de acesso concedido a utilizar o LUIS no Azure AD. 

Em primeiro lugar, o utilizador inicia sessão LUIS e vê a caixa de diálogo pop-up que precisam de aprovação de administrador. O usuário entra em contacto com o administrador de inquilino antes de continuar. 

Em segundo lugar, o administrador de inquilino iniciar sessão numa LUIS e vê um diálogo de pop-up de fluxo de consentimento. Esta é a caixa de diálogo que o administrador tem de conceder permissão para o utilizador. Assim que o administrador de aceita a permissão, o utilizador é continuar com os LUIS.

Se o administrador de inquilino não irá iniciar sessão no LUIS, o administrador pode aceder [consentimento](https://account.activedirectory.windowsazure.com/r#/applications) para LUIS. 

![Permissão do Azure active directory ao Web site da aplicação](./media/luis-how-to-account-settings/tenant-permissions.png)

Se o administrador de inquilinos apenas desejar determinados utilizadores para utilizar o LUIS, consulte este [blog sobre identidades](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Contas de utilizador com várias mensagens de e-mail para funcionários
Se adicionar colaboradores para uma aplicação do LUIS, está a especificar o endereço de e-mail exatamente que um funcionário tem de utilizar o LUIS como um colaborador. Enquanto o Azure Active Directory (Azure AD) permite um único utilizador ter mais de uma conta de e-mail utilizada alternadamente, LUIS exige que o utilizador iniciar sessão com o endereço de e-mail especificado na lista do funcionário. 


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre sua [chave de criação](luis-concept-keys.md#authoring-key). 

