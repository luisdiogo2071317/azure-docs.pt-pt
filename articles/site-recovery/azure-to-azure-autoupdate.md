---
title: Atualização automática do serviço de mobilidade na recuperação após desastre do Azure | Documentos da Microsoft
description: Fornece uma descrição geral da atualização automática do serviço de mobilidade, ao replicar VMs do Azure com o Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: f31fccd2bf6d0daae03b025b53a41a0fad4ce2ef
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210136"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Atualização automática do serviço de mobilidade na replicação do Azure para o Azure

O Azure Site Recovery tem uma cadência de lançamento mensal onde melhoramentos às funcionalidades existentes ou novos são adicionados e problemas conhecidos, se houver são fixos. Isso significaria que permaneça atualizado com o serviço, precisa planejar a implantação desses patches, mensalmente. Para evitar o cabeçalho de ativação pós-falha associado com a atualização, os utilizadores em vez disso, podem optar por permitir a recuperação de Site Gerir atualizações dos componentes. Conforme detalhado no [referência de arquitetura](azure-to-azure-architecture.md) para recuperação após desastre do Azure, instalar o serviço de mobilidade em todas as máquinas virtuais do Azure para o qual os replicação é ativada ao replicar máquinas virtuais do um Azure região para outra. Depois de ativar a atualização automática, a extensão de serviço de mobilidade é atualizada com cada nova versão. Este documento detalha o seguinte:

- Como funciona a atualização automática?
- Ativar as atualizações automáticas
- Problemas comuns e resolução de problemas
 
## <a name="how-does-automatic-update-work"></a>Como funciona a atualização automática

Permitir a recuperação de sites gerir as atualizações, um runbook global (que é utilizado pelos serviços do Azure) é implementado através de uma conta de automatização, que é criada na mesma subscrição que o cofre. Uma conta de automatização é utilizada para um cofre específico. O runbook verifica a existência de cada VM num cofre para o qual é atualizado automaticamente está ativados e inicia uma atualização da extensão do serviço de mobilidade, se uma versão mais recente está disponível. A agenda predefinida do runbook voltar a ocorrer diariamente às 12:00 AM de acordo com o fuso horário da geo da máquina virtual replicada. O agendamento do runbook também pode ser modificado por meio da conta de automatização pelo usuário, se necessário. 

> [!NOTE]
> Ativar atualizações automáticas não requer um reinício das suas VMs do Azure e não afeta a replicação em curso.

> [!NOTE]
> A faturação das tarefas utilizados pela conta de automatização baseia-se no número de minutos utilizados no mês e, por predefinição, 500 minutos estão incluídos como unidades gratuitas para uma conta de automatização de tempo de execução da tarefa. A execução das quantidades diárias de tarefa de um **alguns segundos para cerca de um minuto** e será **abordados os créditos gratuitos**.

UNIDADES gratuitas incluídas (por mês) * * ₹0.14 de 500 minutos de tempo de execução de tarefa de preço / minuto

## <a name="enable-automatic-updates"></a>Ativar as atualizações automáticas

Pode optar por permitir a recuperação de Site gerir as atualizações das seguintes formas:-

- [Como parte do passo ativar replicação](#as-part-of-the-enable-replication-step)
- [Ativar/desativar a extensão de atualizar as definições no interior do Cofre](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Como parte do passo ativar replicação:

Quando ativa a replicação para a máquina virtual a partir de qualquer [da vista de máquina virtual](azure-to-azure-quickstart.md), ou [partir do Cofre de serviços de recuperação](azure-to-azure-how-to-enable-replication.md), obterá uma opção para optar por permitir a recuperação de sites para Gerir atualizações para a extensão de recuperação de sites ou gerir manualmente a mesma.

![enable-replication-auto-update](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Ativar/desativar a extensão de atualizar as definições no interior do Cofre

1. No interior do cofre, navegue até **Manage**-> **infraestrutura do Site Recovery**
2. Sob **máquinas virtuais do Azure para**-> **definições de atualização da extensão**, clique o botão de alternar para escolher se pretende permitir *ASR para gerir atualizações* ou *gerir manualmente*. Clique em **Guardar**.

![cofre-Ativar/desativar-atualização automática](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Quando escolhe *permitir que o ASR para gerir*, a configuração é aplicada a todas as máquinas virtuais no cofre correspondente.


> [!Note] 
> Ambas as opções a notificá-lo a conta de automatização que é usada para gerenciar as atualizações. Se pretende ativar esta funcionalidade pela primeira vez num cofre, será criada uma nova conta de automatização. Todas as replicações de enable subsequentes no mesmo cofre usará o criado anteriormente.

### <a name="manage-manually"></a>Gerir manualmente

1. Se existem novas atualizações disponíveis para o serviço de mobilidade instalado em suas VMs do Azure, verá uma notificação que lê a "atualização do agente de replicação de recuperação do novo Site está disponível. Clique para instalar."

     ![Janela de itens replicados](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)
3. Selecione a notificação para abrir a página de seleção de máquina virtual.
4. Selecione as máquinas virtuais que pretende atualizar o serviço de mobilidade no e selecione **OK**.

     ![Lista de VMS de itens de replicados](./media/vmware-azure-install-mobility-service/update-okpng.png)

A tarefa de serviço de mobilidade de atualização é iniciada para cada uma das máquinas virtuais selecionadas.


## <a name="common-issues--troubleshooting"></a>Problemas comuns e resolução de problemas

Se houver um problema com as atualizações automáticas, será notificado das mesmas em "Problemas de configuração" no dashboard do cofre. 

No caso de tentou ativar as atualizações automáticas e falha, consulte abaixo para resolução de problemas.

**Erro**: Não tem permissões para criar uma conta Run As do Azure (principal de serviço) e conceder a função de Contribuinte ao principal de serviço. 
- Ação recomendada: Certifique-se de que a conta com sessão iniciada é atribuída a "contribuinte" e repita a operação. Consulte a [isso](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) documento para obter mais informações sobre a atribuir as permissões corretas.
 
Depois das atualizações automáticas são ativadas, a maioria dos problemas pode ser recuperada pelo serviço Site Recovery e requer que clicar no '**reparação**' botão.

![botão de reparação](./media/azure-to-azure-autoupdate/repair.png)

No caso do botão de reparação não estiver disponível, consulte a mensagem de erro exibida no painel de definições de extensão.

 - **Erro**: A conta Run As não tem permissão para aceder ao recurso de serviços de recuperação.

    **Ação recomendada**: Eliminar e, em seguida [voltar a criar a conta Run As](https://docs.microsoft.com/azure/automation/automation-create-runas-account) ou certifique-se de que o do Azure Active Directory do aplicativo Run As de automatização da conta tem acesso ao recurso de serviços de recuperação.

- **Erro**: Não foi encontrada a conta Run As. Um destes foi eliminado ou não criado - aplicação de diretório do Azure Active Directory, Principal de serviço, função, recurso de certificado de automatização, recurso de ligação de automatização - ou o Thumbprint não é idêntico entre o certificado e a ligação. 

    **Ação recomendada**: Eliminar e [, em seguida, voltar a criar a conta Run As](https://docs.microsoft.com/azure/automation/automation-create-runas-account).
