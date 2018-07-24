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
ms.openlocfilehash: 6ba090065b18a44cc1f01a62eefb5dcf52bcf356
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213270"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Configurar uma VM identidade do serviço gerido com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a ativar e desativar o sistema de identidade atribuído para uma VM do Azure, com o portal do Azure. Atribuir e remover utilizador identidades atribuído VMS do Azure não é atualmente suportado no portal do Azure.

> [!NOTE]
> Atualmente, o utilizador atribuído a operações de identidade não são suportadas no portal do Azure. Volte mais tarde para obter atualizações. 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem da atribuição de função seguinte:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para ativar e remover a identidade de sistema atribuída de uma VM do Azure.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Identidade de serviço gerida durante a criação de uma VM do Azure

Atualmente, a criação de VM através do portal do Azure não suporta operações de identidade do serviço gerido. Em vez disso, veja um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar a identidade do serviço gerido na VM.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Ativar a identidade de serviço gerido numa VM do Azure existente

Para ativar o sistema de identidade numa VM que foi originalmente aprovisionada sem ele atribuída:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM.

2. Navegue para a Máquina Virtual pretendida e selecione a página "Configuração".

3. Ativar a identidade de sistema atribuído na VM ao selecionar "Sim" em "Identidade de serviço gerida" e, em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   > [!NOTE]
   > Adicionar uma identidade de utilizador atribuída a uma VM não é atualmente suportado no portal do Azure.

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Remover a identidade de serviço gerida de uma VM do Azure

Se tiver uma Máquina Virtual que já não precisa da identidade do sistema atribuído:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM. 

2. Navegue para a Máquina Virtual pretendida e selecione a página "Configuração".

3. Desativar o sistema de identidade na VM de atribuída ao selecionar "Não" em "Identidade de serviço gerida" e, em seguida, clique em Guardar. Esta operação pode demorar 60 segundos ou mais para concluir:

    > [!NOTE]
    > Adicionar uma identidade de utilizador atribuída a uma VM não é atualmente suportado no portal do Azure.

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral de identidade do serviço gerido, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Com o portal do Azure, atribua a identidade do serviço gerido da VM do Azure [acesso a outro recurso do Azure](howto-assign-access-portal.md).

