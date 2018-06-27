---
title: Ganhar visibilidade em todo o inquilino para o Centro de segurança do Azure | Microsoft Docs
description: Saiba mais sobre obter visibilidade ao nível do inquilino no Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: terrylan
ms.openlocfilehash: 2c95b06ce34b850d1bfaf60e47d6e5fede148a38
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025948"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Ganhar visibilidade em todo o inquilino para o Centro de segurança do Azure
Este artigo ajuda-o a começar a utilizar ao efetuar várias ações que maximizem as vantagens que do Centro de segurança do Azure fornece. Estas ações a efetuar permite-lhe ganhar visibilidade em todos os gerir as subscrições do Azure que estejam ligadas ao seu inquilino do Azure Active Directory e eficaz postura de segurança da sua organização em escala ao aplicar políticas de segurança em múltiplas subscrições de forma aggregative.

## <a name="management-groups"></a>Grupos de gestão
Grupos de gestão do Azure fornecem a capacidade de gerir de forma eficiente o acesso, políticas e elaboração de relatórios em grupos de subscrições, bem como gerir de forma eficaz a propriedade do Azure completa, efetuando as ações no grupo de gestão de raiz. Cada inquilino do Azure AD é atribuído um grupo de gestão de nível superior única denominado o grupo de gestão raiz. Este grupo de gestão raiz está incorporado na hierarquia de ter todos os grupos de gestão e subscrições fold cópias de segurança ao mesmo. Permite que este grupo de políticas globais e atribuições de RBAC a aplicar ao nível do diretório. 

