---
title: Migrar VMs do AWS para o Azure com o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como migrar VMs em execução no Amazon Web Services (AWS) para o Azure com o Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
editor: ''
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9b1aabcd88ce93ff9316d3ee99619eccaeb6a38e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
ms.locfileid: "29677449"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrar VMs do Amazon Web Services (AWS) para o Azure

Este tutorial ensina-lhe a migrar máquinas virtuais (VMs) do Amazon Web Services (AWS) para VMs do Azure com o Site Recovery. Quando migrar instâncias de EC2 para o Azure, VMsare tratado como se físico, computadores no local. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Preparar recursos do Azure
> * Preparar as instâncias do EC2 do AWS para migração
> * Implementar um servidor de configuração
> * Ativar a replicação nas VMs
> * Testar a ativação pós-falha para confirmar que está tudo a confirmar
> * Executar uma ativação pós-falha única para o Azure

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.


## <a name="prepare-azure-resources"></a>Preparar recursos do Azure 

Tem de ter alguns recursos prontos no Azure para serem utilizados pelas instâncias do EC2 migradas. Esses recursos incluem uma conta de armazenamento, um cofre e uma rede virtual.

### <a name="create-a-storage-account"></a>Criar uma conta do Storage

As imagens das máquinas replicadas são guardadas no armazenamento do Azure. As VMs do Azure são criadas a partir do armazenamento quando fizer a ativação pós-falha do local para o Azure.

