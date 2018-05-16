---
title: Criar a identidade da aplicação do Azure no portal | Microsoft Docs
description: Descreve como criar uma nova aplicação do Azure Active Directory e um principal de serviço que pode ser utilizada com o controlo de acesso baseado em funções no Gestor de recursos do Azure para gerir o acesso aos recursos.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: e5d93963dddb4acb1147042ae338b32cb5d7646f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Utilize o portal para criar uma aplicação Azure Active Directory e um principal de serviço que pode aceder a recursos

Quando tiver de código que precisa de aceder ou modificar recursos, tem de configurar uma aplicação do Azure Active Directory (AD). Atribuir as permissões necessárias para a aplicação AD. Esta abordagem é preferível executar a aplicação com as suas próprias credenciais, porque pode atribuir permissões para a identidade de aplicação que sejam diferentes a suas própria permissões. Normalmente, estas permissões são restritos exatamente o que a aplicação tem de fazer.

Este artigo mostra como efetuar os passos através do portal. Concentra-se uma aplicação do inquilino único onde a aplicação foi concebida para ser executada dentro da organização apenas um. Normalmente utiliza aplicações de inquilino único para aplicações de linha de negócio que são executadas dentro da sua organização.

> [!IMPORTANT]
> Em vez de criar um principal de serviço, considere utilizar identidade de serviço gerida do Azure AD para a identidade da aplicação. Azure AD MSI é uma funcionalidade de pré-visualização pública do Azure Active Directory simplifica a criação de uma identidade para o código. Se o seu código é executado num serviço que suporta o Azure AD MSI e acede a recursos que suportam a autenticação do Azure Active Directory, Azure AD MSI é uma opção melhor para si. Para saber mais sobre o Azure AD MSI, incluindo os serviços atualmente o suporta, consulte [identidade de serviço geridas para recursos do Azure](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Permissões obrigatórias

Para concluir este artigo, tem de ter permissões suficientes para registar uma aplicação com o seu inquilino do Azure AD e atribuir a aplicação a uma função na sua subscrição do Azure. Vamos certificar-se de que tem as permissões corretas para efetuar esses passos.

### <a name="check-azure-active-directory-permissions"></a>Verifique as permissões do Azure Active Directory

1. Selecione **Azure Active Directory**.

   ![selecionar Azure Active Directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. No Azure Active Directory, selecione **as definições de utilizador**.

   ![Selecione as definições de utilizador](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Verifique o **registos de aplicação** definição. Se definido como **Sim**, os utilizadores que não podem registar aplicações AD. Esta definição significa que qualquer utilizador no inquilino do Azure AD pode registar uma aplicação. Para poder continuar para [permissões de subscrição do Azure verifique](#check-azure-subscription-permissions).

   ![registos de aplicação de vista](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Se os registos de aplicação definição estiver definida como **não**, apenas [administradores globais](../active-directory/active-directory-assign-admin-roles-azure-portal.md) pode registar aplicações. Verifique se a sua conta é um administrador de inquilino do Azure AD. Selecione **descrição geral** e observe as informações do utilizador. Se a sua conta está atribuída à função de utilizador, mas a definição de registo de aplicação (a partir do passo anterior) está limitada a utilizadores de administração, peça ao seu administrador para qualquer lhe atribuir a função de administrador global ou permitir que os utilizadores registar as aplicações.

   ![Localizar utilizador](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Verifique as permissões de subscrição do Azure

Na sua subscrição do Azure, a conta tem de ter `Microsoft.Authorization/*/Write` acesso para atribuir uma aplicação AD a uma função. Esta ação é concedida através de [proprietário](../role-based-access-control/built-in-roles.md#owner) função ou [administrador de acesso de utilizador](../role-based-access-control/built-in-roles.md#user-access-administrator) função. Se a sua conta está atribuída a **contribuinte** função, não tem permissão suficiente. Recebe um erro ao tentar atribuir o principal de serviço a uma função.

Para verificar as permissões de subscrição:

1. Selecione a sua conta no canto superior direito e selecione **as minhas permissões**.

   ![Selecione as permissões de utilizador](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Na lista pendente, selecione a subscrição. Selecione **clique aqui para ver o acesso completo os detalhes para esta subscrição**.

   ![Localizar utilizador](./media/resource-group-create-service-principal-portal/view-details.png)

1. Ver as funções atribuídas e determinar se tem permissões adequadas para atribuir uma aplicação AD a uma função. Caso contrário, peça ao seu administrador de subscrição para adicioná-lo à função de administrador de acesso de utilizador. Na imagem seguinte, o utilizador está atribuído à função de proprietário, o que significa que o utilizador tem permissões adequadas.

   ![Mostrar permissões](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Criar uma aplicação do Azure Active Directory

1. Inicie sessão na sua conta do Azure através de [portal do Azure](https://portal.azure.com).
1. Selecione **Azure Active Directory**.

   ![selecionar Azure Active Directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecione **Registos das aplicações**.

   ![selecionar registos das aplicações](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Selecione **Novo registo de aplicação**.

   ![adicionar aplicação](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Indique um nome e um URL para a aplicação. Selecione **aplicação / API Web** no tipo de aplicação que quer criar. Não é possível criar as credenciais para um [aplicação nativa](../active-directory/manage-apps/application-proxy-configure-native-client-application.md); por conseguinte, se o tipo não funcionam para uma aplicação automatizada. Depois de definir os valores, selecione **criar**.

   ![dar nome à aplicação](./media/resource-group-create-service-principal-portal/create-app.png)

Foi criada com a aplicação.

## <a name="get-application-id-and-authentication-key"></a>Obter a chave de autenticação e ID de aplicação

Ao iniciar sessão programaticamente, precisa do ID da sua aplicação e de uma chave de autenticação. Para obter esses valores, utilize os seguintes passos:

1. Em **Registos das aplicações** no Azure Active Directory, selecione a aplicação.

   ![selecionar aplicação](./media/resource-group-create-service-principal-portal/select-app.png)

1. Copie o **ID da Aplicação** e armazene-o no código da aplicação. Algumas [aplicações de exemplo](#log-in-as-the-application) referem-se a este valor como o ID de cliente.

   ![ID de cliente](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Para gerar uma chave de autenticação, selecione **Definições**.

   ![selecionar as definições](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Para gerar uma chave de autenticação, selecione **Chaves**.

   ![selecionar chaves](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Indique uma descrição e uma duração para a chave. Quando terminar, selecione **Guardar**.

   ![guardar chave](./media/resource-group-create-service-principal-portal/save-key.png)

   Depois de guardar a chave, o valor da mesma é apresentado. Copie este valor, porque não vai conseguir obter a chave mais tarde. O valor da chave é indicado com o ID da aplicação para iniciar sessão como esta. Armazene o valor da chave num local onde a aplicação o possa obter.

   ![chave guardada](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Obter o ID de inquilino

Ao iniciar sessão programaticamente, tem de transmitir o ID de inquilino com o pedido de autenticação.

1. Selecione **Azure Active Directory**.

   ![selecionar Azure Active Directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Para obter o ID de inquilino, selecione as **Propriedades** do seu inquilino do Azure AD.

   ![selecionar propriedades do Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Copie o **ID do Diretório**. Este valor é o ID do inquilino.

   ![ID do inquilino](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Atribuir a aplicação a função

Para aceder a recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida qual a função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, consulte [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md).

Pode definir o âmbito ao nível da subscrição, do grupo de recursos ou do recurso. As permissões são herdadas a níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação para a função de leitor para um grupo de recursos significa podem ler o grupo de recursos e todos os recursos que nele contidos.

1. Navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito de subscrição, selecione **subscrições**. Em vez disso, pode selecionar um grupo de recursos ou um recurso.

   ![Selecione a subscrição](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Selecione a subscrição específica (grupo de recursos ou recursos) para atribuir a aplicação.

   ![Selecione a subscrição de atribuição](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Selecione **(IAM) do controlo de acesso**.

   ![Selecione o acesso](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Selecione **Adicionar**.

   ![Selecionar adicionar](./media/resource-group-create-service-principal-portal/select-add.png)

1. Selecione a função que pretende atribuir à aplicação. A imagem seguinte mostra o **leitor** função.

   ![Selecione a função](./media/resource-group-create-service-principal-portal/select-role.png)

1. Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a aplicação, tem de fornecer o nome do mesmo no campo de pesquisa. Selecione-o.

   ![Procure a aplicação](./media/resource-group-create-service-principal-portal/search-app.png)

1. Selecione **guardar** para concluir a atribuição de função. Verá a aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

## <a name="next-steps"></a>Passos Seguintes
* Para configurar uma aplicação multi-inquilino, consulte [guia para programadores para autorização com a API do Azure Resource Manager](resource-manager-api-authentication.md).
* Para saber mais sobre como especificar políticas de segurança, consulte [controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md).  
* Para obter uma lista de ações disponíveis que pode ser concedeu ou negou aos utilizadores, consulte [operações de fornecedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md).
