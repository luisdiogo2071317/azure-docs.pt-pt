---
title: Configurar e gerir políticas de replicação para a replicação de VMware com o Azure Site Recovery | Microsoft Docs
description: Descreve como configurar as definições de replicação para replicação de VMware no Azure com o Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: sutalasi
ms.openlocfilehash: 6a8e6e7c6fbdbcbf58557a0896e976a608164041
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
ms.locfileid: "29812671"
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Configurar e gerir políticas de replicação para a replicação de VMware
Este artigo descreve como configurar uma política de replicação quando estiver a replicar VMware VMs para o Azure, utilizando [do Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Criar uma política

1. Selecione **Gerir** > **Infraestrutura do Site Recovery**.
2. No **máquinas para VMware e físico**, selecione **políticas de replicação**. 
3. Clique em **+ política de replicação**e especifique o nome da política.
5. No **limiar RPO**, especifique o limite RPO. Alertas são gerados quando a replicação contínua excede este limite.
6. Na **retenção do ponto de recuperação**, especifique (em horas) a duração da janela de retenção para cada ponto de recuperação. As máquinas protegidas podem ser recuperadas para qualquer ponto numa janela de retenção. É suportada até 24 horas de retenção para máquinas replicadas para o armazenamento premium. Até 72 horas é suportada para o armazenamento standard.
7. Em **Frequência de instantâneos consistentes com a aplicação**, especifique a frequência (em minutos) com que os pontos de recuperação que contêm os instantâneos consistentes com aplicações serão criados.
8. Clique em **OK**. A política deve ser criada em 30 segundos a 60 segundos.

Quando cria uma política de replicação, uma política de replicação de reativação pós-falha correspondente é criada automaticamente, com o sufixo "reativação pós-falha". Depois de criar a política, pode editá-lo, selecionando-> **editar definições de**.

## <a name="associate-a-configuration-server"></a>Associar um servidor de configuração 

Associe a política de replicação com o seu servidor de configuração no local.

1. Clique em **associar**e selecione o servidor de configuração.

    ![Associar o servidor de configuração](./media/vmware-azure-set-up-replication/associate1.png)

2. Clique em **OK**. O servidor de configuração deve ser associado em um ou dois minutos.

    ![Associação do servidor de configuração](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Desassocie ou eliminar uma política de replicação
1. Selecione a política de replicação.
    a. Ao desassociar a política do servidor de configuração, certifique-se de que nenhuma máquina replicada está a utilizar a política. Em seguida, clique em **desassocie**.
    b. Para eliminar a política, certifique-se de que não está associada a um servidor de configuração. Em seguida, clique em **eliminar**. Deve demorar 30-60 segundos, para eliminar.
2. Clique em **OK**.
