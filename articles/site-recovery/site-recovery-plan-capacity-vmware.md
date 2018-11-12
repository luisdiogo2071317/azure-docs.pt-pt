---
title: Planear a capacidade e dimensionamento para recuperação de desastre do VMware para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Utilize este artigo para planear a capacidade e dimensionamento ao configurar a recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 10/28/2018
ms.topic: conceptual
ms.author: nisoneji
ms.openlocfilehash: c6ec47017d944a206642932223ebb1d9df9cf942
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011736"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Planear a capacidade e dimensionamento para recuperação de desastre do VMware para o Azure

Utilize este artigo para descobrir a planear a capacidade e dimensionamento, ao replicar VMs de VMware no local e servidores físicos para o Azure com [do Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Como iniciar planeamento de capacidade?

Recolher informações sobre o ambiente de replicação ao executar o [do Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner-doc) para a replicação de VMware. [Saiba mais](site-recovery-deployment-planner.md) sobre esta ferramenta. Irá recolher informações sobre VMs compatíveis e incompatíveis, discos por VM, e alterações a dados por disco. A ferramenta também abarca os requisitos de largura de banda da rede e a infraestrutura do Azure necessários para uma replicação e ativação pós-falha de teste bem-sucedidas.

## <a name="capacity-considerations"></a>Considerações sobre capacidade

**Componente** | **Detalhes** |
--- | --- | ---
**Replicação** | **Taxa de alteração diária máximo:** uma máquina protegida só pode utilizar um servidor de processos e um servidor de processos único pode lidar com um valor diário máximo de 2 TB de taxa de alteração. Portanto, a 2 TB é que os máxima diários de dados alterar a velocidade a que é suportada para uma máquina protegida.<br/><br/> **Débito máximo:** uma máquina replicada pode pertencer a uma conta de armazenamento no Azure. Uma conta de armazenamento standard pode processar um máximo de 20 000 pedidos por segundo, e recomendamos que mantenha o número de operações de entrada/saída por segundo (IOPS) numa máquina de origem a 20 000. Por exemplo, se tiver uma máquina de origem com 5 discos, e cada disco gera 120 IOPS (8K de tamanho) na máquina de origem, em seguida, ele será dentro do Azure por limite de IOPS de disco de 500. (O número de contas de armazenamento necessárias é igual à máquina de origem total IOPS, dividida por 20.000.)
**Servidor de configuração** | O servidor de configuração deve ser capaz de lidar com a capacidade de taxa de alteração diária em todas as cargas de trabalho em execução nas máquinas protegidas e precisa a largura de banda suficiente para replicar continuamente os dados ao armazenamento do Azure.<br/><br/> Como melhor prática, localize o servidor de configuração na mesma rede e segmento de LAN que as máquinas que pretende proteger. Ele pode estar localizado numa rede diferente, mas as máquinas que pretende proteger devem ter visibilidade da camada 3 de rede ao mesmo.<br/><br/> Recomendações de tamanho para o servidor de configuração estão resumidas na tabela na secção seguinte.
**Servidor de processos** | O primeiro servidor de processos está instalado por predefinição no servidor de configuração. Pode implementar servidores de processos adicionais para dimensionar o seu ambiente. <br/><br/> O servidor de processos recebe dados de replicação de máquinas protegidas e otimiza-os com colocação em cache, compressão e encriptação. Em seguida, envia os dados para o Azure. A máquina do servidor de processo deve ter recursos suficientes para executar estas tarefas.<br/><br/> O servidor de processo utiliza um cache em disco. Utilize um disco de cache separado de 600 GB ou mais para processar alterações de dados armazenadas em caso de um afunilamento de rede ou interrupções.

## <a name="size-recommendations-for-the-configuration-server"></a>Recomendações de tamanho para o servidor de configuração

**CPU** | **Memória** | **Tamanho do disco de cache** | **Taxa de alteração de dados** | **Máquinas protegidas**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 núcleos \@ 2,5 gigahertz [GHz]) | 16 GB | 300 GB | 500 GB ou menos | Replicar máquinas inferior a 100.
12 vCPUs (2 sockets * 6 núcleos \@ 2,5 GHz) | 18 GB | 600 GB | 500 GB a 1 TB | Replicar entre 100 150 máquinas.
16 vCPUs (2 sockets * 8 núcleos \@ 2,5 GHz) | 32 GB | 1 TB | 1 TB para 2 TB | Replicar entre 150 200 máquinas.
Implementar outro servidor de processos | | | &GT; 2 TB | Implemente servidores de processos adicionais, se estiver a replicar mais de 200 máquinas ou, se alterar os dados diários de taxa superior a 2 TB.

