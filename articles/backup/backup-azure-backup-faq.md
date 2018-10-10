---
title: FAQ do Azure Backup
description: 'Respostas a perguntas comuns sobre: funcionalidades do Azure Backup incluindo os cofres dos Serviços de Recuperação, que cópias de segurança podem criar, como funcionam, a encriptação e os limites. '
services: backup
author: markgalioto
manager: carmonm
keywords: recuperação de cópia de segurança e após desastres; serviço de cópia de segurança
ms.service: backup
ms.topic: conceptual
ms.date: 8/2/2018
ms.author: markgal
ms.openlocfilehash: 9168a67366664f50a49ae04ef8ddc2f7aa9d665b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886502"
---
# <a name="questions-about-the-azure-backup-service"></a>Perguntas sobre o serviço Azure Backup
Este artigo responde a perguntas comuns sobre os componentes de cópia de segurança do Azure. Em algumas das respostas, existem ligações para os artigos que incluem informação abrangente. Pode fazer perguntas sobre o Azure Backup ao clicar em **Comentários** (à direita). Os comentários aparecem na parte inferior do artigo. É necessária uma conta Livefyre para o comentário. Também pode publicar perguntas sobre o serviço de Backup do Azure no [fórum de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Para analisar rapidamente as secções neste artigo, utilize as ligações à direita, em **Neste artigo**.


## <a name="recovery-services-vault"></a>Cofre dos serviços de recuperação

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Existe algum limite ao número de cofres que podem ser criados em cada subscrição do Azure? <br/>
Sim. Pode criar até 500 cofres de serviços de recuperação por região suportada da cópia de segurança do Azure, por subscrição. Se precisar de mais cofres, crie uma subscrição adicional.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Existem limites no número de servidores/máquinas que podem ser registados em relação a cada cofre? <br/>
Pode registar até 1000 as máquinas virtuais do Azure por cofre. Se estiver a utilizar o agente de MAB, pode registar até 50 agentes MAB por cofre. E pode registrar 50 servidores DPM/servidores MAB para um cofre.

### <a name="can-i-use-a-rest-api-to-query-the-size-of-protected-items-in-a-vault-br"></a>Pode utilizar uma API REST para consultar o tamanho dos itens protegidos num cofre? <br/>
Sim, o artigo [utilizações - lista por cofres](https://t.co/2lgIrIaF0J), lista as informações que podem ser obtidas a partir do Cofre de serviços de recuperação.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Se a minha organização tiver um cofre, como posso isolar os dados de um servidor de outro servidor quando restaurar os dados?<br/>
Todos os servidores registados no mesmo cofre podem recuperar os dados de cópias de segurança de outros servidores *que utilizam a mesma frase de acesso*. Se tiver servidores cujos dados de cópia de segurança pretende isolar de outros servidores na sua organização, utilize uma frase de acesso designada para esses servidores. Por exemplo, os servidores de recursos humanos podem utilizar uma frase de acesso de encriptação, os servidores de gestão de contas outra e os servidores de armazenamento uma terceira.

### <a name="can-i-migrate-my-vault-between-subscriptions-br"></a>Posso migrar meu cofre entre subscrições? <br/>
Não. O Cofre é criado ao nível da subscrição e não pode ser reatribuído para outra subscrição.

### <a name="can-i-migrate-backup-data-to-another-vault-br"></a>Pode migrar dados de cópia de segurança para outro Cofre? <br/>
Não. Não não possível mover dados de cópia de segurança armazenados num cofre num cofre diferente.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup-br"></a>Posso alterar de GRS para LRS depois de uma cópia de segurança? <br/>
Não. Um cofre dos serviços de recuperação só pode alterar as opções de armazenamento antes de quaisquer cópias de segurança foram armazenadas.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-still-supported-br"></a>Os cofres dos Serviços de Recuperação baseiam-se no Resource Manager. Cofres de cópia de segurança ainda são suportados? <br/>
Foram convertidos cofres de cópia de segurança para cofres dos serviços de recuperação. Se não tiver convertido o Cofre de cópia de segurança para um cofre dos serviços de recuperação, em seguida, o Cofre de cópia de segurança foi convertido para um cofre dos serviços de recuperação para.

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Posso migrar um cofre da Cópia de Segurança para um cofre dos Serviços de Recuperação? <br/>
Todos os cofres de cópia de segurança foram convertidos para cofres dos serviços de recuperação. Se não tiver convertido o Cofre de cópia de segurança para um cofre dos serviços de recuperação, em seguida, o Cofre de cópia de segurança foi convertido para um cofre dos serviços de recuperação para.

## <a name="azure-backup-agent"></a>Agente do Backup do Azure
Está disponível uma lista detalhada de perguntas em [FAQ on Azure file-folder backup](backup-azure-file-folder-backup-faq.md) (FAQ sobre a cópia de segurança de ficheiros/pastas do Azure).

## <a name="azure-vm-backup"></a>Cópias de segurança de VMs do Azure
Está disponível uma lista detalhada de perguntas em [FAQ on Azure VM backup](backup-azure-vm-backup-faq.md) (FAQ sobre as cópias de segurança de VMs do Azure).

## <a name="back-up-vmware-servers"></a>Fazer cópia de segurança dos servidores VMware

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Posso criar cópias de segurança dos servidores VMware vCenter para o Azure?
Sim. Pode utilizar o Azure Backup Server para criar cópias de segurança VMware vCenter e ESXi para o Azure. Para obter informações sobre a versão suportada do VMware, consulte o artigo [matriz de proteção do Azure Backup Server](backup-mabs-protection-matrix.md). Para obter instruções passo a passo, consulte [utilizar Azure Backup Server para criar cópia de segurança de um servidor VMware](backup-azure-backup-server-vmware.md).

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster-from-dpm-or-azure-backup-serverbr"></a>É necessário uma licença separada para recuperar um cluster de VMware/Hyper-V completa no local a partir do DPM ou servidor de cópia de segurança do Azure?<br/>
Não precisa separar de licenciamento para a proteção de VMware/Hyper-V. Se for um cliente do System Center, utilize o DPM para proteger as VMs VMware. Se não tenha um cliente do System Center, pode utilizar o Azure Backup Server (pay as you go) para proteger as VMs de VMware.

## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Server e System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Posso utilizar o Servidor do Backup do Azure para criar uma cópia de segurança de Recuperação Bare-metal (BMR) para um servidor físico? <br/>
Sim.

### <a name="can-i-register-my-dpm-server-to-multiple-vaults-br"></a>Posso registar o meu Servidor DPM para diversos cofres? <br/>
Não. Um servidor DPM ou MABS pode ser registado para apenas um cofre.

### <a name="which-version-of-system-center-data-protection-manager-is-supported"></a>Que versão do System Center Data Protection Manager é suportada?
Recomendamos que instale o agente do Azure Backup [mais recente](http://aka.ms/azurebackup_agent) no último update rollup (UR) do System Center Data Protection Manager (DPM).
- Para o System Center DPM 2012 R2 [14 de Rollup de atualização](https://support.microsoft.com/help/4043315/update-rollup-14-for-system-center-2012-r2-data-protection-manager) é a atualização mais recente.
- Para o System Center DPM 2016, [Update Rollup 2](https://support.microsoft.com/en-us/help/3209593) é a atualização mais recente.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-protect-on-premises-applicationvm-workloads-to-azure"></a>Instalei o agente do Backup do Azure para proteger os meus ficheiros e pastas. Posso instalar o System Center DPM para proteger cargas de trabalho de VM/aplicações no local para o Azure?
Sim. No entanto, para utilizar o Azure Backup com o System Center Data Protection Manager (DPM), instale o DPM primeiro e, em seguida, instale o agente de cópia de segurança do Azure. Instalar os componentes do Azure Backup por esta ordem garante que o agente do Azure Backup funciona com o DPM. Instalar o agente do Azure Backup antes do DPM não é aconselhável nem suportado.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Pode utilizar o DPM para efetuar cópias de segurança de aplicações no Azure Stack?
Não. Apesar de poder utilizar o Azure Backup para proteger o Azure Stack, Microsoft Azure Backup não suporta atualmente com o DPM para efetuar cópias de segurança de aplicações no Azure Stack.

## <a name="how-azure-backup-works"></a>Como funciona o Azure Backup
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Se cancelar uma tarefa de cópia de segurança depois de ser iniciada, os dados de cópia de segurança transferidos são eliminados? <br/>
Não. Todos os dados transferidos para o cofre, antes da tarefa de cópia de segurança ser cancelada, permanecem no cofre. O Backup do Azure utiliza um mecanismo de ponto de verificação para adicionar, ocasionalmente, pontos de verificação aos dados de cópia de segurança durante a cópia de segurança. Por existirem pontos de verificação nos dados de cópia de segurança, o processo de cópia de segurança seguinte pode validar a integridade dos ficheiros. A tarefa de cópia de segurança seguinte será incremental face aos dados para os quais foi criada uma cópia de segurança anteriormente. As cópias de segurança incrementais só transferem dados novos ou alterados, o que se traduz numa melhor utilização da largura de banda.

Se cancelar uma tarefa de cópia de segurança para uma VM do Azure, os dados transferidos são ignorados. A próxima tarefa de cópia de segurança transfere os dados incrementais desde a última tarefa de cópia de segurança bem-sucedida.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Existem limites para quando ou para o número de vezes que uma tarefa de cópia de segurança pode ser agendada?<br/>
Sim. Pode executar tarefas de cópia de segurança no Windows Server ou em estações de trabalho Windows até três vezes por dia. Pode executar tarefas de cópia de segurança no System Center DPM até duas vezes por dia. Pode executar uma tarefa de cópia de segurança para as VMs do IaaS uma vez por dia. Utilize a política de agendamento para Windows Server ou estação de trabalho do Windows para especificar agendas diárias ou semanais. Com o System Center DPM, pode especificar agendas diárias, semanais, mensais e anuais.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Por que motivo o tamanho dos dados transferido para o cofre dos Serviços de Recuperação é inferior aos dados dos quais fiz uma cópia de segurança?<br/>
 Todos os dados para os quais são criadas cópias de segurança a partir do Agente do Azure Backup, do SCDPM ou do Azure Backup Server são comprimidos e encriptados antes de serem transferidos. Uma vez aplicada a compressão e encriptação, os dados no cofre dos serviços de recuperação são 30-40% mais reduzidos.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vaultbr"></a>Pode eliminar ficheiros individuais a partir de um ponto de recuperação no Cofre?<br/>
Não, o Azure Backup não suporta a eliminação ou limpar os itens individuais de cópias de segurança armazenadas.

## <a name="what-can-i-back-up"></a>Do que posso fazer uma cópia de segurança
### <a name="which-operating-systems-does-azure-backup-support-br"></a>Que sistemas operativos suportam o Azure Backup? <br/>
O Azure Backup suporta a lista seguinte de sistemas operativos para criar cópias de segurança: ficheiros e pastas, e aplicações de carga de trabalho protegidas com o Azure Backup Server e o System Center Data Protection Manager (DPM).

| Sistema Operativo | Plataforma | SKU |
|:--- | --- |:--- |
| Windows 8 e SPs mais recentes |64 bits |Enterprise, Pro |
| Windows 7 e SPs mais recentes |64 bits |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 e SPs mais recentes |64 bits |Enterprise, Pro |
| Windows 10 |64 bits |Enterprise, Pro, Home |
| Windows Server 2016 |64 bits |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 SPs mais recentes |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 e SPs mais recentes |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 e SPs mais recentes |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 R2 e SPs mais recentes |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 e SPs mais recentes |64 bits |Standard, Workgroup |
| Windows Server 2012 R2 SPs mais recentes |64 bits |Essencial |
| Windows Server 2008 R2 SP1 |64 bits |Standard, Enterprise, Datacenter, Foundation |

**Para cópias de segurança de VMs do Azure:**

* **Linux**: o Azure Backup suporta [uma lista de distribuições apoiadas pelo Azure](../virtual-machines/linux/endorsed-distros.md), exceto Core OS Linux.  Outras distribuições “Bring-Your-Own-Linux” poderão também funcionar, desde que o agente de VM esteja disponível na máquina virtual e haja suporte para Python.
* **Windows Server**: não são suportadas as versões mais antigas do que o Windows Server 2008 R2.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Existe um limite no tamanho de cada origem de dados para uma cópia de segurança? <br/>
O Azure Backup impõe um tamanho máximo para uma origem de dados, no entanto, os limites para a origem são grandes. A partir de agosto de 2015, o tamanho máximo das origens de dados para os sistemas operativos suportados é:

| S.No | Sistema operativo | Tamanho máximo da origem de dados |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 ou posterior |54 400 GB |
| 2 |Windows 8 ou posterior |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

A tabela seguinte explica a forma como é determinado cada tamanho da origem de dados.

| Origem de dados | Detalhes |
|:---:|:--- |
| Volume |A quantidade de dados para a cópia de segurança a partir de único volume de um servidor ou máquina cliente |
| Máquina virtual do Hyper-V |Soma dos dados de todos os VHDs da máquina virtual para a cópia de segurança |
| Base dados do Microsoft SQL Server |Tamanho da SQL Database única para a cópia de segurança |
| Microsoft SharePoint |Soma das bases de dados de conteúdo e da configuração dentro de um farm do SharePoint para a cópia de segurança |
| Microsoft Exchange |Soma de todas as bases de dados do Exchange num servidor Exchange para a cópia de segurança |
| Estado do Sistema/BMR |Cada cópia individual da BMR ou estado do sistema da máquina para a cópia de segurança |

Para cópia de segurança de VM de IaaS do Azure, cada VM pode ter até 32 discos de dados e cada disco de dados pode ter até 4095 GB.

### <a name="is-there-a-limit-on-the-amount-of-data-held-in-a-recovery-services-vault"></a>Existe um limite na quantidade de dados mantidos num cofre dos serviços de recuperação?
Não existe nenhum limite na quantidade de dados, que pode criar uma cópia de segurança serviços de recuperação do cofre.

## <a name="retention-policy-and-recovery-points"></a>Pontos de recuperação e política de retenção
### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Existe alguma diferença entre a política de retenção para o DPM e o Windows Server/cliente Windows (ou seja, no Windows Server sem DPM)?<br/>
Não, o DPM e o Windows Server/cliente Windows têm políticas de retenção diárias, semanais, mensais e anuais.

### <a name="can-i-configure-my-retention-policies-selectively--that-is-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Pode configurar meu políticas de retenção seletivamente – ou seja, configurar semanais e diárias, mas não anuais e mensais?<br/>
Sim, a estrutura de retenção do Backup do Azure permite-lhe ter total flexibilidade na definição da política de retenção, de acordo com os seus requisitos.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Posso "agendar uma cópia de segurança" para as 18:00 e especificar políticas de retenção noutra hora?<br/>
Não. Só podem ser aplicadas políticas de retenção em pontos de cópia de segurança. Na imagem seguinte, a política de retenção é especificada para cópias de segurança criadas às 12:00 e 18:00. <br/>

![Agendar Cópia de Segurança e Retenção](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Se uma cópia de segurança for mantida durante um longo período de tempo, demora mais tempo para recuperar um ponto de dados mais antigo? <br/>
Não – o tempo para recuperar o ponto mais antigo ou mais recente é o mesmo. Cada ponto de recuperação funciona como um ponto completo.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Se cada ponto de recuperação é como um ponto completo, afeta o armazenamento de cópia de segurança faturável total?<br/>
Os produtos de ponto de retenção de longa duração típicos armazenam cópias de segurança como pontos completos. Os pontos completos são *ineficazes* ao nível do armazenamento, mas é mais rápido e mais fácil restaurá-los. As cópias incrementais são *eficientes* ao nível do armazenamento, mas necessitam que restaure uma cadeia de dados, que tem impacto no seu tempo de recuperação. A arquitetura de armazenamento do Backup do Azure dá-lhe o melhor dos dois mundos ao armazenar da melhor maneira dados para restauros rápidos e incorrer em custos de armazenamento reduzido reduzidos. Esta abordagem de armazenamento de dados garante que a largura de banda de entrada e de saída é utilizada com eficácia. A quantidade do armazenamento de dados e o tempo necessário para recuperar os dados são mantidos num mínimo. Saiba mais sobre como as [cópias de segurança incrementais](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) são eficientes.

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Existe um limite no número de pontos de recuperação que podem ser criados?<br/>
Pode criar até 9999 pontos de recuperação por instância protegida. Uma instância protegida é um computador, servidor (físico ou virtual) ou carga de trabalho configurados para criar cópias de segurança para o Azure. Para obter mais informações, consulte as explicações de [Cópia de segurança e retenção](./backup-introduction-to-azure-backup.md#backup-and-retention), e [O que é uma instância protegida](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Quantas recuperações posso efetuar nos dados da cópia de segurança para o Azure?<br/>
Não existe limite no número de recuperações do Backup do Azure.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Quando restaurar os dados, pago pelo tráfego de saída do Azure? <br/>
Não. As recuperações são gratuitas e não lhe é cobrado o tráfego de saída.

### <a name="what-happens-when-i-change-my-backup-policy"></a>O que acontece quando altero meu política de cópia de segurança?
Quando uma nova política é aplicada, agenda e a retenção da nova política é seguida. Se a retenção for estendida, os pontos de recuperação existentes serão marcados para que estejam em conformidade com a política nova. Se a retenção for reduzida, serão marcados para eliminação na tarefa de limpeza seguinte e, posteriormente, eliminados.

## <a name="azure-backup-encryption"></a>Encriptação do Azure Backup
### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Os dados enviados para o Azure são encriptados? <br/>
Sim. Os dados são encriptados na máquina SCDPM/cliente/servidor no local utilizando AES256 e os dados são enviados através de uma ligação HTTPS segura.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Os dados da cópia de segurança no Azure também são encriptados?<br/>
Sim. Os dados enviados para o Azure permanecem encriptados (inativos). A Microsoft não desencripta os dados da cópia de segurança em momento algum. Ao criar uma cópia de segurança de uma VM do Azure, o Azure Backup depende da encriptação da máquina virtual. Por exemplo, se a VM for encriptada com o Azure Disk Encryption ou com outra tecnologia de encriptação, o Azure Backup utiliza essa encriptação para proteger os dados.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Qual é o comprimento mínimo da chave de encriptação utilizado para encriptar os dados da cópia de segurança? <br/>
A chave de encriptação deve ter, pelo menos, 16 caracteres se estiver a utilizar o agente do Azure Backup. Nas VMs do Azure, as chaves utilizadas pelo Azure Key Vault não têm limite de comprimento.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>O que acontece se perder a chave de encriptação? Posso recuperar os dados (ou) pode a Microsoft recuperar os dados? <br/>
A chave utilizada para encriptar os dados da cópia de segurança está presente apenas no local do cliente. A Microsoft não mantém uma cópia no Azure e não tem qualquer acesso à chave. Se o cliente perder a chave, a Microsoft não pode recuperar os dados da cópia de segurança.
