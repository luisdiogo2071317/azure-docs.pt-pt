---
title: Colaborar com outros contribuidores no LUIS aplicações no Azure | Documentos da Microsoft
description: Saiba como colabore com outros contribuintes em aplicativos de compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/31/2018
ms.author: diberry
ms.openlocfilehash: 99f37cb6dc5e05fc5eb4bde09685435ee57fecc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397793"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Como gerir os autores e colaboradores 

Pode colaborar com outras pessoas na sua aplicação LUIS em conjunto. 

## <a name="owner-and-collaborators"></a>Proprietário e os funcionários

Uma aplicação tem um autor único, o proprietário, mas pode ter muitos funcionários. 

## <a name="add-collaborator"></a>Adicionar acesso do funcionário

Para permitir que os funcionários para editar a sua aplicação do LUIS, sobre o **definições** página da sua aplicação LUIS, introduza o e-mail do colaborador e clique em **colaborador de adicionar**. Colaboradores podem iniciar sessão e editar a sua aplicação do LUIS, ao mesmo tempo em que estiver a trabalhar na aplicação.

![Adicionar acesso do funcionário](./media/luis-how-to-collaborate/add-collaborator.png)

## <a name="transfer-of-ownership"></a>Transferência de propriedade

Enquanto o LUIS atualmente não suporta a transferência de propriedade, pode exportar a sua aplicação e outro utilizador de LUIS pode importar a aplicação. Pode haver pequenas diferenças na pontuações de LUIS entre as duas aplicações. 

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