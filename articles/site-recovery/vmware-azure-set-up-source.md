---
title: "Configurar o ambiente de origem para VMware para a replicação do Azure com o Azure Site Recovery | Microsoft Docs"
description: Este artigo descreve como configurar o seu ambiente no local para replicar VMware VMs para o Azure com o Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: b2c564e8d49e39d9cdc09d3fe168388d579de70e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Configurar o ambiente de origem para VMware para a replicação do Azure

Este artigo descreve como configurar o seu ambiente no local de origem, para replicar VMware VMs para Azure. Inclui passos para selecionar o seu cenário de replicação, a configurar uma máquina no local como o servidor de configuração da recuperação de sites e detetar automaticamente no local VMs. 

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe que já tem:
- [Configurar recursos](tutorial-prepare-azure.md) no [portal do Azure](http://portal.azure.com).
- [Configurar VMware no local](vmware-azure-tutorial-prepare-on-premises.md), incluindo uma conta dedicada para a deteção automática.



## <a name="choose-your-protection-goals"></a>Escolha os seus objetivos de proteção

1. No portal do Azure, visite o **dos serviços de recuperação** painel do cofre e selecione o cofre.
2. No menu de recurso do cofre, aceda a **introdução** > **recuperação de Site** > **passo 1: preparar a infraestrutura** > **objetivo de proteção**.

    ![Selecione os objetivos](./media/vmware-azure-set-up-source/choose-goals.png)
3. No **objetivo de proteção**, selecione **para o Azure**e escolha **Sim, com o VMware vSphere hipervisor**. Em seguida, clique em **OK**.

    ![Selecione os objetivos](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

Configurar o servidor de configuração como uma VM de VMware no local, utilizar um modelo de abrir virtualização formato OVF (). [Saiba mais](concepts-vmware-to-azure-architecture.md) sobre os componentes que serão instalados na VM do VMware. 

1. Saiba mais sobre o [pré-requisitos](vmware-azure-deploy-configuration-server.md#prerequisites) para implementação do servidor de configuração.
2. [Verifique os números de capacidade](vmware-azure-deploy-configuration-server.md#capacity-planning) para implementação.
3. [Transferir](vmware-azure-deploy-configuration-server.md#download-the-template) e [importar](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) o modelo OVF (how-to-implementar-configuração-server.md) para configurar uma VM que executa o servidor de configuração de VMware no local.
4. Ative a VM de VMware, e [registá-lo](vmware-azure-deploy-configuration-server.md#register-the-configuration-server) nos serviços de recuperação do cofre.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Adicione a conta de VMware para a deteção automática

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Ligar ao servidor do VMware

Para permitir que o Azure Site Recovery ao detetar máquinas virtuais em execução no seu ambiente no local, terá de ligar o servidor vCenter VMware ou vSphere ESXi anfitriões com a recuperação de Site.

Selecione **+ vCenter** para iniciar a ligação um servidor VMware vCenter ou um anfitrião do VMware vSphere ESXi.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Problemas comuns
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Passos Seguintes
[Configurar o ambiente de destino](./vmware-azure-set-up-target.md) no Azure.
