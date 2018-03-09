---
title: "Configurar MSI num conjunto utilizando o portal do Azure de dimensionamento de máquina virtual do Azure"
description: "Passo por instruções de passo para configurar uma identidade de serviço geridas (MSI) no Azure VMSS, utilizando o portal do Azure."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 4808165aa760be7e397bd3601e958419780e0004
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/07/2018
---
# <a name="configure-an-azure-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Configurar um Azure Máquina Virtual escala definir geridos serviço de identidade (MSI) no portal do Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a ativar e remover MSI para um conjunto de dimensionamento da máquina virtual do Azure, utilizando o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-azure-virtual-machine-scale-set"></a>Ativar MSI durante a criação de conjunto de dimensionamento da máquina virtual do Azure

A partir do momento desta redação, permitir MSI durante a criação de um conjunto no portal do Azure de dimensionamento de máquina virtual não é suportada. Em vez disso, consulte o máquina virtual do Azure escala conjunto criação início rápido artigo seguinte para primeiro criar um conjunto de dimensionamento da máquina virtual do Azure:

- [Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-create-portal.md)  

Em seguida, avance para a secção seguinte para obter detalhes sobre como ativar MSI no conjunto de dimensionamento de máquina virtual.

## <a name="enable-msi-on-an-existing-azure-vmms"></a>Ativar MSI num VMMS existente do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que foi originalmente aprovisionado sem um MSI:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Clique em de **configuração** página, ative o MSI no dimensionamento da máquina virtual definir selecionando **Sim** com "Identidade de serviço gerida", em seguida, clique em **guardar**. Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Remover MSI a partir de um conjunto de dimensionamento da máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um MSI:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita no conjunto de dimensionamento de máquina virtual.

2. Navegue para o conjunto de dimensionamento de máquina virtual pretendida.

3. Clique no **configuração** página, remova o MSI do dimensionamento da máquina virtual definir selecionando **não** em **identidade do serviço gerido**, em seguida, clique em **guardar** . Esta operação pode demorar 60 segundos ou mais para concluir:

   ![Captura de ecrã de página de configuração](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](msi-overview.md).
- No portal do Azure, forneça um máquina virtual do Azure do conjunto de dimensionamento MSI [acesso a outro recurso do Azure](msi-howto-assign-access-portal.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
