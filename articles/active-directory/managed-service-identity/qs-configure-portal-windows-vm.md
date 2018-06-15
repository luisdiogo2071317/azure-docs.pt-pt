---
title: Como configurar MSI numa VM do Azure no portal do Azure
description: Passo pelo passo as instruções para configurar uma identidade de serviço geridas (MSI) na VM do Azure, utilizando o portal do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 62b8504f5c10f338539d263bb231cf96eb405ba6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930345"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Configurar uma VM geridos serviço de identidade (MSI) no portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender como ativar e desativar o sistema atribuído de identidade para uma VM do Azure, utilizando o portal do Azure. Atribuir e remover utilizador atribuído identidades de VMs do Azure não é atualmente suportado através do Portal do Azure.

> [!NOTE]
> Atualmente, o utilizador atribuído operações de identidade não são suportados através do Portal do Azure. Verifique novamente para as atualizações. 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Identidade de serviço geridas durante a criação de uma VM do Azure

Atualmente, a criação de VM através do portal do Azure não suporta operações de identidade de serviço geridas. Em vez disso, consulte um dos seguintes artigos de início rápido de criação de VM para criar primeiro uma VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar a identidade de serviço gerido na VM.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Ativar a identidade de serviço gerida numa VM do Azure existente

Para ativar o sistema atribuído a identidade de uma VM que foi originalmente aprovisionada sem-lo:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual".

2. Navegue para a Máquina Virtual pretendida e selecione a página "Configuration".

3. Ativar a identidade do sistema atribuído na VM ao selecionar "Sim" em "Identidade de serviço gerida" e, em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

    > [!NOTE]
    > A adição de uma identidade de utilizador atribuída a uma VM não é suportada atualmente através do Portal do Azure.

   ![Captura de ecrã de página de configuração](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Remover a identidade de serviço gerida de uma VM do Azure

Se tiver uma Máquina Virtual que já não tem a identidade do sistema atribuído:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual".

2. Navegue para a Máquina Virtual pretendida e selecione a página "Configuration".

3. Desative o sistema atribuído de identidade da VM, selecionando "Não" em "Identidade de serviço gerida", em seguida, clique em Guardar. Esta operação pode demorar 60 segundos ou mais para concluir:

    > [!NOTE]
    > A adição de uma identidade de utilizador atribuída a uma VM não é suportada atualmente através do Portal do Azure.

   ![Captura de ecrã de página de configuração](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma descrição geral da identidade de serviço geridas, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Através do portal do Azure, atribua o MSI da VM do Azure [acesso a outro recurso do Azure](howto-assign-access-portal.md).

