---
title: Como configurar gerido identidades para recursos do Azure na VM do Azure com o portal do Azure
description: Instruções passo a passo instruções para configurar geridos identidades para recursos do Azure na VM do Azure com o portal do Azure.
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
ms.openlocfilehash: 5e6d15adeb11c75b3e7cb27c834ea0b6dfa4c42a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337195"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Configurar identidades geridas para recursos do Azure numa VM com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e desativar o sistema e de identidades geridas atribuído ao utilizador para uma Azure Virtual Machine (VM), com o portal do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem da atribuição de função seguinte:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para ativar e remover atribuído de sistema de identidade gerida de uma VM do Azure.

## <a name="system-assigned-managed-identity"></a>Atribuído ao sistema de identidade gerida

Nesta secção, saiba como ativar e desativar a identidade gerida atribuído de sistema para a VM no portal do Azure.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Ativar a identidade gerida atribuído ao sistema durante a criação de uma VM

Atualmente, o portal do Azure não suporta a ativar a identidade do sistema atribuídos durante a criação de uma VM. Em vez disso, consulte um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM e, em seguida, avance para a secção seguinte para obter detalhes sobre como ativar a identidade atribuída de sistema na VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Ativar a identidade gerida atribuído de sistema numa VM existente

Para ativar a identidade gerida atribuído de sistema numa VM que foi originalmente aprovisionada sem ele:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.

2. Navegue para a Máquina Virtual pretendida e selecione **identidade**.

3. Sob **sistema atribuído**, **estado**, selecione **no** e, em seguida, clique em **guardar**:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Remover sistema atribuído a identidade gerida de uma VM

Se tiver uma Máquina Virtual que já não tem atribuído o sistema de identidade gerida:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM. 

2. Navegue para a Máquina Virtual pretendida e selecione **identidade**.

3. Sob **sistema atribuído**, **estado**, selecione **desligado** e, em seguida, clique em **guardar**:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Atribuído ao utilizador a identidade gerida

 Nesta secção, saiba como adicionar e remover uma identidade gerida atribuído ao utilizador a partir de uma VM com o portal do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Atribuir uma identidade de utilizador atribuída durante a criação de uma VM

Atualmente, o portal do Azure não suporta a atribuição de uma identidade gerida atribuído ao utilizador durante a criação de uma VM. Em vez disso, consulte um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM e, em seguida, avance para a secção seguinte para obter detalhes sobre uma identidade gerida atribuído ao utilizador a atribuir à VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Atribuir uma identidade gerida atribuído ao utilizador a uma VM existente

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.
2. Navegue para a VM e clique em pretendido **identidade**, **atribuída ao utilizador** e, em seguida  **\+adicionar**.

   ![Adicionar identidade gerida atribuído ao utilizador a VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Clique a identidade atribuído ao utilizador que pretende adicionar à VM e, em seguida, clique em **adicionar**.

    ![Adicionar identidade gerida atribuído ao utilizador a VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Remover uma identidade gerida atribuído ao utilizador a partir de uma VM

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.
2. Navegue para a VM e clique em pretendido **identidade**, **atribuída ao utilizador**, o nome do atribuído ao utilizador gerido identidade que pretende eliminar e, em seguida, clique em **remover** (clique em  **Sim** no painel de confirmação).

   ![Remover utilizador atribuído a identidade gerida de uma VM](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Passos Seguintes

- Utilizar o portal do Azure, atribua a identidade gerida de uma VM do Azure [acesso a outro recurso do Azure](howto-assign-access-portal.md).

