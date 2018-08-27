---
title: Como configurar a identidade do serviço gerido na VM do Azure com o portal do Azure
description: Passo a passo de instruções para configurar uma identidade do serviço gerido na VM do Azure com o portal do Azure.
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
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 552fce2ffd8b6bd786010da82e702ee98c3f8647
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888561"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Configurar uma VM identidade do serviço gerido com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e desativar o sistema e para uma Azure Virtual Machine (VM), com o portal do Azure de identidade atribuída ao utilizador. 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem da atribuição de função seguinte:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para ativar e remover a identidade de sistema atribuída de uma VM do Azure.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, saiba como ativar e desativar o sistema de identidade atribuído para a VM no portal do Azure.

### <a name="enable-system-assigned-identity-during-creation-of-a-vm"></a>Ativar o sistema de identidade atribuído durante a criação de uma VM

Atualmente, o portal do Azure não suporta a ativar identidade de sistema atribuída durante a criação de uma VM. Em vez disso, consulte um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM e, em seguida, avance para a secção seguinte para obter detalhes sobre como ativar a identidade de sistema atribuído na VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-identity-on-an-existing-vm"></a>Ativar o sistema de identidade numa VM existente atribuído

Para ativar o sistema de identidade numa VM que foi originalmente aprovisionada sem ele atribuída:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.

2. Navegue para a Máquina Virtual pretendida e selecione **identidade**.

3. Sob **sistema atribuído**, **estado**, selecione **no** e, em seguida, clique em **guardar**:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-identity-from-a-vm"></a>Remover o sistema de identidade atribuído a partir de uma VM

Se tiver uma Máquina Virtual que já não precisa da identidade do sistema atribuído:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM. 

2. Navegue para a Máquina Virtual pretendida e selecione **identidade**.

3. Sob **sistema atribuído**, **estado**, selecione **desligado** e, em seguida, clique em **guardar**:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

 Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de uma VM com o portal do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Atribuir um utilizador a identidade atribuído durante a criação de uma VM

Atualmente, o portal do Azure não suporta a atribuir um utilizador a identidade atribuído durante a criação de uma VM. Em vez disso, consulte um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM e, em seguida, avance para a secção seguinte para obter detalhes sobre como atribuir um utilizador a identidade atribuído à VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-identity-to-an-existing-vm"></a>Atribuir um utilizador a identidade atribuído a uma VM existente

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.
2. Navegue para a VM e clique em pretendido **identidade**, **atribuída ao utilizador** e, em seguida  **\+adicionar**.

   ![Adicionar a identidade de utilizador atribuído à VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Clique a identidade atribuída ao utilizador que pretende adicionar à VM e, em seguida, clique em **adicionar**.

    ![Adicionar a identidade de utilizador atribuído à VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-vm"></a>Remover um utilizador atribuído a identidade de uma VM

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.
2. Navegue para a VM e clique em pretendido **identidade**, **atribuída ao utilizador**, o nome da identidade atribuída ao utilizador que pretende eliminar e, em seguida, clique em **remover** (clique em **Sim** no painel de confirmação).

   ![Remover a identidade atribuída a partir de uma VM ao utilizador](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral de identidade do serviço gerido, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Com o portal do Azure, atribua a identidade do serviço gerido da VM do Azure [acesso a outro recurso do Azure](howto-assign-access-portal.md).

