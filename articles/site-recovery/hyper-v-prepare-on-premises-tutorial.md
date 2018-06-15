---
title: Preparar o servidor de Hyper-V no local para a recuperação após desastre de VMs de Hyper-V para o Azure | Microsoft Docs
description: Saiba como preparar as VMs de Hyper-V no local não são geridos pelo System Center VMM para a recuperação de desastre para o Azure com o serviço do Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7e0219a662483ef123bdc2889a43dd3d93d23ac2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31413233"
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Preparar os servidores de Hyper-V no local para a recuperação de desastre para o Azure

Este tutorial mostra como preparar a infraestrutura de Hyper-V no local quando pretende replicar VMs Hyper-V para o Azure, para fins de recuperação após desastre. Anfitriões Hyper-V podem ser geridos pelo System Center Virtual Machine Manager (VMM), mas não é necessário.  Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Reveja os requisitos do Hyper-V e requisitos de VMM, se aplicável.
> * Preparar a VMM, se aplicável
> * Verificar o acesso à internet para localizações do Azure
> * Preparar as VMs para que possa aceder aos mesmos após a ativação pós-falha para o Azure

Este é o segundo tutorial da série. Certifique-se de que [configurou os componentes do Azure](tutorial-prepare-azure.md) conforme descrito no tutorial anterior.



## <a name="review-requirements-and-prerequisites"></a>Rever requisitos e pré-requisitos

Certifique-se de anfitriões de Hyper-V e VMs cumprir os requisitos.

1. [Certifique-se](hyper-v-azure-support-matrix.md#on-premises-servers) requisitos de servidor no local.
2. [Verifique os requisitos](hyper-v-azure-support-matrix.md#replicated-vms) para as VMs de Hyper-V que pretende replicar para o Azure.
3. Verifique o anfitrião Hyper-V [rede](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); e o anfitrião e o convidado [armazenamento](hyper-v-azure-support-matrix.md#hyper-v-host-storage) para anfitriões de Hyper-V no local.
4. Verifique o que é suportado para [redes do Azure](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [armazenamento](hyper-v-azure-support-matrix.md#azure-storage), e [computação](hyper-v-azure-support-matrix.md#azure-compute-features), após a ativação pós-falha.
5. As suas VM no local que replica para o Azure devem estar em conformidade com os [requisitos de VM do Azure](hyper-v-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-vmm-optional"></a>Preparar o VMM (opcional)

Se os anfitriões de Hyper-V são geridos pelo VMM, terá de preparar o servidor do VMM no local. 

- Certifique-se de que o servidor VMM tem, pelo menos, uma nuvem, com um ou mais grupos de anfitriões. O anfitrião de Hyper-V no qual as VMs estão em execução deve estar localizado na nuvem.
- Prepare o servidor VMM para o mapeamento da rede.

### <a name="prepare-vmm-for-network-mapping"></a>Preparar o VMM para o mapeamento da rede

Se estiver a utilizar o VMM, [mapeamento da rede](site-recovery-network-mapping.md) mapeia entre as redes VM do VMM no local e redes virtuais do Azure. Mapeamento assegura que as VMs do Azure estão ligadas à rede à direita quando são criados após a ativação pós-falha.

Prepare VMM mapeamento da rede da seguinte forma:

1. Certifique-se de que tem um [rede lógica de VMM](https://docs.microsoft.com/system-center/vmm/network-logical) que tiver associado à nuvem em que estão localizados os anfitriões de Hyper-V.
2. Certifique-se de que tem um [rede VM](https://docs.microsoft.com/system-center/vmm/network-virtual) ligado à rede lógica.
3. No VMM, ligar as VMs para a rede VM.

## <a name="verify-internet-access"></a>Verificar o acesso à internet

1. Para efeitos do tutorial, é a configuração mais simples para o servidor VMM e anfitriões Hyper-V tenham acesso direto à internet sem utilizar um proxy. 
2. Certifique-se de que anfitriões Hyper-V e o servidor VMM, se forem relevantes, podem aceder a estes URLs: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Se está a controlar o acesso por endereço IP, certifique-se de que:
    - Regras de firewall baseadas no endereço IP podem ligar ao [intervalos de IP do Datacenter do Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)e a porta HTTPS (443).
    - Permita intervalos de endereços IP para a região do Azure da sua subscrição e para EUA oeste (utilizado para gestão de identidade e controlo de acesso).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Durante um cenário de ativação pós-falha poderá ligar à sua rede replicada no local.

Para ligar a VMs do Windows com RDP após a ativação pós-falha, permitir o acesso da seguinte forma:

1. Para aceder através da Internet, ative o protocolo RDP na VM local antes da ativação pós-falha. Certifique-se de que são adicionadas regras de TCP e UDP para o perfil **Público** e que o protocolo RDP é permitido em **Firewall do Windows** > **Aplicações Permitidas** para todos os perfis.
2. Para aceder através da rede de VPNs, ative o protocolo RDP na máquina local. O protocolo RDP deve estar permitido em **Firewall do Windows** -> **Aplicações e funcionalidades permitidas** para redes de **Domínio e Privadas**.
   Verifique se a política de SAN do sistema operativo está definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135). Não devem haver atualizações do Windows pendentes na VM quando aciona uma ativação pós-falha. Se houverem, só poderá iniciar sessão na máquina virtual depois de a atualização estar concluída.
3. Na VM do Azure do Windows após a ativação pós-falha, consulte o **Diagnóstico de arranque**  para ver uma captura de ecrã da VM. Se não conseguir ligar-se, verifique se a VM está em execução e reveja estas [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Após a ativação pós-falha, pode aceder a VMs do Azure utilizando o mesmo endereço IP que a VM replicada no local ou um endereço IP diferente. [Saiba mais](concepts-on-premises-to-azure-networking.md) sobre como configurar endereços IP de ativação pós-falha.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre para o Azure para as VMs de Hyper-V](tutorial-hyper-v-to-azure.md)
> [configurar a recuperação após desastre para o Azure para as VMs de Hyper-V em nuvens VMM](tutorial-hyper-v-vmm-to-azure.md)
