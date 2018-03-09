---
title: Proteja as VMs do Azure para um site no local | Microsoft Docs
description: "Após a ativação pós-falha de VMs para o Azure, pode iniciar uma reativação pós-falha para colocar as VMs no local. Saiba como voltar a proteger antes de uma reativação pós-falha."
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanaki
ms.openlocfilehash: cd5e53b49a850acf851e8351b5e14e2993176435
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="reprotect-machines-from-azure-to-an-on-premises-site"></a>Proteja máquinas do Azure para um site no local

Depois de [ativação pós-falha](site-recovery-failover.md) de VMs de VMware no local ou servidores físicos para o Azure, o primeiro passo na falhar novamente para o site no local é voltar a proteger as VMs do Azure que criou durante a ativação pós-falha. Este artigo descreve como efetuar esta ação. 

Para uma descrição geral, veja o seguinte vídeo sobre como efetuar a ativação pós-falha do Azure para um site no local.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="before-you-start"></a>Antes de começar

Se utilizou um modelo para criar as máquinas virtuais, certifique-se de que cada máquina virtual tem o seu próprio UUID de discos. Se o UUID no local da máquina de virtual entra em conflito com que o destino principal porque ambos foram criadas a partir do mesmo modelo, irá falhar. Implemente outro destino mestre não tiver sido criado a partir do mesmo modelo.
- Se estiver a tentar para a reativação pós-falha um vCenter alternativo, certifique-se de que o seu novo vCenter e o servidor de destino principal são detetados. Um típico sintoma é que o datastores não são acessíveis ou visível no **Proteja** caixa de diálogo.
- Replicar para o local, terá de uma política de reativação pós-falha. Esta política obter criada automaticamente quando criar uma política de direção direta. Tenha em atenção que:
    - Esta política obtém automaticamente associado ao servidor de configuração durante a criação.
    - Esta política não é editável.
    - Os valores do conjunto da política são (limiar RPO = 15 min, retenção do ponto de recuperação = 24 horas, frequência de instantâneos de consistência de aplicações = 60 minutos)
- Durante a voltar e a reativação pós-falha, o servidor de configuração no local deve estar em execução e ligado.
- Se um servidor vCenter gere as máquinas virtuais para que irá falhar novamente Certifique-se de que tem o [as permissões necessárias](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) para a deteção de VMs nos servidores do vCenter.
- Elimine os instantâneos no servidor de destino principal antes de a voltar a proteger. Se existirem instantâneos no destino principal no local ou a máquina virtual, irá falhar. Os instantâneos da máquina virtual são intercalados automaticamente durante uma tarefa de reproteção.
- Todas as máquinas virtuais de um grupo de replicação deve ser do mesmo tipo de sistema operativo (todos os Windows ou Linux todas as). Um grupo de replicação com sistemas de operativos mistos não é atualmente suportado para reproteção e a reativação pós-falha no local. Isto acontece porque o destino principal deve ser do mesmo sistema operativo que a máquina virtual e todas as máquinas virtuais de um grupo de replicação deve ter o mesmo destino mestre. 
- Um servidor de configuração é necessário no local ao efetuar a cópia. Durante a reativação pós-falha, a máquina virtual tem de existir na base de dados de servidor de configuração. Caso contrário, a reativação pós-falha é bem-sucedida. Certifique-se de que efetua cópias de segurança agendadas regularmente do seu servidor de configuração. Se ocorrer um desastre, restaure o servidor com o mesmo endereço IP, de modo a que funciona de reativação pós-falha.
- Só e a reativação pós-falha requerem uma VPN de site a site replicar os dados. Forneça a rede para que as máquinas virtuais de efetuada a ativação pós-falha no Azure pode alcançar o servidor de configuração no local (ping). Também pode implementar um servidor de processos na rede da máquina virtual efetuada a ativação pós-falha do Azure. Este servidor de processos deve também conseguir comunicar com o servidor de configuração no local.
- Certifique-se de que abrir as portas seguintes para ativação pós-falha e a reativação pós-falha.

    ![Portas para ativação pós-falha e a reativação pós-falha](./media/vmware-azure-reprotect/failover-failback.png)

## <a name="deploy-a-process-server-in-azure"></a>Implementar um servidor de processos no Azure

