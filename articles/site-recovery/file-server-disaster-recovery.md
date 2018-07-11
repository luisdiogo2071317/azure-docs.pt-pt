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
ms.date: 07/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 0b6d5dccbce30c55e259e4bb3f8ae4194a02b646
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916888"
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Proteger um servidor de ficheiros ao utilizar o Azure Site Recovery 

O [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento durante falhas planeadas e não planeadas. Site Recovery gere e orquestra a recuperação após desastre de máquinas no local e máquinas virtuais do Azure (VMs). Recuperação após desastre inclui replicação, ativação pós-falha e recuperação de várias cargas de trabalho.

Este artigo descreve como proteger um servidor de ficheiros utilizando a recuperação de Site e faz com que outras recomendações de acordo com vários ambientes. 

- [Replicar máquinas do servidor de ficheiros de IaaS do Azure](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Replicar um servidor de ficheiros no local com o Site Recovery](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>Arquitetura de servidor de ficheiros
O objetivo de um aberto sistema de compartilhamento de arquivos distribuído é fornecer um ambiente em que um grupo de usuários distribuídos geograficamente pode colaborar para trabalhar com eficiência em ficheiros e possível garantir que os seus requisitos de integridade são impostos. Um ecossistema de servidor de ficheiros típico no local suporta um elevado número de utilizadores em simultâneo e um grande número de itens de conteúdo utiliza a replicação de sistema de ficheiros (DFSR) distribuído para replicação de agendamento e limitação de largura de banda. 

DFSR utiliza um algoritmo de compactação conhecido como RDC compactação diferencial remota () que podem ser utilizados para ficheiros de atualização com eficiência através de uma rede de largura de banda limitada. Deteta inserções, remoções e reorganização de dados em arquivos. DFSR está habilitado para replicar apenas os blocos de arquivo alterado quando os arquivos são atualizados. Também há ambientes de servidor de ficheiros, onde as cópias de segurança diárias são executadas em temporizações de pico, que se destina às necessidades de desastre. Não é implementada a DFSR.

O diagrama seguinte ilustra o ambiente de servidor de ficheiros com o DFSR implementado.
                
![Arquitetura DFSR](media/site-recovery-file-server/dfsr-architecture.JPG)

No diagrama anterior, chamados ativamente a membros de vários servidores de ficheiros participam na replicação de ficheiros entre um grupo de replicação. O conteúdo na pasta replicada está disponível para todos os clientes que enviam pedidos para qualquer um dos membros, mesmo se um membro fica offline.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Recomendações de recuperação após desastre para servidores de ficheiros

* **Replicar um servidor de ficheiros utilizando a recuperação de Site**: servidores de ficheiros podem ser replicados para o Azure utilizando a recuperação de Site. Quando um ou mais servidores de ficheiros no local estão inacessíveis, a recuperação de VMs pode ser colocada no Azure. As VMs, em seguida, podem servir pedidos de clientes, no local, fornecido aqui é a conectividade VPN de site a site e do Active Directory está configurado no Azure. Pode utilizar este método, no caso de um ambiente configurado o DFSR ou um ambiente de servidor de arquivo simples com nenhuma DFSR. 

* **Expandir o DFSR para uma VM de IaaS do Azure**: num ambiente de servidor de ficheiros em cluster com o DFSR implementado, pode expandir o DFSR no local para o Azure. Uma VM do Azure, em seguida, está ativada para executar a função de servidor de ficheiros. 

    * Depois das dependências de conectividade VPN de site a site e o Active Directory são processadas e DFSR está no lugar, quando um ou mais servidores de ficheiros no local estão inacessíveis, os clientes podem ligar à VM do Azure, que serve os pedidos.

    * Pode utilizar esta abordagem se as VMs tenham as configurações que não são suportadas pelo Site Recovery. Um exemplo é um disco de cluster partilhado, o que é, às vezes, normalmente utilizado em ambientes de servidor de ficheiros. DFSR também funciona bem em ambientes de baixa largura de banda com taxa de abandono médio. Precisa considerar o custo adicional de ter uma VM do Azure e em execução o tempo todo. 

* **Utilizar o Azure File Sync para replicar os ficheiros**: Se planeia utilizar a cloud ou já utilizar uma VM do Azure, pode utilizar o Azure File Sync. O Azure File Sync oferece a sincronização de partilhas de ficheiros completamente geridas na cloud que são acessíveis através da norma da indústria [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) protocolo (SMB). Partilhas de ficheiros do Azure, em seguida, podem ser montadas em simultâneo por implementações na cloud ou no local do Windows, Linux e macOS. 

O diagrama a seguir ajuda a determinar quais estratégia a utilizar para o seu ambiente de servidor de ficheiros.

![Árvore de decisões](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>Fatores a considerar em suas decisões sobre a recuperação após desastre para o Azure

|Ambiente  |Recomendação  |Pontos a considerar |
|---------|---------|---------|
|Ambiente de servidor de ficheiros com ou sem DFSR|   [Utilize o Site Recovery para a replicação](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Site Recovery não suporta clusters de disco partilhado ou de armazenamento ligados à rede (NAS). Se o seu ambiente utiliza estas configurações, utilize qualquer uma das outras abordagens, conforme apropriado. <br> Site Recovery não suporta o SMB 3.0. A VM replicada incorpora as alterações apenas quando as alterações feitas para os arquivos são atualizadas na localização original dos ficheiros.
|Ambiente de servidor de ficheiros com o DFSR     |  [Expandir o DFSR para a máquina virtual IaaS do Azure](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR funciona bem em ambientes extremamente fragmentados de largura de banda. Essa abordagem exige uma VM do Azure que está a funcionar e em execução o tempo todo. Precisa levar em conta o custo da VM no seu planejamento.         |
|VM de IaaS do Azure     |     [Sincronização de ficheiros ](#use-azure-file-sync-service-to-replicate-your-files)   |     Se utilizar a sincronização de ficheiros num cenário de recuperação após desastre, durante a ativação pós-falha tem de executar ações manuais para se certificar de que as partilhas de ficheiros são acessíveis para o computador cliente de forma transparente. Sincronização de ficheiros requer a porta 445 ser abrir a partir do computador cliente.     |


### <a name="site-recovery-support"></a>Suporte de recuperação de site
Como a replicação do Site Recovery é independente de aplicativo, estas recomendações devem ser verdadeira para os seguintes cenários.
| Origem    |Para um site secundário    |Para o Azure
|---------|---------|---------|
|Azure| -|Sim|
|Hyper-V|   Sim |Sim
|VMware |Sim|   Sim
|Servidor físico|   Sim |Sim
 

> [!IMPORTANT]
> Antes de continuar com qualquer um dos seguintes três abordagens, certifique-se de que estas dependências cuidam da.

**Conetividade site a site**: tem de ser estabelecida uma ligação direta entre o site no local e a rede do Azure para permitir a comunicação entre servidores. Utilize uma ligação de VPN de site a site segura a uma rede virtual do Azure que é utilizada como o site de recuperação após desastre. Para obter mais informações, consulte [estabelecer uma ligação de VPN de site a site entre um site no local e uma rede virtual do Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Do Active Directory**: DFSR depende do Active Directory. Isso significa que a floresta do Active Directory com os controladores de domínio local é expandida para o site de recuperação após desastre no Azure. Mesmo se não estiver a utilizar DFSR, se os usuários precisam de ser concedido acesso ou verificadas para acesso, tem de seguir estes passos. Para obter mais informações, consulte [expandir no local do Active Directory para o Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Recomendação de recuperação após desastre para máquinas de virtuais de IaaS do Azure

Se estiver a configurar e gerir a recuperação após desastre de servidores de ficheiros alojadas em VMs IaaS do Azure, pode escolher entre duas opções, com base em se pretende mover para o [ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [Utilizar a sincronização de ficheiros](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Utilize o Site Recovery](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>Utilizar a sincronização de ficheiros para replicar arquivos alojados numa máquina virtual IaaS

Os Ficheiros do Azure podem ser utilizados para substituir completamente ou complementar os servidores de ficheiros no local ou em dispositivos NAS tradicionais. Partilhas de ficheiros do Azure também podem ser replicadas com o File Sync em servidores de Windows, no local ou na cloud, para o desempenho e a colocação em cache distribuída dos dados em que é utilizada. Os passos seguintes descrevem a recomendação de recuperação após desastre para VMs do Azure que executam a mesma funcionalidade que os servidores de ficheiros tradicional:
* Proteger máquinas com o Site Recovery. Siga os passos em [replicar uma VM do Azure para outra região do Azure](azure-to-azure-quickstart.md).
* Utilize a sincronização de ficheiros para replicar arquivos a partir da VM que age como o servidor de ficheiros para a cloud.
* Utilize o Site Recovery [plano de recuperação](site-recovery-create-recovery-plans.md) funcionalidade adicionar scripts para [montar a partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) e aceder à partilha na sua máquina virtual.

Os passos seguintes descrevem brevemente como usar a sincronização de ficheiros:

1. [Criar uma conta de armazenamento no Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Se tiver escolhido o armazenamento georredundante com acesso de leitura para as suas contas de armazenamento, obtém acesso de leitura aos seus dados da região secundária em caso de desastre. Para obter mais informações, consulte [estratégias de recuperação de desastres de partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Criar uma partilha de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Iniciar a sincronização de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) no seu servidor de ficheiros do Azure.
4. Crie um grupo de sincronização. Pontos finais dentro de um grupo de sincronização são mantidos em sincronia entre si. Um grupo de sincronização tem de conter pelo menos um ponto final da cloud, que representa uma partilha de ficheiros do Azure. Um grupo de sincronização também tem de conter um ponto final do servidor, que representa um caminho de um servidor do Windows.
5. Os ficheiros agora são mantidos em sincronia entre a partilha de ficheiros do Azure e o seu servidor no local.
6. Em caso de desastre no seu ambiente no local, execute uma ativação pós-falha ao utilizar um [plano de recuperação](site-recovery-create-recovery-plans.md). Adicione o script para [montar a partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) e aceder à partilha na sua máquina virtual.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Replicar a máquina de virtual de servidor de ficheiros com o IaaS com o Site Recovery

Se tiver clientes no local que aceder à máquina de virtual de servidor de ficheiros de IaaS, tire os seguintes passos. Caso contrário, avance para o passo 3.

1. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure.
2. Expanda o Active Directory no local.
3. [Configurar a recuperação após desastre](azure-to-azure-tutorial-enable-replication.md) para a máquina de servidor de ficheiros de IaaS para uma região secundária.


Para obter mais informações sobre recuperação de desastres para uma região secundária, consulte [este artigo](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Replicar um servidor de ficheiros no local com o Site Recovery

Os passos seguintes descrevem os replicação para uma VM de VMware. Para obter os passos replicar uma VM de Hyper-V, consulte [deste tutorial](tutorial-hyper-v-to-azure.md).

1. [Preparar os recursos do Azure](tutorial-prepare-azure.md) para a replicação de máquinas no local.
2. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure. 
3. Expanda o Active Directory no local.
4. [Preparar servidores VMware no local](tutorial-prepare-on-premises-vmware.md).
5. [Configurar a recuperação após desastre](tutorial-vmware-to-azure.md) para o Azure das VMS no local.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Expandir o DFSR para a máquina virtual IaaS do Azure

1. Estabelece uma ligação de VPN de site a site entre o site no local e a rede do Azure. 
2. Expanda o Active Directory no local.
3. [Criar e aprovisionar um servidor de ficheiros VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) na rede virtual do Azure.
Certifique-se de que a máquina virtual é adicionada à mesma rede virtual do Azure, que tem conectividade do cross com o ambiente no local. 
4. Instalar e [configurar o DFSR](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) no Windows Server.
5. [Implementar um espaço de nomes do DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. Com o espaço de nomes DFS implementado, a ativação pós-falha de pastas compartilhadas da produção para sites de recuperação após desastre pode ser feito ao atualizar os destinos de pasta de espaço de nomes DFS. Depois destes DFS alterações de espaço de nomes são replicadas por meio do Active Directory, os utilizadores estão ligados forma transparente para os destinos de pasta apropriada.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>Utilizar a sincronização de ficheiros para replicar os ficheiros no local
Pode usar a sincronização de ficheiros para replicar arquivos para a cloud. Em caso de desastre e a indisponibilidade de seu servidor de ficheiros no local, pode montar as localizações de ficheiro pretendido da cloud e continuar a pedidos de serviço a partir de computadores cliente.
Para integrar o File Sync com o Site Recovery:

* Proteger máquinas do servidor de ficheiros ao utilizar o Site Recovery. Siga os passos em [deste tutorial](tutorial-vmware-to-azure.md).
* Utilize a sincronização de ficheiros para replicar os ficheiros da máquina que funciona como um servidor de ficheiros para a cloud.
* Utilize a funcionalidade de plano de recuperação no Site Recovery para adicionar scripts para montar a partilha de ficheiros do Azure no servidor de ficheiros com ativação pós-falha de VM no Azure.

Siga estes passos para utilizar a sincronização de ficheiros:

1. [Criar uma conta de armazenamento no Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Se tiver escolhido o armazenamento georredundante de acesso de leitura (recomendado) para as suas contas de armazenamento, terá acesso de leitura aos seus dados da região secundária em caso de desastre. Para obter mais informações, consulte [estratégias de recuperação de desastres de partilha de ficheiros do Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Criar uma partilha de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Implementar a sincronização de ficheiros](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) no seu servidor de ficheiros no local.
4. Crie um grupo de sincronização. Pontos finais dentro de um grupo de sincronização são mantidos em sincronia entre si. Um grupo de sincronização tem de conter pelo menos um ponto final da cloud, que representa uma partilha de ficheiros do Azure. O grupo de sincronização também tem de conter um ponto final do servidor, que representa um caminho no servidor de Windows no local.
5. Os ficheiros agora são mantidos em sincronia entre a partilha de ficheiros do Azure e o seu servidor no local.
6. Em caso de desastre no seu ambiente no local, execute uma ativação pós-falha ao utilizar um [plano de recuperação](site-recovery-create-recovery-plans.md). Adicione o script para montar a partilha de ficheiros do Azure e aceder à partilha na sua máquina virtual.

> [!NOTE]
> Certifique-se de que a porta 445 está aberta. Os ficheiros do Azure utiliza o protocolo SMB. SMB comunica através da porta TCP 445. Verifique se a firewall não está a bloquear a porta TCP 445 de uma máquina de cliente.


## <a name="do-a-test-failover"></a>Fazer uma ativação pós-falha de teste

1. Vá ao portal do Azure e selecione o Cofre de serviço de recuperação.
2. Selecione o plano de recuperação criado para o ambiente de servidor de ficheiros.
3. Selecione **ativação pós-falha de teste**.
4. Selecione o ponto de recuperação e a rede virtual do Azure para iniciar o processo de ativação pós-falha de teste.
5. Depois do ambiente secundário está ativo, efetue as validações.
6. Depois das validações terminar, selecione **ativação pós-falha de teste de limpeza** no plano de recuperação e a ativação pós-falha de teste é limpa o ambiente.

Para obter mais informações sobre como efetuar uma ativação pós-falha de teste, consulte [testar a ativação pós-falha para o Site Recovery](site-recovery-test-failover-to-azure.md).

Para obter orientações sobre a fazer a ativação pós-falha de teste do Active Directory e DNS, consulte [considerações de ativação pós-falha do Active Directory e DNS teste](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Fazer uma ativação pós-falha

1. Vá ao portal do Azure e selecione o Cofre de serviços de recuperação.
2. Selecione o plano de recuperação criado para o ambiente de servidor de ficheiros.
3. Selecione **ativação pós-falha**.
4. Selecione o ponto de recuperação para iniciar o processo de ativação pós-falha.

Para obter mais informações sobre como efetuar uma ativação pós-falha, consulte [ativação pós-falha no Site Recovery](site-recovery-failover.md).