O grupo de gestão de raiz é criado automaticamente quando o fizer qualquer uma das seguintes ações: 
1. Optar por utilizar grupos de gestão do Azure, acedendo a **grupos de gestão** no [portal do Azure](https://portal.azure.com).
2. Crie um grupo de gestão através de uma chamada à API.
3. Crie um grupo de gestão com o PowerShell.

Para obter uma descrição detalhada dos grupos de gestão, consulte o [organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md) artigo.

## <a name="create-a-management-group-in-the-azure-portal"></a>Criar um grupo de gestão no portal do Azure
Pode organizar as subscrições em grupos de gestão e aplicar as políticas de governação para os grupos de gestão. Todas as subscrições dentro de um grupo de gestão herdam automaticamente as políticas aplicadas ao grupo de gestão. Enquanto os grupos de gestão não são necessários para carregar o Centro de segurança, recomenda-se vivamente que crie, pelo menos, um grupo de gestão para que o grupo de gestão de raiz é criado. Depois do grupo for criado, todas as subscrições no inquilino do Azure AD serão ligadas ao mesmo. Para obter instruções do PowerShell e obter mais informações, consulte [criar grupos de gestão para a gestão de recursos e organização](../azure-resource-manager/management-groups-create.md).

 
1. Inicie sessão no [portal do Azure](http://portal.azure.com).
2. Selecione **todos os serviços** > **grupos de gestão**.
3. Na página principal, selecione **grupo de gestão novo.** 

    ![Grupo principal](./media/security-center-management-groups/main.png) 
4.  Preencha o campo de ID do grupo de gestão. 
    - O **ID do grupo de gestão** é o identificador exclusivo do diretório que é utilizado para submeter os comandos neste grupo de gestão. Este identificador não é editável após a criação que está a ser utilizada em todo o sistema do Azure para identificar este grupo. 
    - O campo de nome de apresentação é o nome que é apresentado no portal do Azure. Um nome a apresentar separado é um campo opcional ao criar a gestão de grupo e podem ser alteradas em qualquer altura.  

      ![Criar](./media/security-center-management-groups/create_context_menu.png)  
5.  Selecione **guardar**

### <a name="view-management-groups-in-the-azure-portal"></a>Vista de grupos de gestão no portal do Azure
1. Iniciar sessão para o [portal do Azure](http://portal.azure.com).
2. Para ver grupos de gestão, selecione **todos os serviços** no menu principal do Azure.
3. Em **geral**, selecione **grupos de gestão**.

    ![Criar um grupo de gestão](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Conceder a visibilidade de nível de inquilino e a capacidade de atribuir políticas

Para obter visibilidade a postura de segurança de todas as subscrições registado no inquilino do Azure AD, uma função RBAC com permissões suficientes de leitura é necessária para ser atribuído no grupo de gestão de raiz.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Elevar o acesso de administrador global no Azure Active Directory
Um administrador de inquilino do Azure Active Directory não tiver acesso direto a subscrições do Azure. No entanto, como um administrador de diretório, tiverem o direito de elevar si próprios para uma função que tenha acesso. Um administrador de inquilino do Azure AD tem de efetuar a elevação próprio ao administrador de acesso de utilizador ao nível do grupo de gestão raiz para que eles podem atribuir funções RBAC. Para instruções do PowerShell e informações adicionais, consulte [elevar o acesso de Administrador Global no Azure Active Directory](../role-based-access-control/elevate-access-global-admin.md). 


1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) ou [Centro de administração do Azure Active Directory](https://aad.portal.azure.com).

2. Na lista de navegação, clique em **do Azure Active Directory** e, em seguida, clique em **propriedades**.

   ![Propriedades do Azure AD - captura de ecrã](./media/security-center-management-groups/aad-properties.png)

3. Em **Administrador Global pode gerir subscrições do Azure e grupos de gestão**, defina o comutador para **Sim**.

   ![Administrador global pode gerir subscrições do Azure e grupos de gestão - captura de ecrã](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Quando configurar o comutador **Sim**, a sua conta de Administrador Global (atualmente com sessão iniciada no utilizador) está adicionada à função de administrador de acesso de utilizador no Azure RBAC no âmbito de raiz (`/`), que concede acesso para ver e o relatório no todas as subscrições Azure associadas inquilino do Azure AD.

   - Quando configurar o comutador **não**, a conta de Administrador Global (atualmente com sessão iniciada no utilizador) é removida da função de administrador de acesso de utilizador no RBAC do Azure. Não é possível ver todas as subscrições do Azure que estão associadas ao inquilino do Azure AD e pode ver e gerir apenas as subscrições do Azure para que lhe foi concedido acesso.

4. Clique em **guardar** para guardar a definição.

    - Esta definição não é uma propriedade global e aplica-se apenas ao utilizador atualmente com sessão iniciado.

5. Efetue as tarefas que necessita para fazer o acesso elevados. Quando tiver terminado, defina o comutador para **não**.

### <a name="assign-rbac-roles-to-users"></a>Atribuir funções RBAC a utilizadores
Assim que um administrador inquilino tem elevados acesso, que poderá atribuir uma função RBAC para utilizadores relevantes no nível de grupo de gestão de raiz. A função recomendada para atribuir é [ **leitor**](../role-based-access-control/built-in-roles.md#reader). Esta função é necessária para fornecer a visibilidade de nível de inquilino. A função atribuída será propagada automaticamente para todos os grupos de gestão e as subscrições sob o grupo de gestão raiz. Para obter mais informações sobre as funções de RBAC, consulte [funções disponíveis](../active-directory/active-directory-assign-admin-roles-azure-portal.md#available-roles).

1. Instale o [Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Execute os seguintes comandos: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. Quando lhe for solicitado, inicie sessão com credenciais de administrador global. 

    ![Inicie sessão no pedido de captura de ecrã](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Conceda permissões de função de leitor executando o seguinte comando:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Para remover a função, utilize o seguinte comando: 

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

<!-- Currently, PowerShell method only 6/26/18

1. Sign in to the [Azure portal](https://portal.azure.com). 
2. To view management groups, select **All services** under the Azure main menu then select **Management Groups**.
3.  Select a management group and click **details**.

    ![Management Groups details screenshot](./media/security-center-management-groups/management-group-details.PNG)
 
4. Click **Access control (IAM)** then **Add**.
5. Select the role to assign and the user, then click **Save**.  
   
   ![Add Security Reader role screenshot](./media/security-center-management-groups/asc-security-reader.png)
-->

### <a name="remove-elevated-access"></a>Remover o acesso elevado 
Assim que as funções do RBAC tenham sido atribuídas a utilizadores, o administrador de inquilino deve remover o próprio da função de administrador de acesso de utilizador.

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) ou [Centro de administração do Azure Active Directory](https://aad.portal.azure.com).

2. Na lista de navegação, clique em **do Azure Active Directory** e, em seguida, clique em **propriedades**.

3. Em **Administrador Global pode gerir subscrições do Azure e grupos de gestão**, defina o comutador para **não**.

4. Clique em **guardar** para guardar a definição.

### <a name="open-or-refresh-security-center"></a>Abrir ou Atualize o Centro de segurança
Depois de ter atribuídos as funções RBAC, abrir, ou Atualize o Centro de segurança do Azure para verificar que tem visibilidade para todas as subscrições no inquilino do Azure AD. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Selecione **todos os serviços** no menu principal do Azure, em seguida, selecione **Centro de segurança**.
3. No **descrição geral**, há um gráfico de cobertura de subscrição. 
    ![Captura de ecrã de gráfico de cobertura de subscrição](./media/security-center-management-groups/security-center-subscription-coverage.png)
4. Clique em **cobertura** para ver a lista de subscrições abrangidos. 
    ![Captura de ecrã de lista de cobertura de subscrição](./media/security-center-management-groups/security-center-coverage.png)

## <a name="adding-subscriptions-to-a-management-groups"></a>Adicionar subscrições para grupos de gestão
Pode adicionar as subscrições para o grupo de gestão que criou. Estes passos não são obrigatórios para obter visibilidade ao nível do inquilino e de política global e de gestão de acesso.

1. Em **grupos de gestão**, selecione um grupo de gestão para adicionar a subscrição.

    ![Selecione um grupo de gestão para adicionar subscrição](./media/security-center-management-groups/management-group-subscriptions.png)

2. Selecione **adicionar existente**.

    ![Adicionar existente](./media/security-center-management-groups/add-existing.png)

3. Introduza a subscrição em **adicionar recurso existente** e clique em **guardar**.

4. Repita os passos 1 a 3 até que adicionou todas as subscrições no âmbito.

 > [!NOTE]
 > Grupos de gestão podem conter subscrições e grupos de gestão de subordinados. Quando atribui um utilizador de uma função RBAC para o grupo de gestão principal, o acesso é herdado pelos subscrições do grupo de gestão secundário. As políticas definidas no grupo de gestão principal também são herdadas pelo subordinados. 

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a ganhar visibilidade em todo o inquilino para o Centro de segurança do Azure. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

> [!div class="nextstepaction"]
> [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md)

