---
title: Atualizar Cofre de cópia de segurança para o Cofre de serviços de recuperação de cópia de segurança do Azure "
description: Atualizar Cofre de cópia de segurança para o Cofre dos serviços de recuperação para obter os novos recursos como a cópia de segurança de VMs, mais segurança e de cópia de segurança de VM de VMware e de cópia de segurança do Estado do sistema para os servidores do Windows do Gestor de recursos
services: backup
author: raynew
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: b7671271e569802311884861265a7825404c9c75
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490351"
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Cofre de cópias de segurança atualizado para cofre dos serviços de recuperação
Este artigo fornece uma descrição geral do Cofre de serviços de recuperação que oferece, perguntas mais frequentes sobre a cópia de segurança existente de atualização do cofre para o Cofre dos serviços de recuperação e etapas de pós-atualização. Um cofre dos serviços de recuperação é o equivalente do Azure Resource Manager de um cofre de cópia de segurança que hospeda os dados de cópia de segurança. Os dados normalmente são cópias de dados ou informações de configuração para máquinas virtuais (VMs), cargas de trabalho, servidores ou estações de trabalho, quer no local ou no Azure.

## <a name="what-is-a-recovery-services-vault"></a>O que é um cofre dos Serviços de Recuperação?
Um cofre dos Serviços de Recuperação é uma entidade de armazenamento online no Azure utilizada para armazenar dados, como cópias de segurança, pontos de recuperação e políticas de cópia de segurança. Pode utilizar cofres dos serviços de recuperação para armazenar dados de cópia de segurança para vários serviços do Azure, como VMs de IaaS (Linux ou Windows) e bases de dados SQL do Azure. Sistema de suporte de cofres de serviços de recuperação Center DPM, Windows Server, servidor de cópia de segurança do Azure e muito mais. Os cofres dos Serviços de Recuperação facilitam a organização dos dados de cópia de segurança ao minimizar os custos de gestão.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Cofres dos serviços de recuperação comparando e cofres de cópia de segurança
Os cofres dos serviços de recuperação baseiam-se no modelo do Azure Resource Manager do Azure, ao passo que os cofres de cópia de segurança baseiam-se no modelo do Azure Service Manager. Quando atualizar um cofre de cópia de segurança para um cofre dos serviços de recuperação, os dados de cópia de segurança permanecem intactos durante e após o processo de atualização. Os cofres dos serviços de recuperação fornecem funcionalidades não disponíveis para os cofres de cópia de segurança, tais como:

- **Recursos para ajudar a proteger os dados da cópia de segurança aprimorados**: Com cofres dos serviços de recuperação, a cópia de segurança do Azure fornece recursos de segurança para proteger cópias de segurança na cloud. Estas funcionalidades de segurança, certifique-se que pode proteger as suas cópias de segurança e recuperar com segurança os dados de cópias de segurança na cloud, mesmo se os servidores de produção e cópia de segurança sejam comprometidos. [Saiba mais](backup-azure-security-feature.md)