Poderá ter um servidor de processos no Azure antes de falhar novamente para o site no local:
- O servidor de processo] recebe dados de máquina virtual protegida no Azure e envia dados para o site no local.
- Uma rede de latência baixa é necessária entre o servidor de processos e a máquina virtual protegida. Em geral, precisa de considerar a latência ao decidir se é preciso um servidor de processos no Azure:
    - Se tiver configurada uma ligação do ExpressRoute, pode utilizar um servidor de processos no local para enviar dados porque a latência entre a máquina virtual e o servidor de processos é baixa.
    - No entanto, se tiver apenas uma VPN S2S, recomendamos que implementar o servidor de processos no Azure.
    - Recomendamos que utilize um servidor de processos baseado no Azure durante a reativação pós-falha. O desempenho de replicação é superior se o servidor de processos estiver próximo da máquina virtual em replicação (a máquina de efetuada a ativação pós-falha no Azure). Para um prova-do-conceito, pode utilizar o servidor de processos no local, juntamente com o ExpressRoute com o peering privado.

Implemente da seguinte forma:

1. Se precisar de implementar um servidor de processos no Azure, siga [estas instruções](vmware-azure-set-up-process-server-azure.md)
2. As VMs do Azure irá enviar dados de replicação para o servidor de processos. Configure redes de modo a que as VMs do Azure podem aceder ao servidor de processo.
3. Lembre-se de que a replicação a partir do Azure no local pode acontecer apenas através de S2S VPN ou através do peering privado da sua rede de ExpressRoute. Certifique-se de que a largura de banda suficiente está disponível através do canal essa rede.

## <a name="deploy-a-separate-master-target-server"></a>Implementar um servidor de destino principal separado

