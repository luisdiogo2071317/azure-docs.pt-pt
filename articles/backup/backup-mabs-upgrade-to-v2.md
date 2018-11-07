---
title: Instalar o servidor de cópia de segurança do Azure v2
description: O Azure Backup Server v2 oferece recursos aprimorados de cópia de segurança para proteger as VMs, ficheiros e pastas, cargas de trabalho e muito mais. Saiba como instalar ou atualizar para o servidor de cópia de segurança do Azure v2.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: 4f1e0c14d3a835b9f6d739511186bdcc19917a7a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230998"
---
# <a name="install-azure-backup-server-v2"></a>Instalar o servidor de cópia de segurança do Azure v2

O servidor de cópia de segurança do Azure ajuda a proteger as suas máquinas virtuais (VMs), cargas de trabalho, arquivos e pastas e muito mais. V2 de servidor de cópia de segurança do Azure baseia-se na v1 do servidor de cópia de segurança do Azure e oferece novos recursos que não estão disponíveis no v1. Para obter uma comparação das funcionalidades do v1 e v2, consulte a [matriz de proteção do Azure Backup Server](backup-mabs-protection-matrix.md). 

As funcionalidades adicionais no servidor de cópia de segurança v2 são uma atualização do servidor de cópia de segurança v1. No entanto, a v1 do servidor de cópia de segurança não é um pré-requisito para a instalação do servidor de cópia de segurança v2. Se quiser atualizar do v1 do servidor de cópia de segurança para o servidor de cópia de segurança v2, instale v2 do servidor de cópia de segurança no servidor de proteção do servidor de cópia de segurança. Definições do servidor de cópia de segurança existentes permanecem intactas.

