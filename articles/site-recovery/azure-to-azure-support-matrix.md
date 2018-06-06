---
title: Matriz de suporte do Azure Site Recovery para replicar a partir do Azure para o Azure | Microsoft Docs
description: Resume os sistemas operativos suportados e configurações para a replicação do Azure Site Recovery máquinas virtuais do Azure (VMs) de uma região para outro para as necessidades de recuperação (DR) após desastre.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: sujayt
ms.openlocfilehash: 19c439e1182086b91d05f8bb23bc6c07c34a12a2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716317"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Matriz de suporte para replicar a partir de uma região do Azure para outro



Este artigo resume os componentes e configurações suportadas para o Azure Site Recovery quando replicar e recuperar máquinas virtuais do Azure a partir de uma região noutra região.

## <a name="user-interface-options"></a>Opções de interface de utilizador

**Interface de utilizador** |  **Suportado / não suportado**
--- | ---
**Portal do Azure** | Suportadas
**Portal clássico** | Não suportado
**PowerShell** | [Replicação do Azure para o Azure com o PowerShell](azure-to-azure-powershell.md)
**API REST** | Não é atualmente suportado
**CLI** | Não é atualmente suportado


## <a name="resource-move-support"></a>Suporte de movimentação de recursos

**Tipo de movimentação de recurso** | **Suportado / não suportado** | **Observações**  
--- | --- | ---
**Mover o Cofre entre grupos de recursos** | Não suportado |Não é possível mover o Cofre de serviços de recuperação em grupos de recursos.
**Mover computação, armazenamento e de rede entre grupos de recursos** | Não suportado |Se mover uma máquina virtual (ou os respetivos componentes, tais como o armazenamento e de rede associados) depois de ativar a replicação, terá de desativar a replicação e ativar a replicação da máquina virtual novamente.



## <a name="support-for-deployment-models"></a>Suporte para modelos de implementação

**Deployment model** (Modelo de implementação) | **Suportado / não suportado** | **Observações**  
--- | --- | ---
**Clássico** | Suportadas | Apenas pode replicar uma máquina virtual clássica e recuperá-la como uma máquina virtual clássica. Não é possível recuperá-la como uma máquina virtual do Gestor de recursos. Se implementar uma VM clássica sem uma rede virtual e diretamente a uma região do Azure, não é suportada.
**Resource Manager** | Suportadas |

>[!NOTE]
>
> 1. Replicar máquinas virtuais do Azure a partir de uma subscrição para outro para cenários de recuperação após desastre não é suportada.
> 2. Azure migrar máquinas virtuais entre subscrições não é suportada.
> 3. Azure migrar máquinas virtuais na mesma região não é suportada.
> 4. Migrar máquinas virtuais do Azure do modelo de implementação clássica para o recurso de modelo de implementação manager não é suportado.

## <a name="support-for-replicated-machine-os-versions"></a>Suporte para versões de SO da máquina replicada

O abaixo suporte é aplicável a qualquer carga de trabalho em execução no sistema operativo mencionados.

#### <a name="windows"></a>Windows

- Windows Server 2016 (servidor principal, servidor com experiência de utilização) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 com, pelo menos, SP1

