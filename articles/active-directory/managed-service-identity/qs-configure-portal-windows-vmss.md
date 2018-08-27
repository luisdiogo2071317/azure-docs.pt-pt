---
title: Configurar a identidade do serviço gerido num conjunto com o portal do Azure de dimensionamento de máquina virtual do Azure
description: Passo a passo as instruções para configurar uma identidade de serviço geridas no VMSS do Azure, com o portal do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: cbe2e3d9f60ced5c707ce5a701a5aac937ccc072
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887994"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Configurar uma máquina virtual do conjunto de dimensionamento da identidade do serviço gerido com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e desativar o sistema e de identidade para um conjunto com o portal do Azure de dimensionamento de máquina virtual atribuída ao utilizador.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem da atribuição de função seguinte:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para ativar e remover sistema atribuído a identidade gerida a partir de um conjunto de dimensionamento de máquina virtual.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído 

Nesta secção, irá aprender como ativar e desativar o sistema de identidade num conjunto com o portal do Azure de dimensionamento de máquina virtual atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído durante a criação de um conjunto de dimensionamento de máquinas virtuais

Atualmente, o portal do Azure não suporta a ativar sistema de identidade atribuído durante a criação de um conjunto de dimensionamento de máquina virtual. Em vez disso, veja o máquina virtual dimensionamento conjunto criação manual de início rápido artigo seguinte para primeiro criar um conjunto de dimensionamento de máquina virtual e, em seguida, avance para a secção seguinte para obter detalhes sobre como ativar o sistema de identidade num conjunto de dimensionamento de máquina virtual atribuído:

- [Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-identity-on-an-existing-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual existente

Para ativar o sistema de identidade num conjunto de dimensionamento de máquina virtual que foi originalmente aprovisionado sem ele atribuída:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Sob **sistema atribuído**, **estado**, selecione **no** e, em seguida, clique em **guardar**:

   [![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-identity-from-a-virtual-machine-scale-set"></a>Remover a identidade de sistema atribuído de um conjunto de dimensionamento de máquinas virtuais

Se tiver um conjunto de dimensionamento que já não precisa de um sistema de identidade atribuído:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita no conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Sob **sistema atribuído**, **estado**, selecione **desligado** e, em seguida, clique em **guardar**:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de um conjunto de dimensionamento no portal do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Atribuir uma identidade atribuída ao utilizador durante a criação de um conjunto de dimensionamento de máquinas virtuais

Atualmente, o portal do Azure não suporta a atribuir um utilizador a identidade atribuído durante a criação de um conjunto de dimensionamento de máquina virtual. Em vez disso, veja o máquina virtual dimensionamento conjunto criação manual de início rápido artigo seguinte para primeiro criar um conjunto de dimensionamento de máquina virtual e, em seguida, avance para a secção seguinte para obter detalhes sobre a atribuição de um utilizador atribuído identidade:

- [Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-identity-to-an-existing-virtual-machine-scale-set"></a>Atribuir uma identidade de utilizador atribuída a um conjunto de dimensionamento de máquina virtual existente

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.
2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida e clique em **identidade**, **atribuída ao utilizador** e, em seguida  **\+adicionar**.

   ![Adicionar identidade atribuída ao utilizador para o VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Clique a identidade atribuída ao utilizador que pretende adicionar ao conjunto de dimensionamento de máquina virtual e, em seguida, clique em **adicionar**.
   
   ![Adicionar identidade atribuída ao utilizador para o VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Remover uma identidade atribuída ao utilizador a partir de um conjunto de dimensionamento de máquina virtual

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.
2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida e clique em **identidade**, **atribuída ao utilizador**, o nome da identidade atribuída ao utilizador que pretende eliminar e, em seguida, clique em **remover** ( Clique em **Sim** no painel de confirmação).

   ![Remover utilizador atribuído a identidade de um VMSS](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral de identidade do serviço gerido, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Com o portal do Azure, forneça um dimensionamento de máquina virtual do Azure definir a identidade do serviço gerido [acesso a outro recurso do Azure](howto-assign-access-portal.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
