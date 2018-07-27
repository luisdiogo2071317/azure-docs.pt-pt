---
title: Criar a identidade para aplicações do Azure no portal | Documentos da Microsoft
description: Descreve como criar uma nova aplicação do Azure Active Directory e o principal de serviço que pode ser utilizado com o controlo de acesso baseado em funções no Azure Resource Manager para gerir o acesso aos recursos.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: 57d017e2320e5cfea15f1716bc3b6518606e2ea4
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282253"
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Utilizar o portal para criar uma aplicação do Azure Active Directory e o principal de serviço que pode aceder aos recursos

Quando tiver o código que precisa de aceder ou modificar os recursos, tem de configurar uma aplicação do Azure Active Directory (AD). Em seguida, pode atribuir as permissões necessárias para a aplicação do AD. Esta abordagem é preferencial para executar a aplicação em suas próprias credenciais, uma vez que pode atribuir permissões para a identidade de aplicação que são diferentes de suas próprias permissões. Normalmente, estas permissões estão restritas a exatamente aquilo que a aplicação precisa de fazer.

Este artigo mostra-lhe como realizar estes passos através do portal. Ele se concentra num aplicativo de inquilino único onde o aplicativo destina-se para ser executada dentro da organização apenas um. Geralmente usa aplicações de inquilino único para aplicações de linha de negócio que são executados dentro da sua organização.

> [!IMPORTANT]
> Em vez de criar um principal de serviço, considere utilizar a Identidade de Serviço Gerida do Azure AD para a identidade da aplicação. O MSI do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory que simplifica a criação de uma identidade para o código. Se o seu código é executado num serviço que suporta o MSI do Azure AD e acede a recursos que suportam a autenticação do Azure Active Directory, o MSI do Azure AD é uma opção melhor para si. Para saber mais sobre o MSI do Azure AD, incluindo os serviços que atualmente o suportam, veja [Identidade de Serviço Gerida para recursos do Azure](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Permissões obrigatórias

Para concluir este artigo, tem de ter permissões suficientes para registar uma aplicação no seu inquilino do Azure AD e atribuir a aplicação a uma função na sua subscrição do Azure. Vamos certificar-se de que tem as permissões certas para executar as etapas.

### <a name="check-azure-active-directory-permissions"></a>Verificar as permissões do Azure Active Directory

1. Selecione **Azure Active Directory**.

   ![selecionar Azure Active Directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. No Azure Active Directory, selecione **Definições do utilizador**.

   ![Selecione as definições de utilizador](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Verifique os **registos das aplicações** definição. Se definido como **Sim**, usuários não-administradores podem registar aplicações AD. Esta definição significa que qualquer utilizador no inquilino do Azure AD pode registar uma aplicação. Pode avançar para [permissões de subscrição do Azure verificar](#check-azure-subscription-permissions).

   ![ver registos de aplicações](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Se a definição dos registos da aplicação estiver definido como **não**, apenas [administradores globais](../active-directory/users-groups-roles/directory-assign-admin-roles.md) podem registar aplicações. Verifique se a sua conta é um administrador de inquilino do Azure AD. Selecione **descrição geral** e examinar suas informações de utilizador. Se a sua conta está atribuída à função de utilizador, mas a definição de registo de aplicação (a partir do passo anterior) está limitada a utilizadores de administração, peça ao seu administrador para a atribuir à função de administrador global ou para permitir que os utilizadores registem aplicações.

   ![Localizar utilizador](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Verifique as permissões de subscrição do Azure

Na sua subscrição do Azure, a conta tem de ter `Microsoft.Authorization/*/Write` acesso para atribuir uma aplicação AD a uma função. Esta ação é concedida através das funções [Proprietário](../role-based-access-control/built-in-roles.md#owner) ou [Administrador de Acesso dos Utilizadores](../role-based-access-control/built-in-roles.md#user-access-administrator). Se a sua conta é atribuída para o **contribuinte** função, não tem permissão adequada. Receber um erro ao tentar atribuir o principal de serviço a uma função.

Para verificar as permissões de subscrição:

1. Selecione a sua conta no canto superior direito e selecione **as minhas permissões**.

   ![Selecione as permissões de utilizador](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Na lista pendente, selecione a subscrição. Selecione **clique aqui para acesso completo de ver os detalhes para esta subscrição**.

   ![Localizar utilizador](./media/resource-group-create-service-principal-portal/view-details.png)

1. Ver as funções atribuídas e determinar se tem as permissões adequadas para atribuir uma aplicação AD a uma função. Caso contrário, peça ao administrador de subscrição para o adicionar a função de administrador de acesso do utilizador. Na imagem seguinte, o utilizador tem atribuída à função de proprietário, o que significa que o utilizador tem as permissões adequadas.

   ![Mostrar permissões](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Criar uma aplicação do Azure Active Directory

1. Inicie sessão na sua conta do Azure através da [portal do Azure](https://portal.azure.com).
1. Selecione **Azure Active Directory**.

   ![selecionar Azure Active Directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecione **Registos das aplicações**.

   ![selecionar registos das aplicações](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Selecione **Novo registo de aplicação**.

   ![adicionar aplicação](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Indique um nome e um URL para a aplicação. Selecione **aplicação / API Web** no tipo de aplicação que quer criar. Não é possível criar as credenciais para uma [aplicativo nativo](../active-directory/manage-apps/application-proxy-configure-native-client-application.md); portanto, que tipo não funciona para um aplicativo automatizado. Depois de definir os valores, selecione **criar**.

   ![dar nome à aplicação](./media/resource-group-create-service-principal-portal/create-app.png)

Criou seu aplicativo.

## <a name="get-application-id-and-authentication-key"></a>Obter a chave de ID e a autenticação da aplicação

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

## <a name="assign-application-to-role"></a>Atribua a aplicação à função

Para acessar recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida que função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md).

Pode definir o âmbito no nível da subscrição, no grupo de recursos ou ao recurso. As permissões são herdadas para níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação à função do leitor para um grupo de recursos significa pode ler o grupo de recursos e todos os recursos que nele contidos.

1. Navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito da subscrição, selecione **subscrições**. Em vez disso, pode selecionar um grupo de recursos ou recurso.

   ![Selecione a subscrição](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Selecione a subscrição específica (grupo de recursos ou recurso) para atribuir a aplicação.

   ![Selecione a subscrição para atribuição](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Selecione **controlo de acesso (IAM)**.

   ![Selecione o acesso](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Selecione **Adicionar**.

   ![Selecione adicionar](./media/resource-group-create-service-principal-portal/select-add.png)

1. Selecione a função que pretende atribuir à aplicação. Para permitir que o aplicativo execute ações como **reinicie**, **iniciar** e **parar** instâncias, tem de ter que selecionar a função **contribuinte**. A imagem seguinte mostra os **leitor** função.

   ![Selecionar função](./media/resource-group-create-service-principal-portal/select-role.png)

1. Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a sua aplicação, tem de fornecer o nome do mesmo no campo de pesquisa. Selecione-o.

   ![Procure a aplicação](./media/resource-group-create-service-principal-portal/search-app.png)

1. Selecione **guardar** para concluir a atribuir a função. Pode ver a sua aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

## <a name="next-steps"></a>Passos Seguintes
* Para configurar uma aplicação multi-inquilino, consulte [Guia do programador para autorização com a API do Azure Resource Manager](resource-manager-api-authentication.md).
* Para saber mais sobre como especificar políticas de segurança, veja [controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md).  
* Para obter uma lista de ações disponíveis que pode ser concedido ou negado para os utilizadores, consulte [operações de fornecedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md).