Pode instalar o servidor de cópia de segurança v2 no Windows Server 2016 ou Windows Server 2012 R2. Para tirar partido das novas funcionalidades, como o System Center 2016 proteção Manager moderna cópia de segurança do armazenamento de dados, tem de instalar o servidor de cópia de segurança v2 no Windows Server 2016. Antes de atualizar para ou instalar o servidor de cópia de segurança v2, leia sobre o [pré-requisitos de instalação](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> O servidor de cópia de segurança do Azure tem o mesmo código base como o System Center Data Protection Manager. Cópia de segurança v1 do servidor é equivalente ao Data Protection Manager 2012 R2. Cópia de segurança v2 de servidor é equivalente ao Data Protection Manager 2016. Este artigo ocasionalmente faz referência a documentação do Data Protection Manager.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Atualize o servidor de cópia de segurança para v2
Para atualizar do v1 do servidor de cópia de segurança para o servidor de cópia de segurança v2, certifique-se a instalação tem as atualizações necessárias:

- [Atualizar os agentes de proteção](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) nos servidores protegidos.
- Atualize o Windows Server 2012 R2 para o Windows Server 2016.
- Atualize o administrador remoto do Azure Backup Server em todos os servidores de produção.
- Certifique-se de que as cópias de segurança estão definidas para continuar sem reiniciar o servidor de produção.


### <a name="upgrade-steps-for-backup-server-v2"></a>Etapas de atualização para o servidor de cópia de segurança v2

1. No Centro de transferências, [transfira o instalador de atualização](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Depois de extrair o Assistente de configuração, certifique-se de que **executar setup.exe** está selecionado e, em seguida, selecione **concluir**.

  ![Instalador - execute a configuração de configuração](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. No Assistente do Microsoft Azure Backup Server, sob **instale**, selecione **Microsoft Azure Backup Server**.

  ![Configurar o instalador - selecione instalar](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Sobre o **bem-vindo** página, reveja os avisos e, em seguida, selecione **próxima**.

  ![Instalador da instalação - página de boas-vindas](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. O Assistente de configuração executa verificações de pré-requisitos para se certificar de que pode atualizar o seu ambiente. Sobre o **verificações de pré-requisitos** página, selecione **verificar**.

  ![Instalador da instalação - página de verificações de pré-requisitos](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Seu ambiente tem de passar as verificações de pré-requisitos. Se o seu ambiente não passar nas verificações, tenha em atenção os problemas e corrigi-los. Em seguida, selecione **verificar novamente**. Depois de passar as verificações de pré-requisitos, selecione **seguinte**.

  ![Instalador da instalação - botão verificar novamente](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Sobre o **definições do SQL** página, selecione a opção relevante para a instalação do SQL e, em seguida, selecione **verificar e instalar**.

  ![Instalador da instalação - página de definições de SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  As verificações poderão demorar alguns minutos. Quando as verificações de terminar, selecione **seguinte**.

  ![Instalador da instalação - Verifique as definições de SQL e o botão instalar](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. Sobre o **definições de instalação** página, efetue as alterações para a localização onde está instalado o servidor de cópia de segurança ou para a localização de rascunho. Selecione **Seguinte**.

  ![Instalador da instalação - página de definições de instalação](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Para concluir o Assistente de configuração, selecione **concluir**.

  ![Instalador da instalação - concluir](./media/backup-mabs-upgrade-to-v2/run-setup.png)

## <a name="add-storage-for-modern-backup-storage"></a>Adicionar armazenamento para o armazenamento de cópia de segurança Moderno

Para melhorar a eficiência de armazenamento de cópia de segurança, o servidor de cópia de segurança v2 adiciona suporte para volumes. Cópia de segurança servidor v1 e v2 de servidor de cópia de segurança suportam discos.

### <a name="add-volumes-and-disks"></a>Adicionar volumes e discos

Se executar o servidor de cópia de segurança v2 no Windows Server 2016, pode utilizar volumes para armazenar dados de cópia de segurança. Volumes oferecem poupanças de armazenamento e cópias de segurança mais rápidas. Como os volumes são novos no servidor de cópia de segurança, tem de adicioná-los. 

Quando adiciona um volume ao servidor de cópia de segurança, pode dar o volume de um nome amigável. Selecione o **nome amigável** coluna do volume do mesmo nome. Pode alterar o nome mais tarde, se necessário. Também pode utilizar o PowerShell para adicionar ou alterar nomes amigáveis dos volumes.

Para adicionar um volume na consola do administrador:

1. Na consola de administrador do servidor de cópia de segurança do Azure, selecione **gerenciamento** > **armazenamento de disco** > **adicionar**.

  ![Abrir o Assistente de adicionar armazenamento no disco](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

  É aberto o Assistente de adicionar armazenamento no disco.

2. Na **adicionar armazenamento no disco** página, além do **volumes disponíveis** caixa, selecione um volume e, em seguida, selecione **adicionar**.

3. Na **volumes selecionados** caixa, introduza um nome amigável para o volume e, em seguida, selecione **OK**.

  ![Adicionar Assistente de armazenamento em disco - adicionar volume](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Se pretender adicionar um disco, o disco tem de pertencer a um grupo de proteção que tem o armazenamento legado. Pode utilizar estes discos apenas para estes grupos de proteção. Se o servidor de cópia de segurança não tiver origens com proteção legada, o disco não está listado.

  Para obter mais informações sobre como adicionar discos, consulte [adicionar discos para aumentar o armazenamento legado](https://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Não é possível dar um disco um nome amigável.


### <a name="assign-workloads-to-volumes"></a>Atribuir cargas de trabalho a volumes

No servidor de cópia de segurança, especifique as cargas de trabalho são atribuídas aos respetivos volumes. Por exemplo, pode definir volumes dispendiosos que suportam um número elevado de operações de entrada/saída por segundo (IOPS) para armazenar apenas as cargas de trabalho que necessitam de cópias de segurança frequentes, grande volume. Um exemplo é o SQL Server com logs de transação.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Para atualizar as propriedades de um volume no agrupamento de armazenamento no servidor de cópia de segurança, utilize o cmdlet do PowerShell **Update-DPMDiskStorage**.

Sintaxe:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [<CommonParameters>]
```

Todas as alterações efetuadas através do PowerShell são refletidas na IU.


## <a name="protect-data-sources"></a>Proteger origens de dados
Para começar a proteger origens de dados, crie um grupo de proteção. Os passos seguintes realçam as alterações ou adições ao Assistente de novo grupo de proteção.

Para criar um grupo de proteção:

1. Na consola do administrador do servidor de cópia de segurança, selecione **proteção**.

2. No Friso, selecione **New**.

  É aberto o Assistente Criar novo grupo de proteção.

  ![Assistente Criar novo grupo de proteção](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Na página **Bem-vindo**, selecione **Seguinte**.

4. Sobre o **selecionar tipo de grupo de proteção** página, selecione o tipo de grupo de proteção que pretende criar e, em seguida, selecione **próxima**.

  ![Página de tipo de selecionar o grupo de proteção](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. No **selecionar membros do grupo** página, além da **membros disponíveis** caixa, os membros com a proteção de agentes estão listados. Neste exemplo, selecione o volume D:\ e e:\, e, em seguida, adicioná-los ao **membros selecionados**. Selecione **Seguinte**.

  ![Página de membros de selecionar grupo](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Sobre o **selecionar método de proteção de dados** página, introduza um **nome do grupo de proteção**, selecione o método de proteção e, em seguida, selecione **seguinte**. Se pretender que a proteção de curta duração, selecione o **disco** método de cópia de segurança.

  ![Selecione a página de método de proteção de dados](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Sobre o **especificar objetivos a curto prazo** , selecione os detalhes de **período de retenção** e **frequência de sincronização**. Em seguida, selecione **Seguinte**. Opcionalmente, para alterar a agenda para quando os pontos de recuperação são obtidos, selecione **modificar**.

  ![Especificar objetivos a curto prazo página](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Sobre o **rever atribuição do armazenamento de disco** página, reveja os detalhes sobre as origens de dados selecionada, incluindo o tamanho da origem de dados, os valores para o espaço a ser aprovisionado e o volume de armazenamento de destino.

  ![Página de atribuição de armazenamento do disco de revisão](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Volumes de armazenamento baseiam-se a alocação de volume da carga de trabalho (definido com o PowerShell) e o armazenamento disponível. Pode alterar os volumes de armazenamento ao selecionar outros volumes no menu pendente. Se alterar o valor para **armazenamento de destino**, o valor de **armazenamento em disco disponível** dinamicamente as alterações para refletir os valores sob **espaço livre** e  **Sob aprovisionamento espaço**.

  Se as origens de dados cresça conforme planejado, o valor para o **espaço sob aprovisionamento** coluna na **armazenamento em disco disponível** reflete a quantidade de armazenamento adicional necessário. Utilize este valor para o ajudar a planear as suas necessidades de armazenamento de cópias de segurança fluídas. Se o valor for zero, não existem potenciais problemas com o armazenamento no futuro próximo. Se o valor é um número diferente de zero, não tem armazenamento suficiente atribuído (com base na sua política de proteção e o tamanho dos dados dos seus membros protegidos).

  ![Armazenamento de disco sob alocação](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

  Para concluir a criação do seu grupo de proteção, conclua o assistente.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Migrar armazenamento legado para o armazenamento de cópia de segurança Moderno
Depois de atualizar ou instalar o servidor de cópia de segurança v2 e, em seguida, atualize o sistema operativo para o Windows Server 2016, atualize os grupos de proteção para utilizar o armazenamento de cópia de segurança Moderno. Por predefinição, os grupos de proteção não são alterados. Continuam a funcionar conforme foram configurados inicialmente. 

A atualização dos grupos de proteção para utilizar o armazenamento de cópia de segurança moderno é opcional. Para atualizar o grupo de proteção, pare a proteção de todas as origens de dados utilizando a opção manter dados. Em seguida, adicione as origens de dados a um novo grupo de proteção.

1. No System Center 2016 DPM Administrator Console, selecione o **proteção** funcionalidade. Na **membro do grupo de proteção** listar, o membro com o botão direito e, em seguida, selecione **parar proteção do membro**.

  ![Parar proteção do membro](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. Na **remover do grupo** diálogo caixa, reveja o espaço em disco utilizado e o espaço livre disponível no agrupamento de armazenamento. A predefinição é deixar os pontos de recuperação no disco e permitir que os mesmos expirem, conforme a política de retenção associada. Selecione **OK**.

  Se pretender devolver imediatamente o espaço em disco utilizado para o pool de armazenamento livre, selecione o **Eliminar réplica no disco** caixa de verificação para eliminar os dados de cópia de segurança (e pontos de recuperação) associados a esse membro.

  ![Remover da caixa de diálogo de grupo](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Crie um grupo de proteção que utiliza o armazenamento de cópia de segurança Moderno. Inclua as origens de dados não protegidas.


## <a name="add-disks-to-increase-legacy-storage"></a>Adicionar discos para aumentar o armazenamento legado

Se pretender utilizar o armazenamento legado com o servidor de cópia de segurança, poderá ter de adicionar discos para aumentar o armazenamento legado. 

Para adicionar armazenamento em disco:

1. No System Center 2016 DPM Administrator Console, selecione **gerenciamento** > **armazenamento de disco** > **adicionar**.

  ![Adicionar a caixa de diálogo de armazenamento em disco](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. Na **adicionar armazenamento no disco** caixa de diálogo, selecione **adicione discos**.

3. Na lista de discos disponíveis, selecione os discos que pretende adicionar. Selecione **Add**e, em seguida, selecione **OK**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Atualizar o agente de proteção do Data Protection Manager

Servidor de cópia de segurança utiliza o agente de proteção do System Center Data Protection Manager para atualizações. Se estiver a atualizar um agente de proteção não está ligado à rede, é possível utilizar o Data Protection Manager Administrator Console para concluir uma atualização do agente ligado. Tem de atualizar o agente de proteção num ambiente de domínio nonactive. Até que o computador cliente está ligado à rede, o Data Protection Manager Administrator Console mostra que a atualização do agente de proteção está pendente.

As secções seguintes descrevem como atualizar agentes de proteção para computadores cliente que estão ligados e para computadores cliente que não estão ligados.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Atualizar um agente de proteção para um computador cliente ligado

1. Na consola do administrador do servidor de cópia de segurança, selecione **gerenciamento** > **agentes**.

2. No painel de visualização, selecione os computadores de cliente para o qual pretende atualizar o agente de proteção.

  > [!NOTE]
  > O **as atualizações do agente** coluna indica quando uma atualização do agente de proteção está disponível para cada computador protegido. Na **ações** painel, o **atualização** ação está disponível apenas quando um computador protegido está selecionado e as atualizações estiverem disponíveis.

3. Para instalar agentes de proteção atualizados em computadores selecionados, o **ações** painel, selecione **atualização**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Atualizar um agente de proteção num computador cliente que não está ligado

1. Na consola do administrador do servidor de cópia de segurança, selecione **gerenciamento** > **agentes**.

2. No painel de visualização, selecione os computadores de cliente para o qual pretende atualizar o agente de proteção.

  > [!NOTE]
  > O **as atualizações do agente** coluna indica quando uma atualização do agente de proteção está disponível para cada computador protegido. Na **ações** painel, o **atualização** ação não está disponível quando um computador protegido é selecionado, a menos que as atualizações estiverem disponíveis.

3. Para instalar agentes de proteção atualizados nos computadores selecionados, selecione **atualização**.

4. Para um computador cliente que não está ligado à rede, até que o computador estiver ligado à rede, o **estado do agente** coluna mostra o estado **atualização pendente**.

  Quando um computador cliente está ligado à rede, o **as atualizações do agente** coluna para o computador cliente apresentem o estado **atualização**.
  
### <a name="move-legacy-protection-groups-from-the-old-version-and-sync-the-new-version-with-azure"></a>Mover grupos de proteção legada da versão antiga e sincronizar a nova versão com o Azure

Quando o servidor de cópia de segurança do Azure e o sistema operacional é atualizado, está pronto para proteger as origens de dados nova ao utilizar o armazenamento de cópia de segurança Moderno. Origens de dados que já estão protegidas continuam a ser protegido como estavam no servidor de cópia de segurança do Azure (herdado). Todos os novos grupos de proteção, utilize o armazenamento de cópia de segurança Moderno.

Para migrar as origens de dados do modo legado de proteção para o armazenamento de cópia de segurança Moderno:

1.  Adicione novos volumes ao agrupamento de armazenamento do Data Protection Manager. Também pode atribuir um nome amigável e selecionar etiquetas da origem de dados.

2. Para cada origem de dados que está no modo herdado, interrompa a proteção das origens de dados. Em seguida, selecione **reter dados protegidos**.  Isto permite recuperar antigos pontos de recuperação após a migração.

3. Crie um novo grupo de proteção. Em seguida, selecione as origens de dados que pretende armazenar utilizando o novo formato.

  Data Protection Manager armazena uma cópia da réplica de armazenamento de backup herdado do volume de armazenamento de cópia de segurança moderno localmente.
    > [!NOTE] 
    > Criar a cópia é apresentado como uma tarefa de operação de pós-recuperação.

  Todos os novos sincronização e pontos de recuperação, em seguida, são armazenados no armazenamento de cópia de segurança Moderno.

  Pontos de recuperação antigos são eliminados horizontalmente à medida que ocorra. Como os pontos de recuperação antigos sejam eliminados, espaço em disco for liberado.

  Quando todos os volumes herdados são eliminados do armazenamento antigo, pode remover o disco do Azure Backup. Em seguida, pode remover o disco do sistema.

4. Fazer uma cópia de segurança da base de dados do Data Protection Manager.

  > [!IMPORTANT]
  > - O novo nome de servidor tem de ser o mesmo nome que a instância original do servidor de cópia de segurança do Azure. Não é possível alterar o nome da nova instância de servidor de cópia de segurança do Azure se pretender utilizar o agrupamento de armazenamento anterior e a base de dados do Data Protection Manager para manter os pontos de recuperação.
  > - Tem de ter uma cópia de segurança da base de dados do Data Protection Manager. Terá de restaurar a base de dados.

5. Encerrar a instância original do servidor de cópia de segurança do Azure, ou coloque o servidor offline.

6. Repor a conta de computador no Active Directory.

7. Instale o Windows Server 2016 numa nova máquina. O nome do servidor, utilize o mesmo nome de máquina que a instância original do servidor de cópia de segurança do Azure.

8. Ingressar no domínio.

9. Instale o servidor de cópia de segurança do Azure v2. (Remova os discos de agrupamento de armazenamento do Data Protection Manager do servidor antigo e importá-los para o novo servidor.)

10. Restaure a base de dados do Data Protection Manager que criou no passo 4.

11. Anexe o armazenamento do servidor de cópia de segurança original para o novo servidor.

12. No SQL Server, restaure a base de dados do Data Protection Manager.

13. Na linha de comando de administrador no novo servidor, utilize `cd` para ir para a pasta de localização e bin de instalação do Microsoft Azure Backup.  

  Exemplo:  
  C:\Windows\System32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\ a cópia de segurança do Azure

14. Execute `DPMSYNC -SYNC`.
  
  > [!NOTE]
  > Se tiver adicionado *novos* discos ao agrupamento de armazenamento do Data Protection Manager, em vez de mover os antigos, executar `DPMSYNC -Reallocatereplica`.

## <a name="new-powershell-cmdlets-in-backup-server-v2"></a>Novos cmdlets do PowerShell no servidor de cópia de segurança v2

Ao instalar o servidor de cópia de segurança do Azure v2, dois novos cmdlets estão disponíveis: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Passos Seguintes

Saiba como preparar o servidor ou começar a proteger uma carga de trabalho:
- [Preparar as cargas de trabalho do servidor de cópia de segurança](backup-azure-microsoft-azure-backup.md)
- [Utilizar o servidor de cópia de segurança para fazer backup de um servidor VMware](backup-azure-backup-server-vmware.md)
- [Utilizar o servidor de cópia de segurança para criar cópias de segurança do SQL Server](backup-azure-sql-mabs.md)
- [Utilizar o armazenamento de cópia de segurança moderno com o servidor de cópia de segurança](backup-mabs-add-storage.md)