>[!NOTE]
>
> \* Windows Server 2016 Nano Server não é suportado.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8 6.9, 7.0, 7.1, 7.2, 7.3,7.4
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3,7.4
- Ubuntu 14.04 LTS Server [ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7 [ (versões de kernel suportado)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Debian 8 [ (versões de kernel suportado)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Oracle Enterprise Linux 6.4, 6.5 com o kernel compatível do Red Hat ou Unbreakable Enterprise Kernel versão 3 (UEK3)
- SP3 do SUSE Linux Enterprise Server 11
- SP4 do SUSE Linux Enterprise Server 11

(Não é suportada a atualização de replicar máquinas do SLES 11 SP3 para SLES 11 SP4. Se uma máquina replicada tiver sido atualizada do SLES 11SP3 para SLES 11 SP4, terá de desativar a replicação e proteger o computador novamente post a atualização.)

>[!NOTE]
>
> Ubuntu servidores utilizando a autenticação baseada em palavra-passe e de início de sessão e utilizar o pacote de nuvem init para configurar máquinas virtuais na nuvem, podem ter início de sessão baseada em palavra-passe desativado após a ativação pós-falha (dependendo da configuração de cloudinit.) Início de sessão baseada em palavra-passe pode ser novamente ativado na máquina virtual ao repor a palavra-passe no menu de definições (sob a suporte + a resolução de problemas de secção) da máquina virtual no portal do Azure de ativação pós-falha.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Versões suportadas do kernel Ubuntu máquinas virtuais do Azure

**Versão** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
14.04 LTS | 9.13 | 3.13.0-24-Generic para 3.13.0-137-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-104-generic |
14.04 LTS | 9.14 | 3.13.0-24-Generic para 3.13.0-141-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-112-generic |
14.04 LTS | 9.15 | 3.13.0-24-Generic para 3.13.0-143-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-116-generic |
14.04 LTS | 9.16 | 3.13.0-24-Generic para 3.13.0-144-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-119-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic para 4.4.0-104-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic |
16.04 LTS | 9.14 | 4.4.0-21-Generic para 4.4.0-112-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-32-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1009-azure |
16.04 LTS | 9.15 | 4.4.0-21-Generic para 4.4.0-116-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-37-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1012-azure |
16.04 LTS | 9.16 | 4.4.0-21-Generic para 4.4.0-119-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-38-generic,<br/>4.11.0-1009-Azure para 4.11.0-1016-azure,<br/>4.13.0-1005-Azure para 4.13.0-1012-azure |


### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Versões de Debian kernel para máquinas virtuais do Azure

**Versão** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
Debian 7 | 9.14, 9.15, 9.16 | 3.2.0-4-AMD64 para 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 8 | 9.14, 9.15, 9.16 | 3.16.0-4-AMD64 para 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.5-amd64 |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Sistemas de ficheiro suportados e configurações de armazenamento de convidado em máquinas virtuais do Azure com o SO Linux

* Sistemas de ficheiros: ext3 ext4, ReiserFS (Suse Linux Enterprise Server apenas), XFS
* Gestor de volumes: LVM2
* O software MultiPath i: mapeador de dispositivo

## <a name="region-support"></a>Suporte de região

Pode replicar e recuperar VMs entre as duas regiões do mesmo cluster geográfica.

**Cluster geográfica** | **Regiões do Azure**
-- | --
América do | Canadá leste, EUA Central Canadá Central, -Sul, EUA Centro Oeste, EUA leste, EUA Leste 2, EUA oeste, EUA oeste, 2, EUA Central, EUA Centro-Norte
Europa | RU oeste, RU Sul, Europa do Norte, Europa Ocidental, Sul de Central, França França
Ásia | Sul da Índia, Índia Central, Sudeste asiático, leste do Japão, Ásia Oriental, oeste do Japão, Coreia Central, Coreia South
Austrália   | Leste da Austrália, Sudeste da Austrália
Azure Government    | E.u. a US Virginia, E.U.A. us Iowa, E.U.A. us Arizona, E.U.A. us Texas, DOD de e.u. a leste, DOD de e.u. a Central
Alemanha | Alemanha Central, Alemanha Nordeste
China | Leste da China, Norte da China

>[!NOTE]
>
> Região sul do Brasil, pode apenas replicar e efetuar a ativação pós-falha para um dos Sul Central nos, Central EUA oeste, EUA leste, EUA Leste 2, EUA oeste, EUA oeste, 2 e regiões Norte Central-nos e voltar a falhar.


## <a name="support-for-compute-configuration"></a>Suporte para a configuração de computação

**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Tamanho | Nenhum tamanho de VM do Azure com, pelo menos, 2 núcleos de CPU e de 1 GB de RAM | Consulte [tamanhos de máquina virtual do Azure](../virtual-machines/windows/sizes.md)
Conjuntos de disponibilidade | Suportadas | Se utilizar a opção predefinida durante 'Ativar a replicação' passo no portal, o conjunto de disponibilidade é automaticamente criada com base na configuração da região de origem. Pode alterar a disponibilidade de destino definida em ' replicados item > Definições > computação e rede > do conjunto de disponibilidade ' qualquer altura.
VMs do híbrida utilize benefício (CONCENTRADOR) | Suportadas | Se a VM de origem tiver licenças HUB ativada, a ativação pós-falha de teste ou de uma VM de ativação pós-falha também utiliza a licença do HUB.
Conjuntos de dimensionamento de máquinas virtuais | Não suportado |
Imagens de galeria do Azure - Microsoft publicado | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pela recuperação de sites
Imagens de galeria do Azure - terceiros publicadas | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pela recuperação de sites.
Personalizar imagens - terceiros publicadas | Suportadas | Suportado, desde que a VM é executado num sistema operativo suportado pela recuperação de sites.
VMs migrada utilizando a recuperação de sites | Suportadas | Se estiver a que migrar uma máquina de VMware/físico para o Azure utilizando a recuperação de sites, terá de desinstalar a versão mais antiga do serviço de mobilidade e reinicie a máquina antes de replicar-o para outra região do Azure.

## <a name="support-for-storage-configuration"></a>Suporte para a configuração de armazenamento

**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Tamanho máximo de disco do SO | 2048 GB | Consulte [discos utilizados por VMs.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Tamanho do disco de dados | 4095 GB | Consulte [discos utilizados por VMs.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Número de discos de dados | Até 64 como suportado por um tamanho de VM do Azure específico | Consulte [tamanhos de máquina virtual do Azure](../virtual-machines/windows/sizes.md)
Disco temporário | Sempre excluídos da replicação | Disco temporário foi excluído da replicação sempre. Não deve colocar todos os dados persistentes em disco temporário de acordo com a documentação de orientação do Azure. Consulte [disco temporário em VMs do Azure](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) para obter mais detalhes.
Taxa de alteração de dados no disco | Máximo de 10 por disco para o Premium storage e 2 MBps por disco de armazenamento Standard | Se a taxa de alteração de média de dados no disco para além dos 10 (para Premium) e 2 MBps (para Standard) continuamente, a replicação não irá detetar cópias de segurança. No entanto, se for uma rajada ocasionais dados e a taxa de alteração de dados é maior que 10 (para Premium) e 2 MBps (para Standard) durante algum tempo e ficar, a replicação será acompanhar. Neste caso, poderá ver pontos de recuperação ligeiramente atrasado.
Discos em contas de armazenamento standard | Suportadas |
Discos em contas de armazenamento premium | Suportadas | Se uma VM tem discos distribuídos por contas do standard storage e premium, pode selecionar uma conta de armazenamento de destino diferente para cada disco garantir que têm a mesma configuração de armazenamento na região de destino
Discos gerida padrão | Suportados nas regiões do Azure na qual o Azure Site Recovery é suportado. Nuvens de Government não são atualmente suportadas.  |  
Discos Premium gerido | Suportados nas regiões do Azure na qual o Azure Site Recovery é suportado. Nuvens de Government não são atualmente suportadas. |
Espaços de armazenamento | Suportadas |         
Encriptação de Inativos (SSE) | Suportadas | Para contas de armazenamento de cache e de destino, pode selecionar uma conta de armazenamento de SSE ativada.     
Encriptação de disco do Azure (ADE) | Não suportado |
Disco frequente Adicionar/remover | Não suportado | Se adicionar ou remover o disco de dados na VM, terá de desativar a replicação e ativar a replicação novamente para a VM.
Excluir o disco | Não suportado|   Disco temporário é excluído por predefinição.
Direto de espaços de armazenamento  | Não suportado|
Servidor de ficheiros de escalamento horizontal  | Não suportado|
LRS | Suportadas |
GRS | Suportadas |
RA-GRS | Suportadas |
ZRS | Não suportado |  
Armazenamento de acesso frequente e esporádico | Não suportado | Discos da máquina virtual não são suportados no armazenamento de acesso frequente e esporádico
Firewalls de armazenamento do Azure para redes virtuais  | Não | Permitir o acesso específicos redes virtuais do Azure em contas de armazenamento de cache utilizadas para armazenar dados replicados não é suportada.
Contas de armazenamento de V2 para fins gerais (camada ambos frequente e esporádico) | Não | Aumentar os custos de transação substancialmente em comparação com para fins gerais V1 contas de armazenamento

>[!IMPORTANT]
> Certifique-se de que observar os destinos de desempenho e escalabilidade do disco VM para [Linux](../virtual-machines/linux/disk-scalability-targets.md) ou [Windows](../virtual-machines/windows/disk-scalability-targets.md) máquinas virtuais para evitar quaisquer problemas de desempenho. Se seguir as predefinições, a recuperação de sites irá criar as contas de armazenamento com base na configuração da origem e discos necessários. Se personalizar e selecione as suas próprias definições, certifique-se de que segue os destinos de escalabilidade e desempenho de disco para as VMs de origem.

## <a name="support-for-network-configuration"></a>Suporte para a configuração de rede
**Configuração** | **Suportado/não suportado** | **Observações**
--- | --- | ---
Interface de rede (NIC) | Até o número máximo de NICs suportado por um tamanho de VM do Azure específico | NICs são criados quando é criada a VM como parte da ativação pós-falha de teste ou de operação de ativação pós-falha. O número de NICs a ativação pós-falha da VM depende do número de NICs a VM tem no momento da ativação da replicação de origem. Se lhe adicionar/remover NIC depois de ativar a replicação, não afeta a contagem NIC na ativação pós-falha da VM.
Balanceador de Carga de Externo | Suportadas | Precisa de associar o Balanceador de carga previamente configurada utilizando um script de automatização do azure num plano de recuperação.
Balanceador de carga interno | Suportadas | Precisa de associar o Balanceador de carga previamente configurada utilizando um script de automatização do azure num plano de recuperação.
IP público| Suportadas | Tem de associar um IP público já existente para o NIC ou crie um e associe a NIC utilizando um script de automatização do azure num plano de recuperação.
NSG no NIC (Resource Manager)| Suportadas | Precisa de associar o NSG para o NIC utilizando um script de automatização do azure num plano de recuperação.  
NSG na sub-rede (Resource Manager e clássico)| Suportadas | Precisa de associar o NSG para sub-rede a utilizar um script de automatização do azure num plano de recuperação.
NSG na VM (clássica)| Suportadas | Precisa de associar o NSG para o NIC utilizando um script de automatização do azure num plano de recuperação.
(IP estático) do IP reservado / manter o IP de origem | Suportadas | Se a NIC na VM de origem tiver uma configuração de IP estático e a sub-rede de destino tiver o mesmo IP disponível, é atribuída para a ativação pós-falha da VM. Se a sub-rede de destino não tem o mesmo IP disponível, uma dos IPs disponíveis na sub-rede é reservada para esta VM. Pode especificar um IP fixo da sua preferência em ' item replicados > Definições > computação e rede > interfaces de rede. Pode selecionar a NIC e especifique a sub-rede e o IP da sua preferência.
IP dinâmico| Suportadas | Se a NIC na VM de origem tiver uma configuração de IP dinâmica, o NIC a ativação pós-falha de VM também é dinâmica por predefinição. Pode especificar um IP fixo da sua preferência em ' item replicados > Definições > computação e rede > interfaces de rede. Pode selecionar a NIC e especifique a sub-rede e o IP da sua preferência.
Integração do Gestor de Tráfego | Suportadas | Pode pré-configurar o Gestor de tráfego de forma a que o tráfego é encaminhado para o ponto final na região de origem regulares e para o ponto final na região de destino em caso de ativação pós-falha.
Azure geridos DNS | Suportadas |
DNS Personalizado  | Suportadas |    
Proxy não autenticados | Suportadas | Consulte [documento de orientação na rede.](site-recovery-azure-to-azure-networking-guidance.md)    
Proxy autenticado | Não suportado | Se a VM estiver a utilizar um proxy autenticado para conectividade de saída, não é possível replicar utilizando o Azure Site Recovery.    
VPN de site a Site no local (com ou sem ExpressRoute)| Suportadas | Certifique-se de que o UDRs e NSGs estão configurados de forma a que o tráfego de recuperação do Site não for encaminhado para o local. Consulte [documento de orientação na rede.](site-recovery-azure-to-azure-networking-guidance.md)  
Ligação VNET a VNET | Suportadas | Consulte [documento de orientação na rede.](site-recovery-azure-to-azure-networking-guidance.md)  
Pontos Finais de Serviço de Rede Virtual | Suportadas | Firewalls de armazenamento do Azure para as redes virtuais não são suportadas. Permitir o acesso específicos redes virtuais do Azure em contas de armazenamento de cache utilizadas para armazenar dados replicados não é suportada.
Redes Aceleradas | Não suportado | Pode ser replicada uma VM com acelerados rede ativada, mas a ativação pós-falha de VM não terão acelerados de rede ativada. Serão também desativadas na melhoria de redes de VM de origem na reativação pós-falha.


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [redes orientações para replicar as VMs do Azure](site-recovery-azure-to-azure-networking-guidance.md)
- Começar a proteger as cargas de trabalho por [replicar as VMs do Azure](site-recovery-azure-to-azure.md)
