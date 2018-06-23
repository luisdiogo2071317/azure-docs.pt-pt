---
title: Criar um Principal de serviço para a pilha do Azure | Microsoft Docs
description: Descreve como criar um principal de serviço que pode ser utilizado com o controlo de acesso baseado em funções no Gestor de recursos do Azure para gerir o acesso aos recursos.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 3c9f114c2844021d515765888aa19f18a0adc10b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320759"
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Conceder acesso de aplicações aos recursos de pilha do Azure através da criação de principais de serviço

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode conceder um acesso de aplicação aos recursos de pilha do Azure através da criação de um serviço principal que utiliza o Azure Resource Manager. Um principal de serviço permite-lhe delegar permissões específicas utilizando [controlo de acesso baseado em funções](azure-stack-manage-permissions.md).

Como melhor prática, deve utilizar principais de serviço para as suas aplicações. Principais de serviço são preferível para executar uma aplicação utilizando as suas próprias credenciais pelos seguintes motivos:

* Pode atribuir permissões ao serviço principal que sejam diferentes a suas própria conta as permissões. Normalmente, as permissões de um principal de serviço estão limitadas a exatamente o que a aplicação tem de fazer.
* Não é necessário que alterar credenciais da aplicação se a função ou responsabilidades alterar.
* Pode utilizar um certificado para automatizar a autenticação ao executar um script automático.

## <a name="example-scenario"></a>Cenário de exemplo

Tiver uma aplicação de gestão de configuração que precisa de inventário de recursos do Azure com o Azure Resource Manager. Pode criar um serviço principal e atribua-à função de leitor. Esta função dá o acesso só de leitura da aplicação aos recursos do Azure.

## <a name="getting-started"></a>Introdução

Utilize os passos neste artigo como guia para:

* Crie um principal de serviço para a sua aplicação.
* Registar a sua aplicação e crie uma chave de autenticação.
* Atribua a aplicação a uma função.

A forma como tiver configurado o Active Directory para a pilha do Azure determina como criar um principal de serviço. Escolha uma das seguintes opções:

* Criar um serviço principal para [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Criar um serviço principal para [serviços de Federação do Active Directory (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

Os passos para atribuir um principal de serviço a uma função o mesmo para o Azure AD e AD FS. Depois de criar o principal de serviço, pode [delegar permissões](azure-stack-create-service-principals.md#assign-role-to-service-principal) atribuindo-la a uma função.

## <a name="create-a-service-principal-for-azure-ad"></a>Criar um principal de serviço para o Azure AD

Se a pilha do Azure utiliza o Azure AD como o arquivo de identidade, pode criar um serviço principal utilizando os mesmos passos como no Azure, utilizando o portal do Azure.

>[!NOTE]
Verifique se tem o [necessárias permissões do Azure AD](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) antes de começar a criar um principal de serviço.

### <a name="create-service-principal"></a>Criar um principal de serviço

Para criar um principal de serviço para a sua aplicação:

1. Inicie sessão na sua conta do Azure através de [portal do Azure](https://portal.azure.com).
2. Selecione **do Azure Active Directory** > **registos de aplicação** > **adicionar**.
3. Indique um nome e um URL para a aplicação. Selecione **aplicação Web / API** ou **nativo** para o tipo de aplicação que pretende criar. Depois de definir os valores, selecione **criar**.

### <a name="get-credentials"></a>Obter credenciais

Quando iniciar sessão através de programação, utilize o ID para a sua aplicação e uma chave de autenticação. Para obter estes valores:

1. De **registos de aplicação** no Active Directory, selecione a aplicação.

2. Copie o **ID da Aplicação** e armazene-o no código da aplicação. As aplicações no [aplicações de exemplo](#sample-applications) utilizar **id de cliente** ao fazer referência à **ID da aplicação**.

     ![ID da aplicação para a aplicação](./media/azure-stack-create-service-principal/image12.png)
3. Para gerar uma chave de autenticação, selecione **Chaves**.

4. Indique uma descrição e uma duração para a chave. Quando terminar, selecione **Guardar**.

>[!IMPORTANT]
Depois de guardar a chave, a chave **valor** é apresentado. Anote este valor, porque não é possível obter a chave mais tarde. Armazene o valor da chave num local onde a aplicação o possa obter.

![Aviso de valor de chave para a chave guardado.](./media/azure-stack-create-service-principal/image15.png)

O passo final é [atribuir uma função a sua aplicação](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Criar o serviço principal para o AD FS

Se tiver implementado a pilha do Azure através do AD FS como o arquivo de identidade, pode utilizar o PowerShell para as seguintes tarefas:

* Crie um principal de serviço.
* Atribua principal de serviço a uma função.
* Inicie sessão com a identidade de um principal de serviço.

Para obter mais informações sobre como criar o principal de serviço, consulte [criar serviço principal para o AD FS](../azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

## <a name="assign-the-service-principal-to-a-role"></a>Atribuir o principal de serviço a uma função

Para aceder a recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida qual a função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, consulte [RBAC: funções incorporadas](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
Pode definir o âmbito da função no nível de uma subscrição, um grupo de recursos ou um recurso. As permissões são herdadas a níveis inferiores de âmbito. Por exemplo, uma aplicação com a função de leitor para um grupo de recursos significa que a aplicação pode ler qualquer um dos recursos no grupo de recursos.

Utilize os passos seguintes como guia para atribuir uma função a um principal de serviço.

1. No portal do Azure pilha, navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito de subscrição, selecione **subscrições**.

2. Selecione a subscrição para atribuir a aplicação. Neste exemplo, a subscrição está Visual Studio Enterprise.

     ![Selecione a subscrição do Visual Studio Enterprise para atribuição](./media/azure-stack-create-service-principal/image16.png)

3. Selecione **controlo de acesso (IAM)** da subscrição.

     ![Selecione o controlo de acesso](./media/azure-stack-create-service-principal/image17.png)

4. Selecione **Adicionar**.

5. Selecione a função que pretende atribuir à aplicação.

6. Procure a aplicação e selecione-o.

7. Selecione **OK** para concluir a atribuição de função. Pode ver a aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

Agora que já criou um principal de serviço e atribuída uma função, a aplicação pode aceder a recursos de pilha do Azure.

## <a name="next-steps"></a>Passos Seguintes

[Gerir permissões de utilizador](azure-stack-manage-permissions.md)