1. No menu do [portal do Azure](https://portal.azure.com), clique em **Criar um recurso** -> **Armazenamento** -> **Conta de armazenamento**.
2. Introduza um nome para a conta do Storage. Nestes tutoriais, vamos utilizar o nome **awsmigrated2017**. O nome tem de ser exclusivo no Azure e ter entre 3 e 24 carateres compostos apenas por números e letras minúsculas.
3. Mantenha as predefinições em **Modelo de implementação**, **Tipo de conta**, **Desempenho** e **Transferência segura necessária**.
5. Selecione a predefinição **RA-GRS** em **Replicação**.
6. Selecione a subscrição que pretende utilizar neste tutorial.
7. Em **Grupo de recursos**, selecione **Criar novo**. Neste exemplo, vamos utilizar **migrationRG** como o nome.
8. Selecione **Europa Ocidental** como a localização. 
9. Clique em **Criar** para criar a conta do Storage.

### <a name="create-a-vault"></a>Criar um cofre

1. No [portal do Azure](https://portal.azure.com), na navegação do lado esquerdo, clique em **Mais serviços** e procure e selecione **Cofres dos Serviços de Recuperação**.
2. Na página Cofres dos Serviços de Recuperação, clique em **+ Adicionar**, no canto superior esquerdo.
3. Em **Nome**, escreva *myVault*. 
4. Em **Subscrição**, selecione a subscrição adequada.
4. Em **Grupo de recursos**, selecione **Utilizar existente** e *migrationRG*. 
5. Em **Localização**, selecione *Europa Ocidental*. 
5. Para aceder rapidamente ao cofre novo a partir do dashboard, selecione **Afixar ao dashboard**.
7. Quando tiver concluído, clique em **Criar**.

O cofre novo é apresentado em **Dashboard** > **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.

### <a name="set-up-an-azure-network"></a>Configurar uma rede do Azure

Quando as VMs do Azure são criadas após a migração (ativação pós-falha), são associadas a esta rede.

1. No [portal do Azure](https://portal.azure.com), clique em **crie um recurso** > **redes** >
    **rede Virtual**
3. Em **Nome**, escreva *myMigrationNetwork*.
4. Deixe o valor predefinido em **Espaço de endereços**.
5. Em **Subscrição**, selecione a subscrição adequada.
6. Em **Grupo de recursos**, selecione **Utilizar existente** e escolha *migrationRG* no menu pendente.
7. Em **Localização**, selecione **Europa Ocidental**. 
8. Deixe as predefinições para **Sub-rede**, tanto o **Nome**, como o **Intervalo de IP**.
9. Deixe os **Pontos Finais do Serviço** desativados.
10. Quando tiver concluído, clique em **Criar**.


## <a name="prepare-the-ec2-instances"></a>Preparar as instâncias EC2

Precisa de uma ou mais VMs que queira migrar. Estas instâncias EC2 devem estar a executar a versão de 64 bits do Windows Server 2008 R2 SP1 ou posterior, do Windows Server 2012, do Windows Server 2012 R2 ou do Red Hat Enterprise Linux 6.7 (apenas instâncias virtualizadas de HVM). O servidor só deve ter controladores Citrix PV ou AWS PV. As instâncias que executem controladores RedHAt PV não são suportadas.

O Serviço de mobilidade tem de ser instalado em cada VM que queira replicar. O Site Recovery instala este serviço automaticamente quando a replicação para a VM é ativada. Para a instalação automática, te de preparar uma conta nas instâncias EC2 que o Site Recovery vai utilizar para aceder à VM.

Pode utilizar um domínio ou uma conta local. Nas VMs do Linux, a conta deve ser a raiz no servidor do Linux de origem. Nas VMs do Windows, se não estiver a utilizar uma conta de domínio, desative o Controlo de Acesso de Utilizador Remoto no computador local:

  - No registo, em **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adicione a entrada DWORD **LocalAccountTokenFilterPolicy** e defina o valor como 1.
    
Também precisa de uma instância EC2 separada que possa ser utilizada como o servidor de configuração do Site Recovery. Essa instância deve executar o Window Server 2012 R2. 
    

## <a name="prepare-the-infrastructure"></a>Preparar a infraestrutura 

Na página do portal do seu cofre, selecione **Site Recovery**, na secção **Introdução**, e clique em **Preparar infraestrutura**.

### <a name="1-protection-goal"></a>1 Objetivo da proteção

Selecione os seguintes valores na página **Objetivo da proteção**:
   
|    |  | 
|---------|-----------|
| Onde estão localizadas as máquinas? | **No local**|
| Para onde pretende replicar as máquinas? |**Para o Azure**|
| As máquinas estão virtualizadas? | **Não virtualizadas / outra**|

Quando tiver terminado, clique em **OK** para avançar para a secção seguinte.

### <a name="2-source-prepare"></a>2 Preparar a origem 

Na página **Preparar origem**, clique em **+Servidor de Configuração**. 

1. Utilize uma instância do EC2 que execute o Windows Server 2012 R2 para criar um servidor de configuração e registá-lo no seu cofre de recuperação.

2. Configure o proxy na VM de instância do EC2 que está a utilizar como o servidor de configuração, para que possa aceder aos [URLs do serviço](../site-recovery-support-matrix-to-azure.md).

3. Transfira o programa [Microsoft Azure Site Recovery Unified Setup](http://aka.ms/unifiedinstaller_wus) (Configuração Unificada do Microsoft Azure Site Recovery). Pode transferi-lo para o computador local e, em seguida, copiá-lo para a VM que está a utilizar como o servidor de configuração.

4. Clique no botão **Transferir** para transferir a chave de registo do cofre. Copie o ficheiro transferido para a VM que está a utilizar como o servidor de configuração.

5. Na VM, clique com botão direito do rato no instalador que transferiu para o **Microsoft Azure Site Recovery Unified Setup** e selecione **Executar como administrador**. 

    1. Em **Antes de Começar**, selecione **Instalar o servidor de configuração e o servidor de processos** e clique em **Seguinte**.
    2. Em **Licença de Software de Terceiros**, selecione **Aceito o contrato de licença de terceiros.** e clique em **Seguinte**.
    3. Em **registo**, clique em Procurar e navegue para a localização onde pretende colocar o ficheiro da chave de registo do cofre e clique em **Seguinte**.
    4. Em **definições da Internet**, selecione **Ligar ao Azure Site Recovery sem um servidor proxy.** e clique em **Seguinte**.
    5. Na página **Verificação de Pré-requisitos**, são executadas verificações relativamente a vários itens. Quando as verificações terminarem, clique em **Seguinte**. 
    6. EM **Configuração de MySQ**, indique as palavras-passe necessárias e clique em **Seguinte**.
    7. Em **Detalhes do Ambiente**, selecione **Não**, não precisa de proteger máquinas do VMware e clique em **Seguinte**.
    8. Em **Localização da Instalação**, clique em **Seguinte** para aceitar a predefinição.
    9. Em **Seleção de Rede**, clique em **Seguinte** para aceitar a predefinição.
    10. Em **resumo**, clique em **Instalar**.
    11. O **Progresso da Instalação** mostra-lhe informações sobre em que ponto do processo de instalação está. Quando terminar, clique em **Concluído**. Vai receber uma mensagem de pop-up a dizer que poderá ser preciso reiniciar. Clique em **OK**. Também vai receber uma mensagem de pop-up sobre a Frase de Acesso da Ligação do Servidor de Configuração. Copie a frase de acesso para a área de transferência e guarde-a num sítio seguro.
    
6. Na VM, execute **cspsconfigtool.exe** para criar uma ou mais contas de gestão no servidor de configuração. Confirme que as contas de gestão têm permissões de administrador nas instâncias do EC2 que pretende migrar. 

Quando terminar de configurar o servidor de configuração, regresse ao portal, selecione o servidor que acabou de criar para **Servidor de Configuração** e clique em *OK** para avançar para o passo 3, Preparar o Destino.

### <a name="3-target-prepare"></a>3 Preparar o Destino 

Nesta secção, vai introduzir informações sobre os recurso que criou na secção [Preparar recursos do Azure](#prepare-azure-resources) mais atrás neste tutoria.

1. Em **Subscrição**, selecione a subscrição do Azure que utilizou no tutorial [Prepare Azure](../tutorial-prepare-azure.md) (Preparar o Azure).
2. Selecione **Resource Manager** como o modelo de implementação.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis. Estes devem ser os recursos que criou na secção [Preparar recursos do Azure](#prepare-azure-resources) mais atrás neste tutorial.
4. Quando tiver terminado, clique em **OK**.


### <a name="4-replication-settings-prepare"></a>4 Preparar as definições da replicação 

Antes de poder ativar a replicação, tem de criar uma política de replicação.

1. Clique em **+ Replicar e Associar**.
2. Em **Nome**, escreva **myReplicationPolicy**.
3. Deixe as restantes predefinições e clique em **OK** para criar a política. A política nova é associada automaticamente ao servidor de configuração. 

### <a name="5-deployment-planning-select"></a>5 Seleção do planeamento da implementação 

Em **Concluiu o planeamento da implementação?**, selecione **Faço mais tarde**, no menu pendente, e clique em **OK**.

Quando terminar as cinco secções de **Preparar a infraestrutura**, clique em **OK**.


## <a name="enable-replication"></a>Ativar a replicação

Ative a replicação em cada VM que quer migrar. Quando a replicação for ativada, o Site Recovery instala o Serviço de mobilidade automaticamente. 

1. Abra o [Portal do Azure](htts://portal.azure.com).
1. Na página do seu cofre, em **Introdução**, clique em **Site Recovery**.
2. Em **Em máquinas no local e em VMs do Azure**, clique em **Passo 1: Replicar aplicação**. Conclua as páginas do assistente com as seguintes informações e clique em **OK** em cada página, quando tiver concluído:
    - 1 Configuração da Origem:
      
    |  |  |
    |-----|-----|
    | Origem: | **No Local**|
    | Localização da origem:| O nome da instância do EC2 do servidor de configuração.|
    |Tipo de computador/máquina: | **Computadores**|
    | Servidor de processos: | Selecione o servidor de configuração na lista pendente.|
    
    - 2 Configuração do Destino
        
    |  |  |
    |-----|-----|
    | Destino: | Deixe a predefinição.|
    | Subscrição: | Selecione a subscrição que tem estado a utilizar.|
    | Grupo de recursos de ativação pós-falha:| Utilize o grupo de recursos que criou na secção [Preparar recursos do Azure](#prepare-azure-resources).|
    | Modelo de implementação de ativação pós-falha: | Escolha **Resource Manager**|
    | Conta de armazenamento: | Escolha a conta de armazenamento que criou na secção [Preparar recursos do Azure](#prepare-azure-resources).|
    | Rede do Azure: | Escolha **Configurar agora para as máquinas selecionadas**|
    | Rede do Azure de ativação pós-falha: | Escolha a rede que criou na secção [Preparar recursos do Azure](#prepare-azure-resources).|
    | Sub-rede: | Selecione a **predefinição** na lista pendente.|
    
    - 3 Seleção dos computadores
        
        Clique em **+ Computador**, introduza o **Nome**, o **Endereço IP** e o **Tipo de SO** da instância do EC2 que pretende migrar e clique em **OK**.
        
    - 4 Configurar as propriedades
        
        Selecione a conta que criou no servidor de configuração na lista pendente e clique em **OK**.
        
    - 5 Configurar as definições de replicação
    
        Certifique-se de que a política de replicação selecionada na lista pendente é **myReplicationPolicy** e clique em **OK**.
        
3. Quando o assistente estiver concluído, clique em **Ativar a replicação**.
        

Pode acompanhar o progresso do trabalho **Ativar Proteção** em **Monitorização e relatórios** > **Trabalhos** > **Trabalhos do Site Recovery**. Depois da tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.        
        
Quando ativa a replicação, as alterações podem demorar 15 minutos ou mais a produzir efeitos e a aparecer no portal.

## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Quando executa uma ativação pós-falha de teste, ocorre o seguinte:

1. É executada uma verificação dos pré-requisitos, para garantir que estão satisfeitas todas as condições necessárias para a ativação pós-falha.
2. A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
3. É criada uma VM do Azure com base nos dados processados no passo anterior.

No portal, execute a ativação pós-falha de teste da seguinte forma:

1. Na página do seu cofre, aceda a **Itens protegidos** > **Itens Replicados**> clique na VM > **+ Ativação Pós-falha de Teste**.

2. Selecione um ponto de recuperação a utilizar para a ativação pós-falha:
    - **Processado mais recentemente**: faz a ativação pós-falha da VM para o ponto de recuperação mais recente processado pelo Site Recovery. O carimbo de data/hora é apresentado. Com esta opção, não é despendido tempo a processar os dados, pelo que oferece um RTO (objetivo de tempo de recuperação) baixo.
    - **Consistente com a aplicação mais recente**: esta opção faz a ativação pós-falha de todas as VMs para o último ponto de recuperação consistente com a aplicação. O carimbo de data/hora é apresentado.
    - **Personalizado**: selecione qualquer ponto de recuperação.
3. Em **Ativação Pós-Falha de Teste**, selecione a rede do Azure de destino para as VMs do Azure que serão ligadas após a ocorrência da ativação pós-falha. Esta deve ser a rede que criou na secção [Preparar recursos do Azure](#prepare-azure-resources).
4. Clique em **OK** para iniciar a ativação pós-falha. Pode clicar na VM e abrir as respetivas propriedades para acompanhar o progresso. Ou pode clicar no trabalho **Ativação Pós-falha de Teste** na página do seu cofre em **Monitorização e relatórios** > **Trabalhos** >
   **Trabalhos do Site Recovery**.
5. Após a conclusão da ativação pós-falha, a VM do Azure de réplica é apresentada no portal do Azure > **Máquinas Virtuais**. Verifique que a VM tem o tamanho adequado, está ligada à rede certa e está em execução.
6. Deverá conseguir ligar-se à VM replicada no Azure agora.
7. Para eliminar as VMs do Azure criadas durante a ativação pós-falha de teste, clique em **Limpar ativação pós-falha de teste**, no plano de recuperação. Em **Notas**, registe e guarde todas as observações associadas à ativação pós-falha de teste.

Em alguns cenários, a ativação pós-falha requer processamento adicional, que demora cerca de oito a dez minutos a concluir. 


## <a name="migrate-to-azure"></a>Migrar para o Azure

Execute uma ativação pós-falha real para as instâncias do EC2, para migrá-las para as VMs do Azure. 

1. Em **Itens protegidos** > **Itens replicados**, clique nas instâncias do AWS > **Ativação Pós-falha de teste**.
2. Em **Ativação Pós-falha**, selecione um **Ponto de Recuperação** para o qual fazer a ativação pós-falha. Selecione o ponto de recuperação mais recente.
3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha** se quiser que o Site Recovery tente encerrar as máquinas virtuais de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.
4. Verifique se a VM aparece em **Itens replicados**. 
5. Clique com o botão direito do rato em cada VM > **Concluir a Migração**. Desta forma, o processo de migração é concluído, a replicação da VM do AWS é parada e a faturação do Site Recovery para a VM também é parada.

    ![Concluir a migração](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Não cancelar uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada. Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.  


    

## <a name="next-steps"></a>Passos Seguintes

Neste tópico, aprendeu a migrar instâncias do AWS EC2 para VMs do Azure. Para saber mais sobre as VMs do Azure, avance para os tutoriais relativos às VMs do Windows.

> [!div class="nextstepaction"]
> [Tutoriais de máquinas virtuais do Windows do Azure](../../virtual-machines/windows/tutorial-manage-vm.md)