Em que:

* Cada máquina de origem está configurada com 3 discos de 100 GB cada.
* Nós usamos armazenamento benchmark de 8 unidades SAS de 10 mil RPM, com o RAID 10, para medidas de disco de cache.

## <a name="size-recommendations-for-the-process-server"></a>Recomendações de tamanho para o servidor de processos

Se precisar de proteger mais de 200 máquinas ou a taxa de alteração diária for maior do que 2 TB, pode adicionar servidores de processos para processar a carga de replicação. Para aumentar horizontalmente, pode:

* Aumente o número de servidores de configuração. Por exemplo, pode proteger até 400 máquinas com dois servidores de configuração.
* Adicionar mais servidores de processos e utilizá-las para processar o tráfego em vez de (ou além de) o servidor de configuração.

A tabela seguinte descreve um cenário em que:

* Não estiver a planear utilizar o servidor de configuração como um servidor de processos.
* Que tiver configurado a um servidor de processos adicionais.
* Configurar máquinas virtuais protegidas para utilizar o servidor de processos adicionais.
* Cada máquina de origem protegida está configurada com três discos de 100 GB cada.

**Servidor de configuração** | **Servidor de processos adicionais** | **Tamanho do disco de cache** | **Taxa de alteração de dados** | **Máquinas protegidas**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 núcleos \@ 2.5ghz), 16 GB de memória | 4 vCPUs (2 sockets * 2 núcleos \@ 2.5ghz), 8 GB de memória | 300 GB | 250 GB ou menos | Replicar máquinas 85 ou menos.
8 vCPUs (2 sockets * 4 núcleos \@ 2.5ghz), 16 GB de memória | 8 vCPUs (2 sockets * 4 núcleos \@ 2.5ghz), 12 GB de memória | 600 GB | De 250 GB a 1 TB | Replicar entre máquinas de 85 150.
12 vCPUs (2 sockets * 6 núcleos \@ 2.5ghz), 18 GB de memória | 12 vCPUs (2 sockets * 6 núcleos \@ 2.5ghz) 24 GB de memória | 1 TB | 1 TB para 2 TB | Replicar entre 150 225 máquinas.

A maneira na qual dimensiona os seus servidores depende da sua preferência num modelo de aumento vertical ou horizontal.  Aumentar verticalmente ao implementar alguns ponta configuration e servidores de processos ou aumentar horizontalmente, ao implementar mais servidores com menos recursos. Por exemplo, se precisar de proteger 220 máquinas, o, pode efetuar uma das seguintes ações:

* Configure o servidor de configuração com 12 vCPU, 18 GB de memória e um servidor de processos adicionais com 12 vCPU, 24 GB de memória. Configure máquinas protegidas para utilizar apenas o servidor de processos adicionais.
* Configure dois servidores de configuração (2 x 8 Vcpus, 16 GB de RAM) e dois servidores de processos adicionais (vCPU de 1 x 8 e 4 máquinas de vCPU x 1 para identificador 135 + 85 [220]). Configure máquinas protegidas para utilizar apenas os servidores de processos adicionais.


## <a name="control-network-bandwidth"></a>Largura de banda de rede de controlo

Depois de usar o [a ferramenta Deployment Planner](site-recovery-deployment-planner.md) para calcular a largura de banda precisa para a replicação (replicação inicial e, em seguida, delta), pode controlar a quantidade de largura de banda utilizada na replicação através de duas opções:

* **Limitar largura de banda**: tráfego de VMware que replica para o Azure passa através de um servidor de processo específico. Pode limitar a largura de banda nas máquinas em execução como servidores de processos.
* **Influenciar a largura de banda**: pode influenciar a largura de banda utilizada para replicação utilizando duas chaves de registo:
  * O **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** valor de registo Especifica o número de threads que são utilizados para a transferência de dados (replicação inicial ou delta) de um disco. Um valor mais alto aumenta a largura de banda de rede utilizada na replicação.
  * O **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** Especifica o número de threads utilizado para transferência de dados durante a reativação pós-falha.

### <a name="throttle-bandwidth"></a>Limitar largura de banda

