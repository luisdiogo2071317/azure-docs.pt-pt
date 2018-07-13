---
title: Migrar servidores de Windows Server 2008 no local para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como migrar máquinas do Windows Server 2008 no local para o Azure com o Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2018
ms.author: bsiva
ms.openlocfilehash: 0d3f28f0a9f1e9862fabb6ce5e96597f1534abd8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008770"
---
# <a name="migrate-servers-running-windows-server-2008-2008-r2-to-azure"></a>Migrar servidores que executam o Windows Server 2008, 2008 R2 para o Azure

Este tutorial mostra como migrar os servidores no local com o Windows Server 2008 ou 2008 R2 para o Azure com o Azure Site Recovery. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Preparar o ambiente no local para migração
> * Configurar o ambiente de destino
> * Configurar uma política de replicação
> * Ativar a replicação
> * Executar uma migração de teste para confirmar que está tudo a funcionar conforme o previsto
> * Ativação pós-falha do Azure e concluir a migração

As limitações e a secção de problemas conhecidos, listas de algumas das limitações e soluções alternativas para conhecido questões que podem ser encontrados ao migrar para o Windows Server 2008 máquinas para o Azure. 


## <a name="supported-operating-systems-and-environments"></a>Sistemas operativos e Ambientes suportados


|Sistema Operativo  | Ambiente no local  |
|---------|---------|
|Windows Server 2008 SP2 - 32 bits e 64 bits (IA-32 e x86 64)</br>-Standard</br>-Enterprise</br>-Data Center   |     VMs de VMware, VMs Hyper-V e servidores físicos    |
|Windows Server 2008 R2 SP1 – 64 bits</br>-Standard</br>-Enterprise</br>-Data Center     |     VMs de VMware, VMs Hyper-V e servidores físicos|

> [!WARNING]
> - Não é suportada a migração de servidores com o Server Core.
> - Certifique-se de que tem o service pack mais recente e atualizações do Windows instaladas antes da migração.


## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, é útil rever a arquitetura do Azure Site Recovery para [migração de servidor do VMware e físicas](vmware-azure-architecture.md) ou [migração de máquina virtual de Hyper-V](hyper-v-azure-architecture.md) 

Para migrar máquinas de virtuais de Hyper-V com o Windows Server 2008 ou Windows Server 2008 R2, siga os passos a [migrar máquinas no local para o Azure](migrate-tutorial-on-premises-azure.md) tutorial.

O resto deste tutorial mostra-lhe como é possível migrar as máquinas de virtuais de VMware no local e servidores físicos que executem Windows Server 2008 ou 2008 R2.


## <a name="limitations-and-known-issues"></a>Limitações e problemas conhecidos

- O servidor de configuração, servidores de processos adicionais e o serviço de mobilidade utilizado para migrar servidores de Windows Server 2008 SP2 devem estar a executar a versão 9.18.0.1 do software Azure Site Recovery. A configuração unificada para a versão 9.18.0.1 do servidor de configuração e o servidor de processos pode ser transferida a partir [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup).

- Um servidor de configuração existente ou o servidor de processos não pode ser utilizado para migrar os servidores que executam o Windows Server 2008 SP2. Um novo servidor de configuração devem ser aprovisionado com a versão 9.18.0.1 do software Azure Site Recovery. Este servidor de configuração só deve ser utilizado para a migração de servidores Windows para o Azure.

- Pontos de recuperação consistente com a aplicação e a funcionalidade de consistência de várias VMS não são suportadas para a replicação de servidores que executam o Windows Server 2008 SP2. Servidores do Windows Server 2008 SP2 devem ser migradas para um ponto de recuperação consistente com falhas. Pontos de recuperação consistente com falhas são gerados a cada 5 minutos por predefinição. Utilizar uma política de replicação com uma frequência de instantâneo consistente da aplicação configurada fará com que o estado de funcionamento de replicação ativar crítico devido à falta de pontos de recuperação consistente com a aplicação. Para evitar falsos positivos, defina a frequência de instantâneos consistentes com aplicações na política de replicação como "Desligado".

- Os servidores que está a ser migrados devem ter o .NET Framework 3.5 Service Pack 1 para o serviço de mobilidade para trabalhar.


- Se o servidor tem discos dinâmicos, poderá reparar em determinadas configurações, que estes discos no falhadas ao longo do servidor são marcados offline ou apresentadas como discos externos. Também poderá reparar que o estado do conjunto espelhado para volumes espelhados em discos dinâmicos é marcado como "Failed redundância". Pode corrigir este problema do Diskmgmt. msc ao importar estes discos e reativando-los manualmente.

- Os servidores que está a ser migrados, devem ter o driver vmstorfl.sys. Ativação pós-falha poderá falhar se o driver não estiver presente no servidor que está a ser migrado. 
  > [!TIP]
  >Verifique se o driver está presente no "C:\Windows\system32\drivers\vmstorfl.sys". Se o controlador não for encontrado, pode solucionar o problema através da criação de um ficheiro fictício no lugar. 
  >
  > Linha de comandos aberta (executar > cmd) e execute o seguinte: "copiar nul c:\Windows\system32\drivers\vmstorfl.sys"

