---
title: Como configurar o MSI na VM do Azure com o portal do Azure
description: Passo a passo de instruções para configurar uma identidade de serviço gerida (MSI) na VM do Azure, com o portal do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 54bb2466a99569c124128ad3e08f276a0bc0262d
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035270"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Configurar uma VM Managed Service Identity (MSI) com o portal do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a ativar e remover o MSI para uma VM do Azure, com o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Ativar o MSI durante a criação de uma VM do Azure

No momento da redação deste artigo, ativar o MSI durante a criação de uma VM no portal do Azure não é suportada. Em vez disso, veja um dos seguintes artigos de início rápido de criação de VM para primeiro criar uma VM:

- [Criar uma máquina virtual do Windows com o portal do Azure](~/articles/virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Criar uma máquina virtual Linux com o portal do Azure](~/articles/virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar o MSI na VM.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Ativar o MSI numa VM do Azure existente

Se tiver uma VM que foi originalmente aprovisionada sem um MSI:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual".

2. Navegue para a Máquina Virtual pretendida.

2. Clique na página "Configuração", ative o MSI na VM ao selecionar "Sim" em "Identidade de serviço gerida", em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Remover MSI a partir de uma VM do Azure

Se tiver uma Máquina Virtual que já não precisa de um MSI:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual".

2. Navegue para a Máquina Virtual pretendida.

3. Clique na página "Configuração", remova o MSI da VM ao selecionar "Não" em "Identidade de serviço gerida", em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Utilizar o portal do Azure, a dar o MSI da VM do Azure [acesso a outro recurso do Azure](msi-howto-assign-access-portal.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
