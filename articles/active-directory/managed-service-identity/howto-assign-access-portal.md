---
title: Como atribuir acesso MSI para um recurso do Azure, utilizando o portal do Azure
description: Instruções passo a passo para atribuir um MSI num acesso a recursos a outro recurso, utilizando o portal do Azure.
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
ms.openlocfilehash: 06f316a7c96ff266e9f4593fa3a9ac871b2979aa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33929777"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Atribuir acesso uma identidade de serviço geridas para um recurso com o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Depois de configurar um recurso do Azure com uma identidade de serviço geridas (MSI), é possível conceder o acesso do MSI para outro recurso, tal como qualquer principal de segurança. Este artigo mostra como conceder acesso a MSI do conjunto de dimensionamento da máquina virtual ou de uma máquina virtual do Azure para uma conta de armazenamento do Azure, utilizando o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI a outro recurso

Depois de ativar a MSI um recurso do Azure, tal como um [VM do Azure](qs-configure-portal-windows-vm.md) ou [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) utilizando uma conta associada à subscrição do Azure na qual configurou o MSI.

2. Navegue para o recurso pretendido no qual pretende modificar o controlo de acesso. Neste exemplo, vamos dá a uma máquina virtual do Azure e dimensionamento de máquina virtual do Azure definir acesso a uma conta de armazenamento, pelo que iremos navegue para a conta de armazenamento.

3. Para uma máquina virtual do Azure, selecione o **(IAM) do controlo de acesso** página do recurso e selecione **+ adicionar**. Em seguida, especifique o **função**, **atribuir acesso a Máquina Virtual**e especifique correspondente **subscrição** e **grupo de recursos** onde reside o recurso. Na área de critérios de pesquisa, deverá ver o recurso. Selecione o recurso e selecione **guardar**. 

   ![Captura de ecrã de (páginas IAM) do controlo de acesso](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Para um conjunto de dimensionamento da máquina virtual do Azure, selecione o **(IAM) do controlo de acesso** página do recurso e selecione **+ adicionar**. Em seguida, especifique o **função**, **atribuir acesso**. Na área de critérios de pesquisa, procure o seu conjunto de dimensionamento da máquina virtual. Selecione o recurso e selecione **guardar**.
   
   ![Captura de ecrã de (páginas IAM) do controlo de acesso](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. É encaminhado para o principal **(IAM) do controlo de acesso** página, em que verá uma nova entrada para MSI o recurso.

    Máquina virtual do Azure:

   ![Captura de ecrã de (páginas IAM) do controlo de acesso](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Conjunto de dimensionamento da máquina virtual do Azure:

    ![Captura de ecrã de (páginas IAM) do controlo de acesso](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Resolução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, certifique-se de que o MSI foi ativado corretamente. No nosso caso, iremos pode regressar à máquina virtual do Azure e verifique o seguinte:

- Observe o **configuração** página e certifique-se de que o valor para **MSI ativada** é **Sim**.
- Observe o **extensões** página e certifique-se de que a extensão MSI implementada com êxito (**extensões** página não está disponível para um conjunto de dimensionamento da máquina virtual do Azure).

Se o estiver incorreto, poderá ter de voltar a implementar o MSI no seu recurso ou resolver a falha de implementação.

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](overview.md).
- Para ativar o MSI numa máquina virtual do Azure, consulte [configurar um Azure VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md).
- Para ativar o MSI um conjunto de dimensionamento da máquina virtual do Azure, consulte o artigo [configurar um Azure Máquina Virtual escala definir geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vmss.md)


