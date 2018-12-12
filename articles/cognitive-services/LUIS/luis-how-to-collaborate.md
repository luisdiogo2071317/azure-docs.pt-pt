---
title: Colaborar com outras pessoas
titleSuffix: Language Understanding - Azure Cognitive Services
description: Um proprietário da aplicação pode adicionar os funcionários para a aplicação. Estes colaboradores podem modificar o modelo, formar e publicar a aplicação.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: d1db8974ce134b50340db500c9ea1b00126fe10a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086424"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Como gerir os autores e colaboradores 

Um proprietário da aplicação pode adicionar os funcionários para a aplicação. Estes colaboradores podem modificar o modelo, formar e publicar a aplicação. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Adicionar acesso do funcionário

Uma aplicação tem um autor único, o proprietário, mas pode ter muitos funcionários. Para permitir que os funcionários para editar a sua aplicação LUIS, tem de adicionar o e-mail que utilizam para aceder ao portal de LUIS para a lista de colaboradores. Depois que serem adicionados, a aplicação é apresentada no seu portal de LUIS.

1. Selecione **Manage** a partir do menu superior direito, em seguida, selecione **colaboradores** no menu à esquerda.

2. Selecione **adicionar acesso do funcionário** da barra de ferramentas.

    [![Adicionar acesso do funcionário](./media/luis-how-to-collaborate/add-collaborator.png "adicionar acesso do funcionário")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Introduza o endereço de e-mail, que o acesso do funcionário utiliza para iniciar sessão portal do LUIS.

    ![Adicionar endereço de e-mail do colaborador](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Transferência de propriedade

Enquanto o LUIS atualmente não suporta a transferência de propriedade, pode exportar a sua aplicação e outro utilizador de LUIS pode importar a aplicação. Pode haver pequenas diferenças na pontuações de LUIS entre as duas aplicações. 

## <a name="azure-active-directory-resources"></a>Recursos do Azure Active Directory

Se utilizar o Azure Active Directory (Azure AD) na sua organização, LUIS necessita da permissão para aceder às informações, sobre os utilizadores quando pretendem utilizar o LUIS. Os recursos de que necessita de LUIS são mínimos. 

Veja a descrição detalhada quando tentar inscrever-se com uma conta que tem o consentimento de administrador ou não necessita de consentimento de administrador, como o consentimento de administrador:

* Permite-lhe iniciar sessão na aplicação com a sua conta organizacional e permitir que a aplicação leia o seu perfil. Também permite que a aplicação Leia informações básicas da empresa.
* Permite que a aplicação ver e atualizar os seus dados, mesmo quando não está a utilizar a aplicação.

A primeira permissão dá permissão de LUIS para ler dados de perfil básicas, como ID de utilizador, e-mail, nome. A segunda permissão é necessária para atualizar o token de acesso do utilizador.

## <a name="azure-active-directory-tenant-user"></a>Utilizador de inquilino do Azure Active Directory

LUIS usa o fluxo de consentimento do padrão do Azure Active Directory (Azure AD). 

O administrador de inquilinos deve trabalhar diretamente com o utilizador que precisa de acesso concedido a utilizar o LUIS no Azure AD. 

Em primeiro lugar, o utilizador inicia sessão LUIS e vê a caixa de diálogo pop-up que precisam de aprovação de administrador. O usuário entra em contacto com o administrador de inquilino antes de continuar. 

Em segundo lugar, o administrador de inquilino iniciar sessão numa LUIS e vê um diálogo de pop-up de fluxo de consentimento. Esta é a caixa de diálogo que o administrador tem de conceder permissão para o utilizador. Assim que o administrador de aceita a permissão, o utilizador é continuar com os LUIS.

Se o administrador de inquilino não irá iniciar sessão no LUIS, o administrador pode aceder [consentimento](https://account.activedirectory.windowsazure.com/r#/applications) para LUIS. 

![Permissão do Azure active directory ao Web site da aplicação](./media/luis-how-to-collaborate/tenant-permissions.png)

Se o administrador de inquilinos apenas desejar determinados utilizadores para utilizar o LUIS, consulte este [blog sobre identidades](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Contas de utilizador com várias mensagens de e-mail para funcionários

Se adicionar colaboradores para uma aplicação do LUIS, está a especificar o endereço de e-mail exatamente que um funcionário tem de utilizar o LUIS como um colaborador. Enquanto o Azure Active Directory (Azure AD) permite um único utilizador ter mais de uma conta de e-mail utilizada alternadamente, LUIS exige que o utilizador iniciar sessão com o endereço de e-mail especificado na lista do funcionário.