- Pode não ser possível fazer o RDP para servidores de Windows Server 2008 SP2 com o sistema de operativo de 32 bits imediatamente depois que eles são a ativação pós-falha ou o teste de ativação pós-falha para o Azure. A falha no reinício ao longo da máquina virtual a partir do portal do Azure e tente ligar novamente. Se ainda não é possível ligar, verifique se o servidor está configurado para permitir ligações de ambiente de trabalho remotas e certifique-se de que não há regras de firewall ou bloquear a ligação de grupos de segurança de rede. 
  > [!TIP]
  > Uma ativação pós-falha de teste é altamente recomendável antes de migrar servidores. Certifique-se de que já realizou, pelo menos, uma ativação pós-falha de teste em cada servidor que está a migrar. Como parte da ativação pós-falha de teste, ligue para o teste de efetuar a ativação pós-falha da máquina e garantir que as coisas funcionam conforme esperado.
  >
  >A operação de ativação pós-falha de teste é não disruptivas e ajuda-o a migrações de teste através da criação de máquinas virtuais numa rede isolada da sua preferência. Ao contrário da operação de ativação pós-falha, durante a operação de ativação pós-falha de teste, os replicação de dados continua a progres. Pode executar como as ativações pós-falha de teste que quiser antes de está pronto para migrar. 
  >
  >


## <a name="getting-started"></a>Introdução

Execute as seguintes tarefas para preparar o ambiente de VMware/físico de subscrição e no local do Azure:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. Preparar no local [VMware](vmware-azure-tutorial-prepare-on-premises.md)


## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Criar um recurso** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. Na **Name**, especifique o nome amigável **W2K8 migração**. Se tiver mais do que uma subscrição, selecione a que for adequada.
4. Criar um grupo de recursos **w2k8migrate**.
5. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para aceder rapidamente ao cofre a partir do dashboard, clique em **Afixar ao dashboard** e clique em **Criar**.

   ![Novo cofre](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

O cofre novo é adicionado ao **Dashboard** em **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Preparar o ambiente no local para migração

- Transferir o instalador do servidor de configuração (configuração unificada) a partir do [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup)
- [Configurar](physical-azure-disaster-recovery.md#set-up-the-source-environment) o ambiente de origem com o ficheiro de instalador transferido no passo anterior.

> [!IMPORTANT]
> Certifique-se de que utilize o ficheiro de configuração transferido no primeiro passo para instalar e registar o servidor de configuração. Não transferir o ficheiro de configuração do portal do Azure. O ficheiro de configuração disponível em [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup) é a única versão que suporte a migração do Windows Server 2008.
>
> Não é possível utilizar um servidor de configuração existente para migrar máquinas com o Windows Server 2008. Terá de configurar um novo servidor de configuração através da ligação fornecida acima.

 ![Transferir a chave de registo do Cofre](media/migrate-tutorial-windows-server-2008/download-vault-credentials.png) 
 

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique o modelo de implementação do Resource Manager.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.


## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

1. Para criar uma nova política de replicação, clique em **infraestrutura do Site Recovery** > **políticas de replicação** > **+ política de replicação**.
2. Na **criar política de replicação**, especifique um nome de política.
3. Na **limiar RPO**, especifique o limite (RPO) da objetivo de ponto de recuperação. Um alerta é gerado se a replicação RPO excede este limite.
4. Na **retenção do ponto de recuperação**, especifique o intervalo de tempo (em horas) é a janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela. É suportada até a retenção de 24 horas para máquinas replicadas para o armazenamento premium e 72 horas para o armazenamento standard.
5. Na **frequência de instantâneos consistentes com a aplicação**, especifique **desativar**. Clique em **OK** para criar a política.

A política é associada automaticamente ao servidor de configuração.

> [!WARNING]
> Certifique-se de que especifica **OFF** o consistente com a aplicação na definição de frequência da política de replicação de instantâneos. Apenas os pontos de recuperação consistentes com falhas são suportados ao replicar servidores que executam o Windows Server 2008. Providers que qualquer outro valor para a frequência de instantâneo consistente com a aplicação irá resultar em alertas falsas ao ativar o estado de funcionamento de replicação do servidor crítico devido à ausência de pontos de recuperação consistente com a aplicação.

   ![Criar política de replicação](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>Ativar a replicação

[Ativar a replicação](physical-azure-disaster-recovery.md#enable-replication) para o Windows Server 2008 SP2 / servidor do Windows Server 2008 R2 SP1 a serem migrados.
   
   ![Adicionar servidor físico](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![Ativar a replicação](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Executar uma migração de teste

Pode efetuar uma ativação pós-falha de teste da replicação desses servidores após a conclusão de replicação inicial e o estado do servidor passa a **protegido**.

Execute uma [ativação pós-falha de teste](tutorial-dr-drill-azure.md) para o Azure para confirmar que está tudo a funcionar conforme o previsto.

   ![Ativação pós-falha de teste](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Migrar para o Azure

Execute a ativação pós-falha nos computadores que quer migrar.

1. Em **Definições** > **Itens replicados**, clique no computador > **Ativação Pós-falha**.
2. Em **Ativação pós-falha**, selecione um **Ponto de Recuperação** para o qual fazer a ativação pós-falha. Selecione o ponto de recuperação mais recente.
3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. Recuperação de sites irá tentar encerrar o servidor antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Tarefas**.
4. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.
5. Em **Itens replicados**, clique com o botão direito do rato na VM > **Concluir a Migração**. Desta forma, o processo de migração é concluído, a replicação para a VM é parada e a faturação do Site Recovery para a VM também é parada.

   ![Concluir a migração](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **Não cancele uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada. Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.