- **Monitorização central para seu ambiente de TI híbrido**: Com os cofres dos serviços de recuperação, pode monitorizar não apenas sua [VMs IaaS do Azure](backup-azure-manage-vms.md) , mas também sua [recursos no local](backup-azure-manage-windows-server.md#manage-backup-items) partir de um portal central. [Saiba mais](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Controlo de acesso baseado em funções (RBAC)**: RBAC fornece controlo de gestão de acesso detalhado no Azure. [O Azure fornece várias funções incorporadas](../role-based-access-control/built-in-roles.md), e o Azure Backup tem três [funções incorporadas para gerir pontos de recuperação](backup-rbac-rs-vault.md). Os cofres dos serviços de recuperação são compatíveis com o RBAC, que restringe a cópia de segurança e restaurar o acesso para o conjunto definido de funções de utilizador. [Saiba mais](backup-rbac-rs-vault.md)

- **Proteger todas as configurações das máquinas virtuais Azure**: Os cofres dos serviços de recuperação protegem VMs com base no Gestor de recursos, incluindo os discos Premium, o Managed Disks e VMs encriptadas. Atualizar um cofre de cópia de segurança para um cofre dos serviços de recuperação dá-lhe a oportunidade de atualizar as VMs com base no Service Manager para VMs baseadas no Resource Manager. Durante a atualização do cofre, pode reter os pontos de recuperação VM baseada no Service Manager e configure a proteção para as VMs (Resource Manager-ativado) atualizadas. [Saiba mais](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Restauro imediato para IaaS VMs**: Utilizar cofres dos serviços de recuperação, pode restaurar ficheiros e pastas a partir de uma VM de IaaS sem restaurar a VM inteira, que permite que os tempos de restauro mais rápidos. Restauro imediato para IaaS VMs está disponível para VMs do Linux e Windows. [Saiba mais](https://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> Se tiver registados para um cofre de cópia de segurança com o agente MARS anteriores ao 2.0.9083.0, de itens [transferir o agente de MARS mais recente]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) versão para tirar as vantagens de todos os recursos do cofre dos serviços de recuperação. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Gerir os cofres de serviços de recuperação
Os ecrãs seguintes mostram um novo cofre de serviços de recuperação, atualizado a partir do Cofre de cópia de segurança, no portal do Azure. O Cofre atualizado estarão presente num grupo de recursos padrão com o nome "Geo-predefinido-RecoveryServices-ResourceGroup". Exemplo: Se o Cofre de cópia de segurança foi localizado na região E.U.A. oeste, será colocado cópia de segurança num padrão RG com o nome predefinido-RecoveryServices-ResourceGroup-westus.
> [!NOTE]
> Para clientes da norma CPS, o grupo de recursos não é alterado após a atualização do cofre e permanece igual ao que era antes da atualização.

A primeira tela mostra o dashboard do cofre que exibe a entidades-chave do cofre.
![exemplo do Cofre de serviços de recuperação atualizado a partir de um cofre de cópia de segurança](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

O segundo ecrã mostra a ajuda de ligações disponíveis para ajudá-lo a começar a utilizar o Cofre dos serviços de recuperação.

![ligações de ajuda, no painel início rápido](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Passos pós-atualização
Cofre dos Recovery Services suporta a especificação de informações de fuso horário na política de cópia de segurança. Após a atualização com êxito do cofre, vá para políticas de cópia de segurança no menu de definições do cofre e atualizar as informações de fuso horário para cada uma das políticas configuradas no cofre. Este ecrã já mostra o tempo de agenda de cópia de segurança especificado como por fuso horário local utilizado quando criou a política. 

## <a name="enhanced-security"></a>Segurança avançada
Quando um cofre de cópia de segurança é atualizado para um cofre dos serviços de recuperação, as definições de segurança para esse cofre automaticamente são ativadas. Quando as definições de segurança estão em determinadas operações como a eliminar as cópias de segurança ou alterar uma frase de acesso necessitar de um [multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) PIN. Para obter mais informações sobre a segurança avançada, consulte o artigo [recursos de segurança para proteger cópias de segurança híbridas](backup-azure-security-feature.md). Quando a segurança avançada estiver ativada, os dados são mantidos até 14 dias após as informações de ponto de recuperação foi eliminadas do cofre. Os clientes são faturados para o armazenamento destes dados de segurança. Retenção de dados de segurança se aplica a pontos de recuperação direcionados para o agente de cópia de segurança do Azure, Azure Backup Server e System Center Data Protection Manager. 

## <a name="gather-data-on-your-vault"></a>Recolher dados do seu Cofre
Uma vez que atualizar para um cofre dos serviços de recuperação, configurar relatórios para o Azure Backup (para VMs de IaaS e o Microsoft Azure Recovery Services agent) e utiliza o Power BI para aceder aos relatórios. Para obter informações adicionais sobre a recolha de dados, consulte o artigo [configurar a cópia de segurança do Azure comunica](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**O plano de atualização afeta a minha cópias de segurança em curso?**</br>
Não. As cópias de segurança em curso continuam sem interrupções durante e após a atualização.

**O que isto significa atualização para minhas ferramentas existentes?**</br>
Tem de atualizar sua automatização existente ou as ferramentas para o modelo de implementação do Resource Manager para garantir que continuam a funcionar após a atualização. Consulte as referências de cmdlets do PowerShell para o [modelo de implementação do Resource Manager](backup-client-automation.md).

**Posso reverter após a atualização?**</br>
Não. Reversão não é suportada depois dos recursos foram atualizados com êxito.

**Pode ver meu cofre clássico depois da atualização?**</br>
Não. Não é possível ver ou gerir o seu Cofre clássico depois da atualização. Só poderá utilizar o novo portal do Azure para todas as ações de gestão no cofre.

**Por que motivo não vejo servidores protegidos pelo agente de MARS no meu cofre atualizado?**</br>
Tem de instalar o agente de MARS mais recente para ver todos os servidores protegidos pelo agente de MARS no seu cofre. Pode baixar a versão mais recente do agente do [aqui]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).

**Não consigo ver a política de cópia de segurança para os servidores protegidos pelo agente de MARS após a atualização**</br>
Política de cópia de segurança do cofre pode ser desatualizada e, portanto, não foi possível sincronizar para o Cofre atualizado. Atualize a política para garantir a que continuar a ver as suas políticas no cofre atualizado.
Para atualizar a política, vá para o agente MARS e atualizar a política de cópia de segurança configurada.

**Por que não é possível atualizar meu política de cópia de segurança após a atualização?**</br>
Isto acontece quando está um agente de cópia de segurança antigo e selecione o período de retenção mínimo para ser inferior ao valor mínimo permitido. Quando um cofre de cópia de segurança é atualizado para um cofre dos serviços de recuperação, as definições de segurança para esse cofre automaticamente são ativadas. Para garantir que há sempre um número válido de pontos de recuperação disponíveis, existe algum período de retenção mínimo tem de ser mantidos de acordo com a funcionalidade de segurança. Para obter mais detalhes, consulte [aqui](backup-azure-security-feature.md).
Além disso, tem de atualizar os agentes de cópia de segurança do Azure para a versão mais recente para tirar as vantagens dos recursos mais recentes do Azure Backup.

**Posso atualizar meu agente, mas ainda não é possível ver todos os objetos que está a ser sincronizados, até mesmo dias, após a atualização**</br>
Verifique se se registrou o mesmo computador para vários cofres. Certifique-se de que está a visualizar o mesmo cofre no qual o agente MARS está registado. Para saber qual cofre o agente MARS está registado, abra o registo do Windows e verifique o valor de chave de ServiceResourceName em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config cofre registrado para que o agente de MARS aparecerá daí. Se a chave de ServiceResourceName não estiver visível no seu sistema, contacte-nos com o valor das chaves ResourceId e MachineId em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config e iremos ajudar a resolver o problema.

**Por que motivo não vejo as informações de tarefas para os meus recursos após a atualização?**</br>
Monitorização de cópias de segurança (agente MARS e IaaS) é uma nova funcionalidade que obtém quando atualizar o seu Cofre de cópia de segurança para cofre dos serviços de recuperação. As informações de monitorização demora até 12 horas para sincronizar com o serviço.

**Como posso comunicar um problema?**</br>
Se qualquer parte da atualização do cofre falhar, tenha em atenção que o OperationId listados no erro. Microsoft Support proativamente funcionará para resolver o problema. Pode contactar o suporte ou envie um e-mail para rsvaultupgrade@service.microsoft.com com o ID de subscrição, nome do cofre e OperationId. Vamos tentar resolver o problema mais depressa possível. Não repita a operação, a menos que explicitamente instruído para fazê-lo pela Microsoft.

## <a name="next-steps"></a>Passos Seguintes
Utilize os seguintes artigos para:</br>
[Fazer uma cópia de segurança de uma VM de IaaS](backup-azure-arm-vms-prepare.md)</br>
[Fazer uma cópia de segurança de um servidor de cópia de segurança do Azure](backup-azure-microsoft-azure-backup.md)</br>
[Fazer cópias de segurança do Windows Server](backup-configure-vault.md)