O servidor de destino mestre recebe dados da reativação pós-falha. Por predefinição, o servidor de destino mestre é executado no servidor de configuração no local. No entanto, consoante o volume de tráfego de cópia falhou, poderá ter de criar um servidor de destino principal separado para reativação pós-falha. Eis como criar um:

    * [Criar um servidor de destino principal do Linux](vmware-azure-install-linux-master-target.md) para reativação pós-falha de VMs do Linux. Isto é necessário.
    * Opcionalmente, crie um servidor de destino principal separado para reativação pós-falha da VM do Windows. Para tal, execute novamente a configuração do Unified e selecione criar um servidor de destino principal. [Saiba mais](physical-azure-set-up-source.md#run-azure-site-recovery-unified-setup).

Depois de criar um servidor de destino mestre, efetue o seguinte:

- Se a máquina virtual está presente no local no vCenter server, o servidor de destino mestre precisa de acesso à VMDK da máquina de virtual no local. É necessário acesso ao escrever os dados replicados para discos da máquina virtual. Certifique-se de que o arquivo de dados da máquina de virtual no local está montado no anfitrião de destino principal com acesso de leitura/escrita.
- Se a máquina virtual não está presente no local no vCenter server, tem de criar uma nova máquina virtual durante a o serviço de recuperação de Site. Esta máquina virtual é criada no anfitrião do ESX no qual criar o destino principal. Escolha o anfitrião do ESX cuidadosamente, para que a máquina virtual de reativação pós-falha é criada no anfitrião que pretende.
- Não é possível utilizar vMotion de armazenamento para o servidor de destino mestre *. Isto pode provocar a reativação pós-falha falhou. Não é possível iniciar a máquina virtual porque os discos não estão disponíveis. Para evitar este problema, exclua os servidores de destino principal da sua lista de vMotion.
- Se um destino principal sofre uma tarefa de vMotion de armazenamento após só, os discos de máquinas virtuais protegidas que estão anexados ao destino principal migrar para o destino da tarefa vMotion. Se tentar falhar novamente após este, Desanexação do disco falha porque os discos não forem encontrados. Em seguida, torna-se difícil localizar os discos nas suas contas do storage. Terá de encontrá-los manualmente e anexe-os para a máquina virtual. Depois disso, a máquina virtual no local pode ser reiniciada.
- Adicione uma unidade de retenção para o servidor de destino principal existente do Windows. Adicione um novo disco e formatar o disco. A unidade de retenção é utilizada para parar os pontos no tempo quando a máquina virtual são replicados para o site no local. Seguem-se alguns critérios de uma unidade de retenção. Sem estes critérios, a unidade não será listada para o servidor de destino principal.
    - O volume não é utilizado para qualquer outra finalidade, tais como um destino de replicação.
    - O volume não está no modo de bloqueio.
    - O volume não é um volume de cache. A instalação de destino mestre não deve existir nesse volume. O volume de instalação personalizada para o destino de servidor e o mestre de processo não é elegível para um volume de retenção. Quando o servidor de processos e o destino principal estão instalados num volume, o volume é um volume de cache de destino principal.
    - O tipo de sistema de ficheiros do volume não é FAT ou FAT32.
    - A capacidade do volume for diferente de zero.
    - O volume de retenção predefinido para o Windows é o volume de R.
    - O volume de retenção predefinido para Linux é /mnt/retention.
- Tem de adicionar uma nova unidade se estiver a utilizar uma máquina de servidor de configuração do servidor de processo existente ou uma escala ou um computador de servidor de destino do processo servidor/principal. A nova unidade deve satisfazer os requisitos de anteriores. Se a unidade de retenção não estiver presente, não aparecem na lista pendente de seleção no portal. Depois de adicionar uma unidade para o destino principal no local, que demora até 15 minutos para a unidade a aparecer na seleção no portal. Também pode atualizar o servidor de configuração se a unidade não aparecer após 15 minutos.
- Instale as ferramentas do VMware no servidor de destino mestre. Sem as ferramentas do VMware, não é possível detetar o datastores no anfitrião ESXi do destino principal.
- Definir o `disk.EnableUUID=true` definição os parâmetros de configuração da máquina virtual destino principal no VMware. Se esta linha não existir, adicioná-lo. Esta definição é necessário para fornecer um UUID consistente para o disco de máquina virtual (VMDK) para que o se monta corretamente.
- O destino principal deve ter pelo menos um arquivo de dados VMFS ligado. Se não houver nenhum, o **arquivo de dados** entrada na página de reproteção estará vazia, e não é possível continuar.
- O servidor de destino principal não pode ter instantâneos de discos. Se existirem instantâneos, só e a reativação pós-falha falharem.
- O destino principal não pode ter um controlador de Paravirtual SCSI. O controlador só pode ser um controlador de LSI Logic. Sem um controlador de LSI Logic só irá falhar.
- Em qualquer instância especificada, o destino principal pode ter Step-by-atmst 60 discos ligados ao mesmo. Se o número de máquinas virtuais que está a ser proteger ao destino principal no local têm um número total de soma de mais de 60 discos, em seguida, reprotects ao destino principal começarão a falhar. Certifique-se de que tem suficiente mestre ranhuras de disco de destino ou implementar servidores de destino mestre adicionais.
    

## <a name="enable-reprotection"></a>Ativar só

Depois de uma máquina virtual arranca no Azure, demora algum tempo para o agente registar novamente para o servidor de configuração (até 15 minutos). Durante este tempo, não poderá voltar a proteger e uma mensagem de erro indica que o agente não está instalado. Se isto acontecer, aguarde alguns minutos e, em seguida, tente voltar novamente.


1. No **cofre** > **replicado itens**, faça duplo clique na máquina virtual que é foi efetuada a ativação pós-falha e, em seguida, selecione **voltar a proteger**. Também pode clique na máquina e selecione **voltar a proteger** de botões de comando.
2. Certifique-se a direção da proteção, **do Azure para o local**, já está selecionada.
3. No **servidor de destino mestre** e **servidor de processos**, selecione o servidor de destino principal no local e o servidor de processos.
4. Para **arquivo de dados**, selecione o arquivo de dados para o qual pretende recuperar o discos no local. Esta opção é utilizada quando a máquina virtual no local é eliminada e tem de criar novos discos. Esta opção é ignorada se os discos já existem, mas ainda tem de especificar um valor.
5. Escolha a unidade de retenção.
6. A política de reativação pós-falha é selecionada automaticamente.
7. Clique em **OK** para começar a nova proteção. Um trabalho começa a replicar a máquina virtual do Azure para o site no local. Pode acompanhar o progresso no separador **Trabalhos**. Depois do que for bem sucedida, a máquina virtual entrará num estado protegido.

Tenha em atenção que:
- Se pretender recuperar para uma localização alternativa (quando a máquina virtual no local é eliminada), selecione a unidade de retenção e arquivo de dados que estão configurados para o servidor de destino principal. Quando houver uma para o site no local, as máquinas virtuais de VMware no plano de proteção de reativação pós-falha utilizar o mesmo arquivo de dados como servidor de destino mestre. Uma nova máquina virtual, em seguida, é criada no vCenter.
- Se pretender recuperar a máquina virtual no Azure para uma máquina virtual no local, monte datastores da máquina de virtual no local com acesso de leitura/escrita no anfitrião ESXi de servidor de destino mestre.
    ![Caixa de diálogo de voltar a proteger](./media/vmware-azure-reprotect/reprotectinputs.png)

- Também pode voltar a proteger no nível de um plano de recuperação. Um grupo de replicação pode proteger apenas através de um plano de recuperação. Quando voltar a proteger através da utilização de um plano de recuperação, tem de fornecer os valores para cada máquina protegida.
- Utilize o mesmo servidor de destino mestre para voltar a proteger um grupo de replicação. Se utilizar um servidor de destino mestre diferente para voltar a proteger um grupo de replicação, o servidor não pode fornecer um ponto no tempo comum.
- As máquinas no local é desativada durante só. Desta forma, garante-se a consistência dos dados durante a replicação. Não ative a máquina virtual após a conclusão da só.



## <a name="common-issues"></a>Problemas comuns


- Atualmente, a recuperação de sites suporta a falhar back apenas a um sistema de ficheiros de máquina virtual (VMFS) ou o arquivo de dados de vSAN. Não é suportado um arquivo de dados do NFS. Devido a esta limitação, a seleção de arquivo de dados de entrada de reproteção ecrã está vazio no caso de NFS datastores ou que mostra o arquivo de dados de vSAN, mas ocorre uma falha durante a tarefa. Se tenciona voltar a falhar, pode criar um arquivo de dados VMFS no local e não-lo. Este reativação pós-falha fará com que uma transferência completa da VMDK.
- Se executar uma deteção de utilizadores de só de leitura do vCenter e proteger máquinas virtuais, proteção com êxito e a ativação pós-falha funciona. Que, durante a ativação pós-falha falha porque não é possível detetar o datastores. Um sintoma é que o datastores não estão listados durante só. Para resolver este problema, pode atualizar a credencial do vCenter com uma conta apropriada que tenha permissões e repita a tarefa. 
- Quando uma máquina virtual Linux a efetuar pós-falha e executá-la no local, pode ver que o pacote do Gestor de rede foi desinstalado do computador. Esta desinstalação acontece porque o pacote do Gestor de rede é removido quando a máquina virtual é recuperada no Azure.
- Quando uma máquina virtual Linux está configurada com um endereço IP estático e ativação pós-falha do Azure, o endereço IP é adquirido de DHCP. Quando efetuar a ativação pós-falha no local, a máquina virtual continua a utilizar DHCP para adquirir o endereço IP. Manualmente iniciar sessão máquina e definir o endereço IP para um endereço estático se necessário. Máquina virtual do Windows pode adquirir novamente o IP estático.
- Se utilizar a edição gratuita ESXi 5.5 ou edição gratuita do hipervisor vSphere 6, ativação pós-falha é bem-sucedida, mas não ativar a reativação pós-falha. Para ativar a reativação pós-falha, atualize a licença de avaliação de um programa.
- Se não conseguirem contactar o servidor de configuração do servidor de processos, utilize o Telnet para verificar a conectividade ao servidor de configuração na porta 443. Também pode tentar executar o ping do servidor de configuração do servidor de processos. Um servidor de processos também deve ter um heartbeat quando está ligado ao servidor de configuração.
- Um servidor do Windows Server 2008 R2 SP1 que está protegido como um servidor físico no local não pode ser falhado novamente a partir do Azure para um site no local.
- Não pode falhar novamente nas seguintes circunstâncias:
    - As máquinas são migradas para o Azure. [Saiba mais](migrate-overview.md#what-do-we-mean-by-migration).
    - Mover uma VM para outro grupo de recursos.
    - Eliminar a VM do Azure.
    - Desativar proteção da VM.
    - Criar manualmente a VM no Azure. A máquina deve ter sido protegido inicialmente no local e a ativação pós-falha para o Azure antes de reproteção.
    - Apenas pode proceder para um anfitrião ESXi. Não é possível a reativação pós-falha VMs de VMware ou servidores físicos como anfitriões Hyper-V, máquinas físicas ou estações de trabalho VMware.


## <a name="next-steps"></a>Passos Seguintes

Depois da máquina virtual entrou num estado protegido, pode [iniciar uma reativação pós-falha](vmware-azure-failback.md). A reativação pós-falha irá encerrar a máquina virtual no Azure e efetuar o arranque de máquina virtual no local. Prevê algum período de indisponibilidade para a aplicação. Escolha uma hora de reativação pós-falha quando a aplicação pode tolerar o período de indisponibilidade.