1. Abra o snap-in MMC de cópia de segurança do Azure na máquina que atua como o servidor de processos. Por predefinição, um atalho para cópia de segurança está disponível no ambiente de trabalho ou na seguinte pasta: C:\Program Files\Microsoft Azure Recovery Services agent\bin\wabadmin.
2. No snap-in, clique em **Alterar Propriedades**.

    ![Captura de ecrã do Azure Backupna MMC snap-in de opção para alterar as propriedades](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Sobre o **limitação** separador, selecione **ativar a limitação para operações de cópia de segurança de utilização de largura de banda de internet**. Defina os limites para o trabalho e de descanso. Intervalos válidos são entre 512 Kbps 1023 Mbps por segundo.

    ![Caixa de diálogo de captura de ecrã de propriedades de cópia de segurança do Azure](./media/site-recovery-vmware-to-azure/throttle2.png)

Também pode utilizar o cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) para configurar a limitação. Exemplo:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** indica que não é necessária nenhuma limitação.

### <a name="influence-network-bandwidth-for-a-vm"></a>Influenciar a largura de banda de rede para uma VM

1. No registo da VM, navegue até **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Para influenciar o tráfego de largura de banda num disco de replicação, modifique o valor de **UploadThreadsPerVM**, ou crie a chave, se não existir.
   * Para influenciar a largura de banda para o tráfego de reativação pós-falha do Azure, modifique o valor de **DownloadThreadsPerVM**.
2. O valor predefinido é 4. Numa rede “sobreaprovisionada”, os valores predefinidos destas chaves de registo devem ser alterados. O valor máximo é 32. Monitorize o tráfego para otimizar o valor.


## <a name="deploy-additional-process-servers"></a>Implementar servidores de processos adicionais

Se tiver de dimensionar fora de sua implantação, além de 200 máquinas de origem, ou tem um total de alterações diariamente a taxa de mais de 2 TB, terá de servidores de processos adicionais para processar o volume de tráfego. Siga as instruções dadas no [este artigo](vmware-azure-set-up-process-server-scale.md) para configurar o servidor de processos. Depois de configurar o servidor, pode migrar máquinas de origem para utilizá-lo.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrar máquinas para utilizar o novo servidor de processos

1. Na **configurações** > **servidores de Site Recovery**, clique no servidor de configuração e, em seguida, expanda **processar servidores**.

    ![Caixa de diálogo de captura de ecrã do servidor de processos](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. O servidor de processos atualmente em utilização com o botão direito e clique em **comutador**.

    ![Caixa de diálogo de servidor de captura de ecrã de configuração](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. Na **servidor de processos de destino selecione**, selecione o novo servidor de processos que pretende utilizar e, em seguida, selecione as máquinas virtuais que o servidor irá processar. Clique no ícone de informações para obter informações sobre o servidor. Para ajudar a tomar decisões de carga, é apresentado o espaço médio necessário para replicar a cada máquina virtual selecionada para o novo servidor de processos. Clique na marca de verificação para iniciar a replicação para o novo servidor de processo.

## <a name="deploy-additional-master-target-servers"></a>Implementar servidores de destino mestre adicionais

Terá de servidor de destino mestre adicionais durante os seguintes cenários

1. Se estiver a tentar proteger uma máquina de virtual baseado em Linux.
2. Se o servidor de destino principal disponível no servidor de configuração não tem acesso para o arquivo de dados da VM.
3. Se o número total de discos no mestre de servidor de destino (não. de discos locais no servidor) + discos a proteger excede 60 discos.

Para adicionar um novo servidor de destino mestre para **máquina virtual baseada no Linux**, [clique aqui](vmware-azure-install-linux-master-target.md).

Para **máquina virtual baseada no Windows**, siga o abaixo dado instruções.

1. Navegue para **cofre dos serviços de recuperação** > **infraestrutura do Site Recovery** > **servidores de configuração**.
2. Clique no servidor de configuração necessárias > **+ servidor de destino mestre**.![ Adicionar-master-destino-server.png](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Transferir a configuração unificada e executá-lo na VM para configurar o servidor de destino mestre.
4. Escolher **instalação de destino mestre** > **seguinte**. ![MT.PNG escolha](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Escolha a localização de instalação predefinida > clique em **instalar**. ![Instalação de MT](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Clique em **continuar a configuração** para registar o destino principal com o servidor de configuração. ![Configuration.PNG MT continuar](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Introduza o endereço IP do servidor de configuração & frase de acesso. [Clique aqui](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) para saber como gerar frase de acesso.![ CS-ip-frase de acesso](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Clique em **registar** e clique em registo de postagem **concluir**.
9. Após o registo com êxito, este servidor está listado no portal, em **cofre dos serviços de recuperação** > **infraestrutura do Site Recovery** > **configuração servidores** > servidores de destino do servidor de configuração relevantes mestre.

 >[!NOTE]
 >Também pode transferir a versão mais recente do mestre de configuração unificada do servidor de destino para Windows [aqui](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Passos Seguintes

Transfira e execute o [do Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner)
