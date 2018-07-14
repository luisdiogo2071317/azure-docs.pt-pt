---
title: Como atribuir acesso MSI a um recurso do Azure, com o portal do Azure
description: Instruções passo a passo para atribuir um MSI num acesso a recursos para outro recurso, com o portal do Azure.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: cf6df6dd87de56ca3c8180d5377e35d9be9bbd79
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036450"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Atribuir um acesso de identidade do serviço gerido a um recurso com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Depois de configurar um recurso do Azure com uma identidade de serviço gerida (MSI), pode dar o acesso MSI para outro recurso, tal como qualquer entidade de segurança. Este artigo mostra-lhe como conceder a uma máquina virtual do Azure ou o acesso MSI do conjunto de dimensionamento de máquina virtual para uma conta de armazenamento do Azure, com o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI para outro recurso

Depois de ativar a MSI num recurso do Azure, tal como um [VM do Azure](qs-configure-portal-windows-vm.md) ou [VMSS do Azure](qs-configure-portal-windows-vmss.md):

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure na qual configurou o MSI.

2. Navegue para o recurso pretendido no qual pretende modificar o controlo de acesso. Neste exemplo, estamos oferecendo uma máquina virtual do Azure e o dimensionamento de máquina virtual do Azure definir acesso a uma conta de armazenamento, para que, navegar para a conta de armazenamento.

3. Para uma máquina virtual do Azure, selecione o **controlo de acesso (IAM)** página do recurso e selecione **+ adicionar**. Em seguida, especifique a **função**, **atribuir acesso a Máquina Virtual**e especifique o correspondente **subscrição** e **grupo de recursos** onde reside o recurso. Na área de critérios de pesquisa, deverá ver o recurso. Selecione o recurso e selecione **guardar**. 

   ![Captura de ecrã de controlo (IAM) de acesso](../managed-service-identity/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Para obter um conjunto de dimensionamento de máquina virtual do Azure, selecione o **controlo de acesso (IAM)** página do recurso e selecione **+ adicionar**. Em seguida, especifique a **função**, **atribuir acesso a**. Na área de critérios de pesquisa, procure o conjunto de dimensionamento de máquina virtual. Selecione o recurso e selecione **guardar**.
   
   ![Captura de ecrã de controlo (IAM) de acesso](../managed-service-identity/media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. É reencaminhado para o principal **controlo de acesso (IAM)** página, onde verá uma nova entrada para MSI o recurso.

    Máquina virtual do Azure:

   ![Captura de ecrã de controlo (IAM) de acesso](../managed-service-identity/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Conjunto de dimensionamento de máquina virtual do Azure:

    ![Captura de ecrã de controlo (IAM) de acesso](../managed-service-identity/media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Resolução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, certifique-se de que o MSI foi ativado corretamente. No nosso caso, podemos voltar para a máquina virtual do Azure e verifique o seguinte:

- Examinar os **Configuration** página e certifique-se de que o valor de **MSI ativada** é **Sim**.
- Examinar os **extensões** página e certifique-se de que a extensão MSI implementada com êxito (**extensões** página não está disponível para um conjunto de dimensionamento de máquina virtual do Azure).

Se uma for incorreta, poderá ter de voltar a implementar o MSI no seu recurso, ou a falha de implementação de resolução de problemas.

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](overview.md).
- Para ativar o MSI numa máquina virtual do Azure, consulte [configurar uma Azure VM Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vm.md).
- Para ativar o MSI num conjunto de dimensionamento de máquina virtual do Azure, consulte [configurar um Azure Máquina Virtual dimensionamento definir Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vmss.md)


