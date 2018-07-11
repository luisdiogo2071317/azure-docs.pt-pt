---
title: Configurar o ambiente de origem para VMware para replicação do Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como configurar o seu ambiente no local para replicar VMs de VMware para o Azure com o Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: 1380c1bc820a815fae317a86fcd0ee4f46dd9aa5
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952659"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Configurar o ambiente de origem para VMware para replicação do Azure

Este artigo descreve como configurar o seu ambiente no local de origem, para replicar VMs de VMware para o Azure. Ele inclui os passos para selecionar o seu cenário de replicação, a configuração de uma máquina no local que o servidor de configuração do Site Recovery, e a detetar automaticamente VMs no local. 

## <a name="prerequisites"></a>Pré-requisitos

O artigo pressupõe que já tem:
- [Configurar recursos](tutorial-prepare-azure.md) no [portal do Azure](http://portal.azure.com).
- [Configurar o VMware no local](vmware-azure-tutorial-prepare-on-premises.md), incluindo uma conta dedicada para a deteção automática.



## <a name="choose-your-protection-goals"></a>Escolha os seus objetivos de proteção

1. No portal do Azure, vá para o **serviços de recuperação** painel do cofre e selecione o cofre.
2. No menu de recursos do cofre, aceda a **introdução** > **Site Recovery** > **passo 1: preparar a infraestrutura**  >  **Objetivo de proteção**.

    ![Selecione os objetivos](./media/vmware-azure-set-up-source/choose-goals.png)
3. Na **objetivo de proteção**, selecione **para o Azure**e escolha **Sim, com o VMware vSphere Hypervisor**. Em seguida, clique em **OK**.

    ![Selecione os objetivos](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

Pode configurar o servidor de configuração como um VM de VMware no local através de um modelo de aplicativo de Virtualização aberto (OVA). [Saiba mais](concepts-vmware-to-azure-architecture.md) sobre os componentes que serão instalados na VM do VMware.

1. Saiba mais sobre o [pré-requisitos](vmware-azure-deploy-configuration-server.md#prerequisites) para implementação de servidor de configuração.
2. [Verificar números de capacidade](vmware-azure-deploy-configuration-server.md#capacity-planning) para implementação.
3. [Baixe](vmware-azure-deploy-configuration-server.md#download-the-template) e [importar](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) o modelo de OVA para configurar um VM que executa o servidor de configuração de VMware no local. A licença fornecida com o modelo é uma licença de avaliação e é válida por 180 dias. Publicar este período, cliente tem de ativar o windows com uma licença procured.
4. Ative a VM de VMware, e [registá-lo](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) no cofre dos serviços de recuperação.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Adicione a conta de VMware para a deteção automática

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Ligar ao servidor VMware

Para permitir que o Azure Site Recovery detetar máquinas virtuais em execução no seu ambiente no local, terá de ligar o seu servidor VMware vCenter ou anfitriões ESXi do vSphere com o Site Recovery.

Selecione **+ vCenter** para começar a ligar um servidor VMware vCenter ou um anfitrião do VMware vSphere ESXi.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Problemas comuns
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Passos Seguintes
[Configurar o ambiente de destino](./vmware-azure-set-up-target.md) no Azure.
