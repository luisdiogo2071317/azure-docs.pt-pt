---
title: Matriz de suporte do Azure Site Recovery para replicar a partir do Azure para o Azure | Documentos da Microsoft
description: Resume os sistemas operativos suportados e configurações para a replicação do Azure Site Recovery de máquinas virtuais do Azure (VMs) de uma região para outro para as necessidades de (DR recuperação) após desastre.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: sujayt
ms.openlocfilehash: 5cf795b85e295488a04e95dbabcee46134361495
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222477"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Matriz de suporte para replicar a partir de uma região do Azure para outra



Este artigo resume as configurações suportadas e componentes ao replicar e recuperar máquinas virtuais do Azure a partir de uma região para outra região, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.

## <a name="user-interface-options"></a>Opções de interface de utilizador

**Interface do usuário** |  **Suportado / não suportado**
--- | ---
**Portal do Azure** | Suportadas
**PowerShell** | [Replicação do Azure para o Azure com o PowerShell](azure-to-azure-powershell.md)
**API REST** | Não são atualmente suportados
**CLI** | Não são atualmente suportados


## <a name="resource-support"></a>Suporte de recursos

**Tipo de movimentação do recurso** | **Detalhes**
--- | --- | ---
**Mover o Cofre entre grupos de recursos** | Não suportado<br/><br/> Não é possível mover um cofre dos serviços de recuperação entre grupos de recursos.
**Mover os recursos de armazenamento/computação/rede entre grupos de recursos** | Não suportado.<br/><br/> Se mover uma VM ou componentes associados, como o armazenamento/rede depois que está a replicar, terá de desativar a replicação e reativar a replicação para a VM.
**Replicar VMs do Azure a partir de uma subscrição para outro para recuperação após desastre** | Suportado no mesmo inquilino do Azure Active Directory. Não é suportada para VMs clássicas.
**Migrar VMs entre regiões dentro os clusters geográficos suportados (dentro e entre subscrições)** | Suportado no mesmo inquilino do Azure Active Directory para VMs de "Modelo de implementação do Resource manager". Não é suportada para VMs de "Modelo de implementação clássico".
**Migrar VMs na mesma região** | Não suportado.


## <a name="support-for-replicated-machine-os-versions"></a>Suporte para versões de SO da máquina replicada

O abaixo o suporte se aplica a qualquer carga de trabalho em execução no sistema operacional mencionado.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Server Core, servidor com experiência de área de trabalho) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 com no SP1, pelo menos,

