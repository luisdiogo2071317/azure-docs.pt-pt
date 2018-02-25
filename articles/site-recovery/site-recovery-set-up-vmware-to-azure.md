---
title: Configurar o ambiente de origem (VMware para o Azure) | Microsoft Docs
description: "Este artigo descreve como configurar o seu ambiente no local para iniciar a replicação de máquinas virtuais VMware para o Azure."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: anoopkv
ms.openlocfilehash: 2b6b0e5cc95f28b5596e7e898f5f035e3647d9c5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Configurar o ambiente de origem (VMware para o Azure)
> [!div class="op_single_selector"]
> * [VMware para o Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Físico para o Azure](./site-recovery-set-up-physical-to-azure.md)

Este artigo descreve como configurar a sua origem, o ambiente no local, para replicar máquinas virtuais em execução no VMware para o Azure. Inclui passos para selecionar o seu cenário de replicação, a configurar uma máquina no local como o servidor de configuração da recuperação de sites e detetar automaticamente no local VMs. 

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe que já tem:
- [Configurar recursos](tutorial-prepare-azure.md) no [portal do Azure](http://portal.azure.com).
- [Configurar VMware no local](tutorial-prepare-on-premises-vmware.md), incluindo uma conta dedicada para a deteção automática.



## <a name="choose-your-protection-goals"></a>Escolha os seus objetivos de proteção

1. No portal do Azure, visite o **dos serviços de recuperação** painel do cofre e selecione o cofre.
2. No menu de recurso do cofre, aceda a **introdução** > **recuperação de Site** > **passo 1: preparar a infraestrutura** > **objetivo de proteção**.

    ![Selecione os objetivos](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. No **objetivo de proteção**, selecione **para o Azure**e escolha **Sim, com o VMware vSphere hipervisor**. Em seguida, clique em **OK**.

    ![Selecione os objetivos](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

Configurar o servidor de configuração como uma VM de VMware no local, utilizar um modelo de abrir virtualização formato OVF (). [Saiba mais](concepts-vmware-to-azure-architecture.md) sobre os componentes que serão instalados na VM do VMware. 

1. Saiba mais sobre o [pré-requisitos](how-to-deploy-configuration-server.md#prerequisites) para implementação do servidor de configuração. [Verifique os números de capacidade](how-to-deploy-configuration-server.md#capacity-planning) para implementação.
2. [Transferir](how-to-deploy-configuration-server.md#download-the-template) e [importar](how-to-deploy-configuration-server.md#import-the-template-in-vmware) o modelo OVF (how-to-implementar-configuração-server.md) para configurar uma VM que executa o servidor de configuração de VMware no local.
3. Ative a VM de VMware, e [registá-lo](how-to-deploy-configuration-server.md#register-the-configuration-server) nos serviços de recuperação do cofre.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Adicione a conta de VMware para a deteção automática

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Ligar ao servidor do VMware

Para permitir que o Azure Site Recovery ao detetar máquinas virtuais em execução no seu ambiente no local, terá de ligar o servidor vCenter VMware ou vSphere ESXi anfitriões com a recuperação de Site.

Selecione **+ vCenter** para iniciar a ligação um servidor VMware vCenter ou um anfitrião do VMware vSphere ESXi.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Problemas comuns
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Passos Seguintes
[Configurar o ambiente de destino](./site-recovery-prepare-target-vmware-to-azure.md) no Azure.
