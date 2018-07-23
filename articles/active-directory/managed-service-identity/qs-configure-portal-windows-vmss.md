---
title: Configurar MSI num conjunto com o portal do Azure de dimensionamento de máquina virtual do Azure
description: Passo a passo de instruções para configurar uma identidade de serviço gerida (MSI) em VMSS do Azure, com o portal do Azure.
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
ms.openlocfilehash: 34911c6133a6a2549a98db426f979b0c3c85dbd7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185302"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Configurar uma máquina virtual Managed Service Identity (MSI) com o portal do Azure do conjunto de dimensionamento

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a ativar e desativar o sistema de identidade para um conjunto de dimensionamento de máquina virtual atribuído com o portal do Azure. Atribuir e remover identidades atribuídas por utilizadores de um conjunto de dimensionamento de máquina virtual do Azure não é suportado atualmente pelo portal do Azure.

> [!NOTE]
> Atualmente, o utilizador atribuído a operações de identidade não são suportadas no portal do Azure. Volte mais tarde para obter atualizações.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem da atribuição de função seguinte:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para ativar e remover a identidade do serviço gerido de um conjunto de dimensionamento de máquina virtual.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Identidade de serviço gerida durante a criação de um conjunto de dimensionamento de máquina virtual do Azure

Atualmente, a criação de VM através do portal do Azure não suporta operações de identidade do serviço gerido. Em vez disso, veja o máquina virtual do Azure dimensionamento conjunto criação manual de início rápido artigo seguinte para primeiro criar um conjunto de dimensionamento de máquina virtual do Azure:

- [Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure](../../virtual-machine-scale-sets/quick-create-portal.md)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar o MSI no conjunto de dimensionamento de máquina virtual.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Ativar a identidade de serviço gerido num VMMS existente do Azure

Para ativar o sistema de identidade numa VM que foi originalmente aprovisionada sem ele atribuída:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Ativar a identidade de sistema atribuído na VM ao selecionar "Sim" em "Identidade de serviço gerida" e, em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   [![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Remover a identidade do serviço gerido de um conjunto de dimensionamento de máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um MSI:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita no conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Desativar o sistema de identidade na VM de atribuída ao selecionar "Não" em "Identidade de serviço gerida" e, em seguida, clique em Guardar. Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral de identidade do serviço gerido, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- Com o portal do Azure, forneça um máquina virtual do Azure do conjunto de dimensionamento MSI [acesso a outro recurso do Azure](howto-assign-access-portal.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
