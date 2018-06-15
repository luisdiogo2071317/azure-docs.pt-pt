---
title: Configurar MSI num conjunto utilizando o portal do Azure de dimensionamento de máquina virtual do Azure
description: Passo por instruções de passo para configurar uma identidade de serviço geridas (MSI) no Azure VMSS, utilizando o portal do Azure.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: c915c692a12781538e10d367d40e3efe473a6853
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33929048"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-the-azure-portal"></a>Configurar um VMSS geridos serviço de identidade (MSI) no portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender como ativar e desativar o sistema atribuído de identidade para um VMSS, utilizando o portal do Azure. Atribuir e remover utilizador atribuído identidades de um VMSS do Azure não é atualmente suportado através do Portal do Azure.

> [!NOTE]
> Atualmente, o utilizador atribuído operações de identidade não são suportados através do Portal do Azure. Verifique novamente para as atualizações.

## <a name="prerequisites"></a>Pré-requisitos


- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md).
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Identidade de serviço geridas durante a criação de um conjunto de dimensionamento da máquina virtual do Azure

Atualmente, a criação de VM através do portal do Azure não suporta operações de identidade de serviço geridas. Em vez disso, consulte o máquina virtual do Azure escala conjunto criação início rápido artigo seguinte para primeiro criar um conjunto de dimensionamento da máquina virtual do Azure:

- [Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure](../../virtual-machine-scale-sets/quick-create-portal.md)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar MSI no conjunto de dimensionamento de máquina virtual.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Ativar a identidade de serviço geridas num VMMS existente do Azure

Para ativar o sistema atribuído a identidade de uma VM que foi originalmente aprovisionada sem-lo:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Ativar a identidade do sistema atribuído na VM ao selecionar "Sim" em "Identidade de serviço gerida" e, em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   [![Captura de ecrã de página de configuração](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Remover a identidade de serviço geridas de um conjunto de dimensionamento da máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um MSI:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita no conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Desative o sistema atribuído de identidade da VM, selecionando "Não" em "Identidade de serviço gerida", em seguida, clique em Guardar. Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](../media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma descrição geral da identidade de serviço geridas, consulte [descrição geral](overview.md).

## <a name="next-steps"></a>Passos Seguintes

- No portal do Azure, forneça um máquina virtual do Azure do conjunto de dimensionamento MSI [acesso a outro recurso do Azure](howto-assign-access-portal.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
