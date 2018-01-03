---
title: "Preparar o servidor de Hyper-V no local para a recuperação após desastre de VMs de Hyper-V para o Azure | Microsoft Docs"
description: "Saiba como preparar as VMs de Hyper-V no local não são geridos pelo System Center VMM para a recuperação de desastre para o Azure com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6e2837341e16f5077cfff18d4a34c097c165ef09
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Preparar os servidores de Hyper-V no local para a recuperação de desastre para o Azure

Este tutorial mostra como preparar a infraestrutura de Hyper-V no local quando pretende replicar VMs Hyper-V para o Azure, para fins de recuperação após desastre. Anfitriões Hyper-V podem ser geridos pelo System Center Virtual Machine Manager (VMM), mas não é necessário.  Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Reveja os requisitos do Hyper-V e requisitos de VMM, se aplicável.
> * Preparar a VMM, se aplicável
> * Verificar o acesso à internet para localizações do Azure
> * Preparar as VMs para que possa aceder aos mesmos após a ativação pós-falha para o Azure

Este é o segundo tutorial da série. Certifique-se de que tem [configurar os componentes do Azure](tutorial-prepare-azure.md) conforme descrito no tutorial anterior.



## <a name="review-server-requirements"></a>Reveja os requisitos de servidor

Certifique-se de anfitriões Hyper-V cumpram os seguintes requisitos. Se estiver a gerir os anfitriões em nuvens do System Center Virtual Machine Manager (VMM), certifique-se os requisitos do VMM.


**Componente** | **Hyper-V gerida pelo VMM** | **Hyper-V sem o VMM**
--- | --- | ---
**Sistema operativo anfitrião de Hyper-V** | 2016, do Windows Server 2012 R2 | ND
**VMM** | VMM 2012, O VMM 2012 R2 | ND


## <a name="review-hyper-v-vm-requirements"></a>Reveja os requisitos de VM de Hyper-V

Certifique-se de que a VM está em conformidade com os requisitos de resumidos na tabela.

**Requisito de VM** | **Detalhes**
--- | ---
**Sistema operativo convidado** | Qualquer SO convidado [suportado pelo Azure](https://technet.microsoft.com/library/cc794868.aspx).
**Requisitos do Azure** | No local VMs de Hyper-V têm de cumprir requirements(site-recovery-support-matrix-to-azure.md) de VM do Azure.

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

1. Para efeitos do tutorial, é a configuração mais simples para os anfitriões Hyper-V e o servidor VMM, se aplicável, para ter acesso direto à internet sem utilizar um proxy. 
2. Certifique-se de que anfitriões Hyper-V e o servidor VMM, se forem relevantes, podem aceder a estes URLs: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Certifique-se de que:
    - Quaisquer regras de firewall baseadas no endereço IP devem permitir a comunicação com o Azure.
    - Permita os [Intervalos de IP do Datacenter do Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) e a porta HTTPS (443).
    - Permita intervalos de endereços IP para a região do Azure da sua subscrição e para EUA oeste (utilizado para gestão de identidade e controlo de acesso).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Durante um cenário de ativação pós-falha poderá ligar à sua rede replicada no local.

Para ligar a VMs do Windows com RDP após a ativação pós-falha, efetue o seguinte:

1. Para aceder através da internet, ative o RDP na VM no local antes da ativação pós-falha. Certifique-se de que TCP e regras UDP estão adicionadas para o **pública** perfil e que o RDP é permitido na **Firewall do Windows** > **aplicações permitidas** para todos os perfis.
2. Para aceder através de VPN de site a site, ative o RDP na máquina no local. RDP deve ser permitido no **Firewall do Windows** -> **aplicações e funcionalidades permitidas** para **domínio e privada** redes.
   Verifique se a política de SAN do sistema operativo estiver definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135). Não deverá haver nenhum atualizações pendentes do Windows na VM quando acione uma ativação pós-falha. Se existirem, não poderá iniciar sessão para a máquina virtual enquanto a atualização for concluída.
3. No Windows VM do Azure após a ativação pós-falha, verifique **diagnóstico de arranque** para ver uma captura de ecrã da VM. Se não conseguir ligar, verifique se a VM está em execução e reveja estes [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre para o Azure para as VMs de Hyper-V](tutorial-hyper-v-to-azure.md)
> [configurar a recuperação após desastre para o Azure para as VMs de Hyper-V em nuvens VMM](tutorial-hyper-v-vmm-to-azure.md)
