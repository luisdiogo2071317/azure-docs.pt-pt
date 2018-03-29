---
title: Proteger um servidor de ficheiros ao utilizar o Azure Site Recovery
description: Este artigo descreve como proteger um servidor de ficheiros utilizando o Azure Site Recovery
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 830f9c76d9d1bf11692fa9f2f5c49cbecdb69f25
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Proteger um servidor de ficheiros ao utilizar o Azure Site Recovery 

O [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento durante falhas planeadas e não planeadas. Recuperação de site gere e orquestra a recuperação após desastre de máquinas no local e as máquinas de virtuais (VMs) do Azure. Recuperação após desastre inclui a replicação, ativação pós-falha e recuperação de várias cargas de trabalho.

Este artigo descreve como proteger um servidor de ficheiros utilizando a recuperação de Site e faz com que outras recomendações de acordo com vários ambientes. 

- [Replicar máquinas de servidor de ficheiros do IaaS do Azure](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Replicar um servidor de ficheiros no local utilizando a recuperação de Site](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>Arquitetura de servidor de ficheiros
O objetivo de um abra sistema de partilha de ficheiros distribuído consiste em fornecer um ambiente em que um grupo de utilizadores distribuídos geograficamente pode colaborar para trabalhar de forma eficiente em ficheiros e ser garantida que os respetivos requisitos de integridade são impostos. Um ecossistema de servidor de ficheiros típica no local suporta um número elevado de utilizadores em simultâneo e um grande número de itens de conteúdo utiliza o ficheiro de sistema de replicação distribuídos (DFSR) para replicação agendamento e limitação de largura de banda. 

DFSR utiliza um algoritmo de compressão conhecido como diferencial compressão remota (RDC) que podem ser utilizados de forma eficiente atualizar ficheiros através de uma rede de largura de banda limitada. Deteta inserções, remoção e rearrangements dos dados em ficheiros. DFSR está ativada para replicar apenas os blocos de ficheiros alterados quando são atualizados ficheiros. Também existem ambientes de servidor de ficheiros, onde são efetuadas cópias de segurança diárias em temporizações de pico, que se adaptar a necessidades de desastre. DFSR não está implementada.

O diagrama seguinte ilustra o ambiente de servidor de ficheiros com DFSR implementado.
                
![Arquitetura DFSR](media/site-recovery-file-server/dfsr-architecture.JPG)

No diagrama anterior, vários servidores de ficheiros chamados membros ativamente participam na replicação de ficheiros entre um grupo de replicação. O conteúdo na pasta replicada está disponível para todos os clientes que enviam pedidos para um dos membros, mesmo se um membro fica offline.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Recomendações de recuperação de desastres para servidores de ficheiros

* **Replicar num servidor de ficheiros utilizando a recuperação de Site**: servidores de ficheiros podem ser replicados para o Azure utilizando a recuperação de Site. Quando um ou mais servidores de ficheiros no local não estão acessíveis, a recuperação de VMs pode ser colocada no Azure. As VMs, em seguida, podem servir pedidos de clientes, no local, não existe fornecido é conectividade VPN de site a site e do Active Directory está configurado no Azure. Pode utilizar este método no caso de um ambiente configurado de DFSR ou num ambiente de servidor de ficheiros simples com nenhuma DFSR. 

* **Expandir o DFSR para uma VM do IaaS do Azure**: num ambiente de servidor de ficheiros em cluster com o DFSR implementada, pode expandir o DFSR no local para o Azure. VM do Azure, em seguida, é ativada para realizar a função de servidor de ficheiros. 

    * Depois das dependências de conectividade VPN de site a site e o Active Directory são processadas e DFSR no local, quando um ou mais servidores de ficheiros no local não estão acessíveis, os clientes podem ligar à VM do Azure, que serve os pedidos.

    * Pode utilizar esta abordagem se as VMs têm configurações que não são suportadas pela recuperação de sites. Um exemplo é um disco de cluster partilhado, o que é por vezes, normalmente utilizado em ambientes de servidor de ficheiros. DFSR também funciona bem em ambientes de pouca largura de banda com a taxa de rotatividade média. Precisa de considerar o custo adicional de ter uma VM do Azure, cópias de segurança e sempre executadas. 

* **Utilizar a sincronização de ficheiros do Azure para replicar os ficheiros**: Se planeia utilizar a nuvem ou já utilizar uma VM do Azure, pode utilizar a sincronização de ficheiros do Azure. Sincronização de ficheiros do Azure oferece a sincronizar completamente gerido de partilhas de ficheiros na nuvem que estão acessíveis através da norma da indústria [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) protocolo (SMB). Partilhas de ficheiros do Azure, em seguida, podem ser montadas em simultâneo por implementações de nuvem ou no local do Windows, Linux e macOS. 

O diagrama seguinte ajuda-o a determinar a estratégia de que a utilizar para o seu ambiente de servidor de ficheiros.

![árvore de decisões](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>Fatores a considerar nas decisões sobre a recuperação após desastre para o Azure

|Ambiente  |Recomendação  |Pontos a considerar |
|---------|---------|---------|
|Ambiente de servidor de ficheiros com ou sem DFSR|   [Utilizar a recuperação de Site para a replicação](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Recuperação de sites não suporta clusters de disco partilhado ou de armazenamento ligados à rede (NAS). Se o seu ambiente utiliza estas configurações, utilize uma das outras abordagens, conforme apropriado. <br> Recuperação de sites não suporta o SMB 3.0. A VM replicada incorpore alterações apenas quando as alterações efetuadas aos ficheiros são atualizadas na localização original dos ficheiros.
|Ambiente de servidor de ficheiros com o DFSR     |  [Expandir o DFSR para uma máquina virtual do IaaS do Azure](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR funciona bem em ambientes extremamente crunched de largura de banda. Esta abordagem requer uma VM do Azure que está a funcionar e todas as o tempo de execução. Precisa de uma conta para o custo da VM no seu planeamento.         |
|VM do IaaS do Azure     |     [Sincronização de ficheiros ](#use-azure-file-sync-service-to-replicate-your-files)   |     Se utilizar a sincronização de ficheiros num cenário de recuperação após desastre, durante a ativação pós-falha tem de colocar as ações manuais para se certificar de que as partilhas de ficheiros estão acessíveis para a máquina do cliente de forma transparente. Sincronização de ficheiros requer que a porta 445 para ser aberto a partir do computador cliente.     |


### <a name="site-recovery-support"></a>Suporte de recuperação de site
Como a replicação do Site Recovery desconhece de aplicações, estas recomendações são esperadas para conter VERDADEIRO para os seguintes cenários.
| Origem    |Para um site secundário    |Para o Azure
|---------|---------|---------|
|Azure| -|Sim|
|Hyper-V|   Sim |Sim
|VMware |Sim|   Sim
|Servidor físico|   Sim |Sim
 

> [!IMPORTANT]
> Antes de continuar com qualquer um dos três seguintes abordagens, certifique-se de que estas dependências são tratadas.

**Conetividade site a site**: tem de ser estabelecida uma ligação direta entre o site no local e a rede do Azure para permitir a comunicação entre servidores. Utilize uma ligação de VPN de site a site segura a uma rede virtual do Azure que é utilizada como o site de recuperação após desastre. Para obter mais informações, consulte [estabelecer uma ligação de VPN de site a site entre um site no local e uma Azure virtual network](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Do Active Directory**: DFSR depende do Active Directory. Isto significa que a floresta do Active Directory com os controladores de domínio local é expandida para o site de recuperação após desastre no Azure. Mesmo se não estiverem a utilizar DFSR, se os utilizadores pretendidos tem de ser concedido acesso ou verificadas para o acesso, tem de seguir estes passos. Para obter mais informações, consulte [expanda no local do Active Directory para o Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Recomendação de recuperação de desastre para as máquinas virtuais do IaaS do Azure

Se estiver a configurar e gerir a recuperação após desastre de servidores de ficheiros alojados em VMs do IaaS do Azure, pode escolher entre duas opções, com base em se pretende mover para [ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [Utilizar a sincronização de ficheiros](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Utilizar a recuperação de Site](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>Utilizar a sincronização de ficheiros para replicar os ficheiros alojados numa máquina virtual IaaS

Os Ficheiros do Azure podem ser utilizados para substituir completamente ou complementar os servidores de ficheiros no local ou em dispositivos NAS tradicionais. Partilhas de ficheiros do Azure podem ser também replicadas com sincronização de ficheiros para os servidores do Windows, no local ou na nuvem, o desempenho e a colocação em cache distribuída dos dados em que é utilizado. Os passos seguintes descrevem a recomendação de recuperação após desastre para as VMs do Azure que executam a mesma funcionalidade como servidores de ficheiros tradicional:
* Proteger máquinas utilizando a recuperação de Site. Siga os passos no [replicar uma VM do Azure para outra região do Azure](azure-to-azure-quickstart.md).
* Utilize a sincronização de ficheiros para replicar os ficheiros da VM que funciona como servidor de ficheiros para a nuvem.
* Utilizar a recuperação de Site [plano de recuperação](site-recovery-create-recovery-plans.md) funcionalidade para adicionar scripts para [montar a partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) e aceder à partilha na sua máquina virtual.

Os seguintes passos descrevem brevemente como utilizar a sincronização de ficheiros:

1. [Criar uma conta de armazenamento no Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Se tiver escolhido o armazenamento georredundante com acesso de leitura para as contas do storage, obter acesso de leitura aos seus dados da região secundária em caso de desastre. Para obter mais informações, consulte [estratégias de recuperação de desastre de partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Criar uma partilha de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Iniciar sincronização de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) no seu servidor de ficheiros do Azure.
4. Crie um grupo de sincronização. Pontos finais dentro de um grupo de sincronização são mantidos sincronizados entre si. Um grupo de sincronização tem de conter pelo menos um ponto final da nuvem, que representa uma partilha de ficheiros do Azure. Um grupo de sincronização também tem de conter um ponto final do servidor, que representa um caminho num servidor do Windows.
5. Os ficheiros são agora mantidos sincronizados entre a partilha de ficheiros do Azure e o servidor no local.
6. Em caso de desastre no seu ambiente no local, execute uma ativação pós-falha utilizando um [plano de recuperação](site-recovery-create-recovery-plans.md). Adicione o script para [montar a partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) e aceder à partilha na sua máquina virtual.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Replicar uma máquina de virtual do servidor de ficheiros IaaS utilizando a recuperação de Site

Se tiver clientes no local que aceder à máquina virtual de servidor de ficheiro de IaaS, executar os seguintes passos. Caso contrário, avance para o passo 3.

1. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure.
2. Expanda o Active Directory no local.
3. [Configurar a recuperação após desastre](azure-to-azure-tutorial-enable-replication.md) para a máquina do servidor de ficheiros IaaS para uma região secundária.


Para obter mais informações sobre recuperação de desastres para uma região secundária, consulte [neste artigo](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Replicar um servidor de ficheiros no local utilizando a recuperação de Site

Os passos seguintes descrevem a replicação para uma VM de VMware. Para obter os passos replicar uma VM de Hyper-V, consulte [neste tutorial](tutorial-hyper-v-to-azure.md).

1. [Preparar os recursos do Azure](tutorial-prepare-azure.md) para a replicação de máquinas no local.
2. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure. 
3. Expanda o Active Directory no local.
4. [Preparar servidores do VMware no local](tutorial-prepare-on-premises-vmware.md).
5. [Configurar a recuperação após desastre](tutorial-vmware-to-azure.md) para o Azure para VMs no local.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Expandir o DFSR para uma máquina virtual do IaaS do Azure

1. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure. 
2. Expanda o Active Directory no local.
3. [Criar e aprovisionar um servidor de ficheiros VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) na rede virtual do Azure.
Certifique-se de que a máquina virtual é adicionada à mesma rede virtual do Azure, que tenha conectividade em vários locais com o ambiente no local. 
4. Instalar e [configurar DFSR](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) no Windows Server.
5. [Implementar um espaço de nomes DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. Com o espaço de nomes DFS implementado, a ativação pós-falha das pastas partilhadas de produção para locais de recuperação de desastre pode ser feita atualizando os destinos de pasta de espaço de nomes DFS. Depois destas DFS alterações de espaço de nomes são replicadas através do Active Directory, os utilizadores estão ligados transparente para os destinos de pasta adequados.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>Utilizar a sincronização de ficheiros para replicar os ficheiros no local
Pode utilizar a sincronização de ficheiros para replicar os ficheiros para a nuvem. Em caso de desastre e indisponibilidade do seu servidor de ficheiros no local, pode montar as localizações de ficheiros pretendido da nuvem e continuar a pedidos de serviço a partir de computadores cliente.
Para integrar a sincronização de ficheiros com a recuperação de Site:

* Proteger as máquinas de servidor de ficheiros utilizando a recuperação de Site. Siga os passos no [neste tutorial](tutorial-vmware-to-azure.md).
* Utilize a sincronização de ficheiros para replicar os ficheiros da máquina que funciona como um servidor de ficheiros para a nuvem.
* Utilize a funcionalidade de plano de recuperação na recuperação de Site para adicionar scripts para montar a partilha de ficheiros do Azure no servidor de ficheiros de efetuada a ativação pós-falha VM no Azure.

Siga estes passos para utilizar a sincronização de ficheiros:

1. [Criar uma conta de armazenamento no Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Se tiver escolhido o armazenamento georredundante com acesso de leitura (recomendado) para as contas do storage, tem acesso de leitura aos seus dados da região secundária em caso de desastre. Para obter mais informações, consulte [estratégias de recuperação de desastre de partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Criar uma partilha de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Implementar a sincronização de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) no seu servidor de ficheiros no local.
4. Crie um grupo de sincronização. Pontos finais dentro de um grupo de sincronização são mantidos sincronizados entre si. Um grupo de sincronização tem de conter pelo menos um ponto final da nuvem, que representa uma partilha de ficheiros do Azure. O grupo de sincronização também tem de conter um ponto final do servidor, que representa um caminho no Windows server no local.
5. Os ficheiros são agora mantidos sincronizados entre a partilha de ficheiros do Azure e o servidor no local.
6. Em caso de desastre no seu ambiente no local, execute uma ativação pós-falha utilizando um [plano de recuperação](site-recovery-create-recovery-plans.md). Adicione o script para montar a partilha de ficheiros do Azure e aceder à partilha na sua máquina virtual.

> [!NOTE]
> Certifique-se de que a porta 445 está aberta. Ficheiros do Azure utiliza o protocolo SMB. SMB comunica através da porta TCP 445. Verifique se a firewall não está a bloquear a porta TCP 445 partir de uma máquina cliente.


## <a name="do-a-test-failover"></a>Efetue uma ativação pós-falha de teste

1. Aceda ao portal do Azure e selecione o Cofre de serviço de recuperação.
2. Selecione o plano de recuperação criado para o ambiente de servidor de ficheiros.
3. Selecione **ativação pós-falha de teste**.
4. Selecione o ponto de recuperação e a rede virtual do Azure para iniciar o processo de ativação pós-falha de teste.
5. Depois do ambiente secundário está ativo, efetue as validações.
6. Depois de validações terminar, selecione **ativação pós-falha de teste de limpeza** no plano de recuperação e ativação pós-falha de teste ambiente limpos.

Para obter mais informações sobre como efetuar uma ativação pós-falha de teste, consulte [testar a ativação pós-falha no Site Recovery](site-recovery-test-failover-to-azure.md).

Para obter orientações sobre a fazer a ativação pós-falha de teste para o Active Directory e DNS, consulte [considerações de ativação pós-falha do Active Directory e DNS](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Efetue uma ativação pós-falha

1. Aceda ao portal do Azure e selecione o Cofre dos serviços de recuperação.
2. Selecione o plano de recuperação criado para o ambiente de servidor de ficheiros.
3. Selecione **ativação pós-falha**.
4. Selecione o ponto de recuperação para iniciar o processo de ativação pós-falha.

Para obter mais informações sobre como efetuar uma ativação pós-falha, consulte [ativação pós-falha na recuperação de Site](site-recovery-failover.md).
