---
title: Configurar contas do Azure para acoustics - serviços cognitivos
description: Siga este guia para configurar contas do Azure Batch e de armazenamento necessário para trabalhar com acoustics.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181642"
---
# <a name="create-an-azure-batch-account"></a>Criar uma conta do Azure Batch
Siga este guia para configurar contas do Azure Batch e de armazenamento necessário para trabalhar com acoustics. Para obter informações sobre o plug-in de Unity desenvolvido como parte do projeto Acoustics, consulte [o que é acoustics](what-is-acoustics.md). Para obter informações sobre como incorporar acoustics em seu projeto Unity, consulte [introdução ao](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Obter uma subscrição do Azure
Uma [subscrição do Azure](https://azure.microsoft.com/free/) é necessária antes de configurar contas do Batch e armazenamento. Se estiver a iniciar cópia de segurança pela primeira vez, o Azure fornece alguns recursos gratuitos de tempo limitado e crédito de $200.

## <a name="create-azure-batch-and-storage-accounts"></a>Criar contas do Azure Batch e armazenamento
Em seguida, siga [estas instruções](https://docs.microsoft.com/azure/batch/batch-account-create-portal) para configurar o Azure Batch e associados a contas de armazenamento do Azure.

Escolha as opções predefinidas para contas do Batch e armazenamento:
  
  ![Nova conta do Batch](media/NewBatchAccountCreate.png)

  ![Nova conta de armazenamento](media/BatchStorageAccountCreate.png)

Demora alguns minutos para o Azure implementar as contas. Procure uma notificação de conclusão, no canto superior direito no portal.
  
  ![Contas implementadas](media/BatchAccountsDeployNotification.png)

As suas contas devem agora estar visíveis no seu dashboard.
  
  ![Dashboard do portal](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Configurar acoustics criar da interface do Usuário com credenciais do Azure
Clique na ligação de conta do Batch no dashboard, em seguida, clique nas **chaves** link na página de conta de Batch para acessar as suas credenciais.
  
  ![Ligação de chaves do batch](media/BatchAccessKeys.png)

  ![Credenciais de conta do batch](media/BatchKeysInfo.png)

Clique nas **conta de armazenamento** link na página para acessar as credenciais da conta de armazenamento do Azure.
  
  ![Credenciais da Conta de Armazenamento](media/StorageKeysInfo.png)

Introduza estas credenciais no separador de criar, conforme descrito no [inserir passo a passo da interface do Usuário](bake-ui-walkthrough.md).

## <a name="node-types-and-region-support"></a>Tipos de nós e suporte de região
Projeto Acoustics requer a que série F e H de computação com otimização de nós de VM do Azure que podem não ser suportados em todas as regiões do Azure. Verifique se existem [esta tabela](https://azure.microsoft.com/global-infrastructure/services) para garantir que está a escolher a localização certa para a sua conta do Batch. Com esta série de momento em que H máquinas virtuais são suportadas no E.U.A. leste, e.u.a. Centro-Norte, Centro-Sul, E.U.A. oeste, E.U.A. oeste 2, Europa do Norte, Europa Ocidental e leste do Japão.

## <a name="upgrading-your-quota"></a>Atualizar a sua quota
Contas de Batch do Azure aprovisionadas na conta de criação com um limite de 20 núcleos de computação. Pode querer aumentar este limite de tempo de criar mais rápido, uma vez que consegue paralelizar tão sua carga de trabalho acoustics por muitos nós, até ao número de pontos de sonda em seu cenário. Pode pedir um aumento de quota ao clicar no **Quota** ligar na sua página de portal do Azure Batch e, em seguida, clicar em **aumentar a Quota de pedido**:

![Aumento de Quota do Azure](media/azurequotas.png)

## <a name="next-steps"></a>Passos Seguintes
* Introdução ao [integração acoustics ao seu projeto Unity](getting-started.md)
* Explorar o [cena de exemplo](sample-walkthrough.md)

