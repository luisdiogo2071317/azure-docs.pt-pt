---
title: Resolver problemas relacionados com o Hyper-V para a replicação do Azure com o Azure Site Recovery | Microsoft Docs
description: Descreve como resolver problemas com o Hyper-V para a replicação do Azure utilizando o Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 04/09/2018
ms.author: rayne
ms.openlocfilehash: 95a33c80b1aeef7fbf8bea0ab760bbd66babdac8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Resolver problemas relacionados com o Hyper-V para o Azure replicação e ativação pós-falha

Este artigo descreve problemas comuns que poderá encontrar ao replicar no local VMs de Hyper-V para o Azure, utilizando [do Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Ativar a problemas de proteção

Se ocorrerem problemas ao ativar a proteção para VMs de Hyper-V, verifique o seguinte:

1. Verifique se os anfitriões Hyper-V e as VMs estão em conformidade com todas as [requisitos e pré-requisitos](hyper-v-azure-support-matrix.md).
2. Se os servidores Hyper-V estão localizados em nuvens do System Center Virtual Machine Manager (VMM), certifique-se de que já preparou a [servidor VMM](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Verifique se o serviço de gestão de Máquina Virtual de Hyper-V está em execução em anfitriões Hyper-V.
4. Verifique a existência de problemas que são apresentados no registo de Hyper-V-VMMS\Admin na VM. Este registo está localizado em **registos de serviços e aplicações** > **Microsoft** > **Windows**.
5. Na VM do convidado, certifique-se de que WMI está ativada e acessível.
  - [Saiba mais sobre](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) teste básico de WMI.
  - [Resolver problemas](https://aka.ms/WMiTshooting) WMI.
  - [Resolver problemas ](https://technet.microsoft.com/library/ff406382.aspx#H22) problemas com os scripts WMI e serviços.
5. Na VM do convidado, certifique-se de que a versão mais recente dos serviços de integração está em execução.
    - [Verifique](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) que têm a versão mais recente.
    - [Manter](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) serviços de integração atualizado.
    
## <a name="replication-issues"></a>Problemas de replicação

Resolva problemas com replicação inicial e contínua da seguinte forma:

1. Certifique-se de que está a executar o [versão mais recente](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) dos serviços de recuperação de sites.
2. Certifique-se de que o se a replicação está em pausa:
  - Verifique o estado de funcionamento da VM na consola do Gestor de Hyper-V.
  - Se for crítico, com o botão direito a VM > **replicação** > **Ver estado de funcionamento de replicação**.
  - Se a replicação está em pausa, clique em **retomar replicação**.
3. Certifique-se de que os serviços necessários estão em execução. Se não estão, reiniciá-las.
    - Se estiver a replicar Hyper-V sem o VMM, verifique se estes serviços estão em execução no anfitrião Hyper-V:
        - Serviço de gestão de máquinas virtuais
        - Serviço de agente de serviços de recuperação do Microsoft Azure
        - Serviço Microsoft Azure Site Recovery
        - Serviço de anfitrião do fornecedor de WMI
    - Se estiver a replicar com o VMM no ambiente, verifique que estes serviços estão em execução:
        - No anfitrião Hyper-V, verifique se o serviço de gestão de Máquina Virtual, o Microsoft Azure Recovery Services Agent e o serviço de anfitrião do fornecedor de WMI estão em execução.
        - No servidor do VMM, certifique-se de que o serviço do Virtual Machine Manager do System Center está em execução.
4. Verifique a conectividade entre o servidor de Hyper-V e do Azure. Para tal, abra o Gestor de tarefas no anfitrião Hyper V. No **desempenho** separador, clique em **abra o Monitor de recursos**. No **rede** separador > **Processess com atividade de rede**, verifique se cbengine.exe ativamente está a enviar grandes (MB) de volumes de dados.
5. Verifique se os anfitriões Hyper-V podem ligar para o URL do blob storage do Azure. Para tal, selecione e verifique **cbengine.exe**. Vista **ligações TCP** para verificar a conectividade do anfitrião para o blob storage do Azure.
6. Verifique os problemas de desempenho, conforme descrito abaixo.
    
### <a name="performance-issues"></a>problemas de desempenho

Limitações de largura de banda de rede podem ter impacto na replicação. Resolver problemas da seguinte forma:

1. [Verifique](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) se existirem largura de banda ou limitação restrições no seu ambiente.
2. Execute o [gerador de perfis de implementação Planner](hyper-v-deployment-planner-run.md).
3. Depois de executar o gerador de perfis, siga o [largura de banda](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) e [armazenamento](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) recomendações.
4. Verifique [dados churn limitações](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Se vir elevados de dados churn numa VM, efetue o seguinte:
  - Verifique se a VM está marcada para ressincronização.
  - Siga [estes passos](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) para investigar o volume de alterações da origem.
  - Volume de alterações pode ocorrer quando os ficheiros de registo HRL excedem 50% do espaço em disco disponível. Se este for o problema, Aprovisione mais espaço de armazenamento para todas as VMs no qual ocorre o problema.
  - Certifique-se de que a replicação não está em pausa. Se estiver, continua a escrever as alterações ao ficheiro hrl, que pode contribuir para o tamanho maior.
 

## <a name="critical-replication-state-issues"></a>Problemas de estado de replicação crítica

1. Para verificar o estado de funcionamento de replicação, ligar para a consola do Gestor de Hyper-V no local, selecione a VM e verifique o estado de funcionamento.

    ![Estado de funcionamento de replicação](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Clique em **Ver estado de funcionamento de replicação** para ver os detalhes:

    - Se a replicação está em pausa, com o botão direito a VM > **replicação** > **retomar replicação**.
    - Se uma VM num anfitrião Hyper-V configurada na recuperação de Site migra para outro anfitrião Hyper-V no mesmo cluster ou para um computador autónomo, a replicação para a VM não é afetada. Verificar apenas que o novo anfitrião do Hyper-V cumpre todos os pré-requisitos e está configurado no Site Recovery.

## <a name="app-consistent-snapshot-issues"></a>Problemas de instantâneo consistentes da aplicação

Um instantâneo consistentes da aplicação é um instantâneo de ponto no tempo dos dados de aplicação dentro da VM. Serviço de cópia de sombra de volume (VSS) assegura que as aplicações na VM estão num estado consistente quando se obtém o instantâneo.  Esta secção fornece detalhes sobre alguns problemas comuns que podem ocorrer.

### <a name="vss-failing-inside-the-vm"></a>VSS falhar dentro da VM

1. Verifique se a versão mais recente dos serviços de integração se encontra instalado e em execução.  Verifique se está disponível uma atualização, executando o seguinte comando a partir de uma linha de comandos elevada do PowerShell no anfitrião Hyper-V: **get-vm | selecione o nome, estado, IntegrationServicesState**.
2. Verifique se os serviços VSS estão em execução e bom estado de funcionamento:
    - Para tal, inicie sessão no VM do convidado. Em seguida, abra uma linha de comandos de administrador e execute os seguintes comandos para verificar se todos os os escritores VSS se encontram em bom estado.
        - **Vssadmin escritores de lista**
        - **Vssadmin sombras de lista**
        - **Listar fornecedores a vssadmin**
    - Verifique a saída. Se escritores se encontram em estado de falha, efetue o seguinte:
        - Verifique o registo de eventos de aplicações na VM erros de operação do VSS.
    - Experimente reiniciar estes serviços associados com o escritor com falhas:
        - Cópia sombra de volumes
         - Fornecedor VSS do Azure Site Recovery
    - Depois de fazê-lo, aguarde duas horas para ver se os instantâneos consistentes da aplicação são gerados com êxito.
    - Como último recurso, tente reiniciar a VM. Isto poderá resolver serviços que estão num Estado não responde.
3. Certifique-se de que não tem discos dinâmicos na VM. Não é suportado para instantâneos consistentes da aplicação. Pode verificar-se na gestão de discos (diskmgmt.msc).

    ![Disco dinâmico](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. Certifique-se de que não tem um disco de iSCSI ligado à VM. Tal não é suportado.
5. Certifique-se de que o serviço de cópia de segurança está ativado. Certifique-se de isto em **definições de Hyper-V** > **serviços de integração**.
6. Certifique-se de que existem não está em conflito com as aplicações tirar instantâneos VSS. Pode ocorrer se várias aplicações estão a tentar criar instantâneos VSS no mesmo está em conflito do tempo. Por exemplo, se uma aplicação de cópia de segurança está a demorar instantâneos VSS quando a recuperação de sites estiver agendada, a política de replicação para criar um instantâneo.   
7. Verifique se a VM está a experienciar uma elevada taxa de alteração:
    - Pode medir a taxa de alteração de dados diárias para as VMs de convidado, utilizando os contadores de desempenho no anfitrião Hyper-V. Para tal, ative o seguinte contador. Aggregrate um exemplo deste valor em todos os discos da VM para 5-15 minutos, para obter o volume de alterações VM.
        - Categoria: "dispositivo de armazenamento Virtual do Hyper-V"
        - Contador: "escrever Bytes / seg"</br>
        - Esta taxa de alteração de dados terá de aumentar ou manter-se a um nível elevado, dependendo de como ocupado a VM ou o respetivas aplicações são.
        - O fluxo de dados do disco de média de origem é 2 MB/s de armazenamento standard para a recuperação de Site. [Saiba mais](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Além disso, pode [verificar metas de escalabilidade do armazenamento](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets.md#scalability-targets-for-a-storage-account).
8. Execute o [Planeador de implementação](hyper-v-deployment-planner-run.md).
9. Reveja as recomendações para [rede](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) e [armazenamento](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>VSS falhar dentro do anfitrião de Hyper-V

1. Verifique os registos de eventos para erros VSS e recomendações:
    - No servidor de anfitrião do Hyper-V, abra o registo de eventos do administrador do Hyper-V no **Visualizador de eventos** > **registos de serviços e aplicações** > **Microsoft**  >  **Windows** > **Hyper-V** > **Admin**.
    - Certifique-se de que o se existem quaisquer eventos que indiquem a falhas de instantâneo consistentes da aplicação.
    - Um erro de típico é: "Hyper-V não foi possível gerar o conjunto de instantâneos VSS para a máquina virtual 'XYZ': O escritor teve um erro de não transitório. Reiniciar o serviço VSS poderá resolver problemas se o serviço está a responder."

2. Para gerar os instantâneos VSS para a VM, certifique-se que os serviços de integração do Hyper-V são instalados na VM e que o serviço de integração de cópia de segurança (VSS) está ativado.
    - Certifique-se de que o serviço/daemons do VSS de serviços de integração estão em execução no convidado e estão num **OK** estado.
    - Pode verificar isto partir de uma sessão elevada do PowerShell no anfitrião Hyper-V com o comando **definir-VMIntegrationService - VMName<VMName>-nome VSS** também pode obter estas informações, iniciando sessão numa VM do convidado. [Saiba mais](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - Certifique-se de que os serviços de integração de cópia de segurança/VSS na VM estão em execução e em bom estado de funcionamento. Se não estiver, reinicie esses serviços, e e o serviço do requerente de cópia sombra de volumes Hyper-V no servidor de anfitrião do Hyper-V.

### <a name="common-errors"></a>Erros comuns

**Código de erro** | **Mensagem** | **Detalhes**
--- | --- | ---
**0x800700EA** | "Não conseguiu gerar o conjunto de instantâneos VSS para a máquina virtual de Hyper-V: estão disponíveis mais dados. (0x800700EA). Geração de conjunto de instantâneos VSS pode falhar se a operação de cópia de segurança está em curso.<br/><br/> Operação de replicação da máquina virtual falhou: estão disponíveis mais dados. " | Verificar se a VM tem o disco dinâmico ativado. Tal não é suportado.
**0x80070032** | "Falha ao ligar à máquina virtual o requerente de cópia de sombra de Volume de Hyper-V <. / VMname > porque a versão não corresponde à versão esperada pelo Hyper-V | Verifique se as atualizações mais recentes do Windows estão instaladas.<br/><br/> [Atualizar](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date) para a versão mais recente do Integration Services.



## <a name="collect-replication-logs"></a>Os registos de replicação de recolha

Todos os eventos de replicação de Hyper-V são registados no registo do Hyper-V-VMMS\Admin, localizado na **registos de serviços e aplicações** > **Microsoft** > **Windows**. Além disso, pode ativar um registo de análise para o serviço de gestão de Máquina Virtual de Hyper-V, da seguinte forma:

1. Certifique-os registos analíticos e de depuração podem ser visualizados no Visualizador de eventos. Para tal, no Visualizador de eventos, clique em **vista** > **Mostrar depurar registos analíticos e.**. O registo de análise é apresentado em **Hyper-V-VMMS**.
2. No **ações** painel, clique em **ativar registo**. 

    ![Ativar o registo](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Depois de estar ativado, é apresentado no **Monitor de desempenho**, como um **sessão de rastreio de eventos** em **conjuntos de Recoletores de dados**. 
4. Para ver as informações recolhidas, pare a sessão de rastreio ao desativar o registo. Em seguida, guarde o registo e abri-lo novamente no Visualizador de eventos ou utilizar outras ferramentas para convertê-la conforme necessário.


### <a name="event-log-locations"></a>Localizações de registo de eventos

**Registo de eventos** | **Detalhes** |
--- | ---
**Aplicações e serviço de registos/Microsoft/VirtualMachineManager/servidor/Admin** (servidor VMM) | Registos para resolver problemas do VMM.
**Aplicações e serviço de registos/MicrosoftAzureRecoveryServices/replicação** (anfitrião de Hyper-V) | Registos para resolver problemas do Microsoft Azure Recovery Services Agent. 
**Aplicações e serviço de registos/Microsoft/Azure Site Recovery/fornecedor/Operational** (anfitrião de Hyper-V)| Registos para resolver problemas de serviço do Microsoft Azure Site Recovery.
**Aplicações e serviço de registos/Microsoft/Windows/Hyper-V-VMMS/Admin** (anfitrião de Hyper-V) | Registos para resolver problemas de gestão de VM de Hyper-V.

### <a name="log-collection-for-advanced-troubleshooting"></a>Coleção de registo para resolução de problemas avançada

Estas ferramentas podem ajudar na resolução de problemas avançada:

-   Para o VMM, efetuar a recolha de registo da recuperação de Site utilizando o [ferramenta de diagnóstico de suporte plataforma (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   Para o Hyper-V sem o VMM, [transferir esta ferramenta](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab), e execute-o no anfitrião Hyper-V para recolher os registos.