>[!NOTE]
>
> \* Windows Server 2016 Nano Server não é suportado.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3 7.4, 7.5   
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1 e 7.2, 7.3,7.4, 7.5
- Ubuntu 14.04 LTS Server [ (versões de kernel de suportadas)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (versões de kernel de suportadas)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7 [ (versões de kernel de suportadas)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Debian 8 [ (versões de kernel de suportadas)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 12 SP1, SP2, SP3 [ (versões de kernel de suportadas)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 11 SP3
- SP4 do SUSE Linux Enterprise Server 11
- Oracle Enterprise Linux 6.4, 6.5, 6.6, 6.7 com o kernel compatível do Red Hat ou Indestrutíveis Enterprise Kernel versão 3 (UEK3)

(Atualização de replicar as máquinas de SLES 11 SP3 para SLES 11 SP4 não é suportada. Se uma máquina replicada foi atualizada de SLES 11SP3 para SP4 de 11 de SLES, terá de desativar a replicação e proteger a máquina novamente após a atualização.)

>[!NOTE]
>
> Ubuntu servidores usando a autenticação baseada em palavra-passe e o início de sessão e utilizar o pacote de cloud-init para configurar máquinas de virtuais de cloud, podem ter início de sessão baseada em palavra-passe desabilitado aquando da ativação pós-falha (dependendo da configuração de cloudinit.) Início de sessão baseada em palavra-passe pode ser novamente ativado na máquina virtual ao repor a palavra-passe no menu definições (sob o suporte + resolução de problemas de secção) da máquina virtual no portal do Azure com ativação pós-falha.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Versões suportadas do kernel de Ubuntu para máquinas virtuais do Azure

**Versão** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
14.04 LTS | 9.19 | 3.13.0-24-Generic para 3.13.0-153-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-131-generic |
14.04 LTS | 9.18 | 3.13.0-24-Generic para 3.13.0-151-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-128-generic |
14.04 LTS | 9.17 | 3.13.0-24-Generic para 3.13.0-147-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-124-generic |
14.04 LTS | 9.16 | 3.13.0-24-Generic para 3.13.0-144-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-119-generic |
|||
16.04 LTS | 9.19 | 4.4.0-21-Generic para 4.4.0-131-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-45-generic,<br/>4.15.0-13-Generic para 4.15.0-30-generic<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1018-azure <br/>4.15.0-1012-Azure para 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-Generic para 4.4.0-128-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-45-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1018-azure |
16.04 LTS | 9.17 | 4.4.0-21-Generic para 4.4.0-124-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-41-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1016-azure |
16.04 LTS | 9.16 | 4.4.0-21-Generic para 4.4.0-119-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-38-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1012-azure |


### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Versões do Debian kernel com suporte para máquinas virtuais do Azure

**Versão** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
Debian 7 | 9.17,9.18,9.19 | 3.2.0-4-AMD64 para 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.16 | 3.2.0-4-AMD64 para 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.19 | 3.16.0-4-AMD64 para 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.17, 9.18 | 3.16.0-4-AMD64 para 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-AMD64 para 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.5-amd64 |

### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Versões suportadas do kernel de 12 do SUSE Linux Enterprise Server para máquinas virtuais do Azure

**Versão** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.18 | SP1 3.12.49-11-default para 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default para 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default para 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default para 4.4.121-92.80-default</br></br>4.4.73-5-default SP3 para 4.4.138-94.39-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.17 | SP1 3.12.49-11-default para 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default para 3.12.74-60.64.88-default</br></br> SP2 4.4.21-69-default para 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>4.4.73-5-default SP3 para 4.4.126-94.22-default |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Sistemas de ficheiro suportados e configurações de armazenamento de convidado em máquinas virtuais do Azure, executar o SO Linux

* Sistemas de ficheiros: ext3 ext4, ReiserFS (Suse Linux Enterprise Server apenas), XFS
* Gestor de volumes: LVM2
* O software MultiPath i: mapeador de dispositivo

## <a name="region-support"></a>Suporte de região

Pode replicar e recuperar VMs entre quaisquer duas regiões dentro do mesmo cluster geográfico.

**Cluster geográfica** | **Regiões do Azure**
-- | --
América | Leste do Canadá, Canadá Central, Centro-Sul dos E.U.A., EUA Centro-Oeste, E.U.A. leste, E.U.A. Leste 2, E.U.A. oeste, E.U.A. oeste 2, E.U.A. Central, Centro-Norte
Europa | Oeste do Reino Unido, sul do Reino Unido, Europa do Norte, Europa Ocidental, França, Sul de França
Ásia | Sul da Índia, Índia Central, Sudeste asiático, leste do Japão, Ásia Oriental, oeste do Japão, Coreia Central, Sul da Coreia
Austrália   | Leste da Austrália, Sudeste da Austrália, Austrália Central, Austrália Central 2
Azure Government    | US GOV Virginia, US GOV Iowa, US GOV Arizona, US GOV Texas, US DOD leste, US DOD Centro
Alemanha | Alemanha Central, Alemanha Nordeste
China | Leste da China, Norte da China

>[!NOTE]
>
> Para a região sul do Brasil, pode apenas replicar e efetuar a ativação pós-falha para um dos Centro-Sul, e.u.a. centro-oeste, E.U.A. leste, E.U.A. Leste 2, E.U.A. oeste, E.U.A. oeste 2 e regiões Centro-Norte e a reativação pós-falha.

## <a name="support-for-vmdisk-management"></a>Suporte para a gestão de VM/disco

**Ação** | **Detalhes**
-- | ---
Redimensionar disco numa VM replicada | Suportadas
Adicionar disco à VM replicada | Não suportado. Precisa para desativar a replicação para a VM, adicione o disco e, em seguida, ative novamente a replicação.


## <a name="support-for-compute-configuration"></a>Suporte para a configuração de computação

**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Tamanho | Qualquer tamanho de VM do Azure com, pelo menos, 2 núcleos de CPU e 1 GB de RAM | Consulte [tamanhos de máquina virtual do Azure](../virtual-machines/windows/sizes.md)
Conjuntos de disponibilidade | Suportadas | Se utilizar a opção predefinida durante o passo de "Ativar replicação" no portal, o conjunto de disponibilidade é automaticamente criada com base na configuração da região de origem. Pode alterar a destino conjunto de disponibilidade no "item replicado > Definições > computação e rede > conjunto de disponibilidade" qualquer altura.
Zonas de disponibilidade | Não suportado | As VMs implementadas em zonas de disponibilidade não são atualmente suportadas.
VMs de benefício (HUB) de utilização híbrida | Suportadas | Se a VM de origem tiver uma licença HUB ativada, a ativação pós-falha de teste ou de uma VM de ativação pós-falha também utiliza a licença HUB.
Conjuntos de dimensionamento de máquinas virtuais | Não suportado |
Imagens da galeria do Azure - Microsoft publicado | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pelo Site Recovery
Imagens da galeria do Azure - publicado de terceiros | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pelo Site Recovery.
Imagens personalizadas - publicado de terceiros | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pelo Site Recovery.
VMs migradas com o Site Recovery | Suportadas | Se estiver a que migrar uma máquina de VMware/físico para Azure com o Site Recovery, terá de desinstalar a versão mais antiga do serviço de mobilidade e reinicie a máquina antes de replicá-los para outra região do Azure.

## <a name="support-for-storage-configuration"></a>Suporte para a configuração de armazenamento

**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Tamanho máximo de disco do SO | 2048 GB | Consulte [discos utilizados por VMs.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Tamanho do disco de dados máximo | 4095 GB | Consulte [discos utilizados por VMs.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Número de discos de dados | Até 64 como suportados por um tamanho de VM do Azure específico | Consulte [tamanhos de máquina virtual do Azure](../virtual-machines/windows/sizes.md)
Disco temporário | Sempre excluídos da replicação | Disco temporário é excluído da replicação sempre. Não, deve colocar todos os dados persistentes no disco temporário de acordo com a orientação do Azure. Consulte a [disco temporário em VMs do Azure](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) para obter mais detalhes.
Taxa de alteração de dados no disco | Máximo de 10 MBps por disco para armazenamento Premium e 2 MBps por disco de armazenamento Standard | Se a taxa de alteração de dados de média no disco para além dos 10 (para Premium) e 2 MBps (por padrão) continuamente, a replicação não irá acompanhar. No entanto, se for uma explosão de dados ocasionais e a taxa de alteração de dados for superior a 10 (para Premium) e 2 MBps (por padrão) há algum tempo e se resume, replicação será acompanhar. Neste caso, poderá ver pontos de recuperação um pouco atrasado.
Discos nas contas de armazenamento standard | Suportadas |
Discos nas contas de armazenamento premium | Suportadas | Se uma VM tem discos espalhadas em contas de armazenamento standard e premium, pode selecionar uma conta de armazenamento de destino diferente para cada disco garantir que tem a mesma configuração de armazenamento na região de destino
Discos geridos Standard | Suportada nas regiões do Azure em que o Azure Site Recovery é suportado. |  
Discos geridos Premium | Suportada nas regiões do Azure em que o Azure Site Recovery é suportado. |
Espaços de armazenamento | Suportadas |         
Encriptação em repouso (SSE) | Suportadas | O SSE é a predefinição nas contas de armazenamento.   
Azure Disk Encryption (ADE) para o sistema operacional do Windows | As VMs ativadas para [encryption com a aplicação do Azure AD](https://aka.ms/ade-aad-app) são suportados |
Azure Disk Encryption (ADE) para o SO Linux | Não suportado |
Disco de acesso frequente Adicionar/remover | Não suportado | Se adicionar ou remover o disco de dados na VM, terá de desativar a replicação e ative a replicação novamente para a VM.
Excluir o disco | Não suportado|   Disco temporário é excluído por predefinição.
Espaços de armazenamento direto  | Não suportado|
Servidor de ficheiros de escalamento horizontal  | Não suportado|
LRS | Suportadas |
GRS | Suportadas |
RA-GRS | Suportadas |
ZRS | Não suportado |  
Armazenamento de acesso esporádico e frequente | Não suportado | Discos da máquina virtual não são suportados no armazenamento de acesso esporádico e frequente
Firewalls de armazenamento do Azure para redes virtuais  | Não | Não é suportada a permissão de acesso a redes virtuais do Azure específicas em contas de armazenamento de cache utilizadas para armazenar dados replicados.
Contas de armazenamento para fins gerais V2 (camada de acesso tanto frequente ou esporádico) | Não | Aumento dos custos de transação substancialmente em comparação com fins gerais V1 contas de armazenamento

>[!IMPORTANT]
> Certifique-se de que observar que os destinos de escalabilidade e desempenho disco VM para [Linux](../virtual-machines/linux/disk-scalability-targets.md) ou [Windows](../virtual-machines/windows/disk-scalability-targets.md) máquinas virtuais para evitar quaisquer problemas de desempenho. Se seguir as configurações padrão, o Site Recovery irá criar os discos necessários e as contas de armazenamento com base na configuração da origem. Se personaliza e selecionar suas próprias definições, certifique-se de que segue os destinos de escalabilidade e desempenho de disco para as VMs de origem.

## <a name="support-for-network-configuration"></a>Suporte para a configuração de rede
**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Interface de rede (NIC) | Número máximo de NICs suportado por um tamanho de VM do Azure específico | NICs são criados quando a VM é criada como parte da ativação pós-falha de teste ou operação de ativação pós-falha. O número de NICs na VM de ativação pós-falha depende o número de NICs a VM tem no momento da ativação da replicação de origem. Se adicionar/remover NIC depois de ativar a replicação, não afeta a contagem de NIC na VM de ativação pós-falha.
Balanceador de Carga de Externo | Suportadas | Precisa associar o Balanceador de carga previamente configurada com um script de automatização do azure num plano de recuperação.
Balanceador de carga interno | Suportadas | Precisa associar o Balanceador de carga previamente configurada com um script de automatização do azure num plano de recuperação.
IP público| Suportadas | Tem de associar um IP público já existente para a NIC ou criar uma e associar a NIC usando um script de automatização do azure num plano de recuperação.
NSG em NIC (Resource Manager)| Suportadas | Tem de associar o NSG à NIC usando um script de automatização do azure num plano de recuperação.  
NSG na sub-rede (Resource Manager e clássica)| Suportadas | Tem de associar o NSG à sub-rede usando um script de automatização do azure num plano de recuperação.
NSG na VM (clássico)| Suportadas | Tem de associar o NSG à NIC usando um script de automatização do azure num plano de recuperação.
Reservado de IP (IP estático) / manter o IP de origem | Suportadas | Se a NIC na VM de origem tem uma configuração de IP estático e a sub-rede de destino tem o mesmo IP disponível, é atribuído à VM de ativação pós-falha. Se a sub-rede de destino não tem o mesmo IP disponível, um dos IPs disponíveis na sub-rede está reservado para esta VM. Pode especificar um IP fixo à sua escolha em "item replicado > Definições > computação e rede > interfaces de rede. Pode selecionar a NIC e especifique a sub-rede e o IP da sua preferência.
IP dinâmico| Suportadas | Se a NIC na VM de origem tem uma configuração de IP dinâmico, a NIC a ativação pós-falha de VM também é dinâmica por padrão. Pode especificar um IP fixo à sua escolha em "item replicado > Definições > computação e rede > interfaces de rede. Pode selecionar a NIC e especifique a sub-rede e o IP da sua preferência.
Integração do Gestor de Tráfego | Suportadas | Pode configurar previamente o seu Gestor de tráfego de forma que o tráfego é encaminhado para o ponto final na região de origem regularmente e para o ponto final na região de destino em caso de ativação pós-falha.
DNS de geridas do Azure | Suportadas |
DNS Personalizado  | Suportadas |    
Proxy não autenticados | Suportadas | Consulte [documento de orientação para funcionamento em rede.](site-recovery-azure-to-azure-networking-guidance.md)    
Proxy autenticado | Não suportado | Se a VM estiver a utilizar um proxy autenticado para conectividade de saída, não podem ser replicada com o Azure Site Recovery.    
VPN site a Site no local (com ou sem o ExpressRoute)| Suportadas | Certifique-se de que as UDRs e NSGs estão configurados de forma que o tráfego de recuperação de Site não é encaminhado para o local. Consulte [documento de orientação para funcionamento em rede.](site-recovery-azure-to-azure-networking-guidance.md)  
Ligação VNET a VNET | Suportadas | Consulte [documento de orientação para funcionamento em rede.](site-recovery-azure-to-azure-networking-guidance.md)  
Pontos Finais de Serviço de Rede Virtual | Suportadas | Firewalls de armazenamento do Azure para redes virtuais não são suportadas. Não é suportada a permissão de acesso a redes virtuais do Azure específicas em contas de armazenamento de cache utilizadas para armazenar dados replicados.
Redes Aceleradas | Suportadas | Funcionamento em rede acelerado tem de estar ativado na VM de origem. [Saiba mais](azure-vm-disaster-recovery-with-accelerated-networking.md).


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [redes orientações para replicar VMs do Azure](site-recovery-azure-to-azure-networking-guidance.md)
- Começar a proteger as cargas de trabalho por [replicar VMs do Azure](site-recovery-azure-to-azure.md)
