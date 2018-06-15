---
title: A atualização automática do serviço de mobilidade na recuperação de desastre do Azure para o Azure | Microsoft Docs
description: Fornece uma descrição geral da atualização automática do serviço de mobilidade, ao replicar as VMs do Azure utilizando o Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/16/2018
ms.author: rajanaki
ms.openlocfilehash: add80b17c76e7262f55e50cd07d4e9b053cfa1ff
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209836"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Atualização automática do serviço de mobilidade na replicação do Azure para o Azure

O Azure Site Recovery tem uma cadência de versão mensal onde melhoramentos a funcionalidades existentes ou novas são adicionadas e problemas conhecidos se qualquer sejam corrigidos. Isto seria significa que permaneça atual com o serviço, terá de planear a implementação destas correções de erros, mensalmente. Para evitar no cabeçalho de ativação pós-falha associado com a atualização, os utilizadores em vez disso, podem optar por permitir a recuperação de sites gerir atualizações dos componentes. Como detalhadas no [referência de arquitetura](azure-to-azure-architecture.md) para recuperação de desastre do Azure para o Azure, o serviço de mobilidade obtém instalado em todas as máquinas virtuais do Azure para o qual a replicação está ativada ao replicar máquinas virtuais do um Azure região para outro. Depois de ativar a atualização automática, a extensão de serviço de mobilidade obtém atualizada com cada nova versão. Este documento fornece detalhes sobre o seguinte:

- Como funciona a atualização automática?
- Ativar as atualizações automáticas
- Problemas comuns e resolução de problemas
 
## <a name="how-does-automatic-update-work"></a>Como funciona a atualização automática

Uma vez que permite que a recuperação de sites gerir as atualizações, um runbook global (que é utilizado pelos serviços do Azure) é implementado através de uma conta de automatização, que é criada na mesma subscrição do cofre. Uma conta de automatização é utilizada para um cofre específico. O runbook verifica cada VM num cofre para a qual automaticamente atualizações estão ativadas e inicia uma atualização da extensão do serviço de mobilidade esteja disponível uma versão mais recente. A agenda predefinida de recurrs o runbook diariamente, às 12:00:00 de acordo com o fuso horário do georreplicação a máquina virtual replicada. O agendamento do runbook pode também ser modificado através da conta de automatização pelo utilizador, se necessário. 

> [!NOTE]
> Ativar atualizações automáticas não requer um reinício do suas VMs do Azure e não afeta a replicação em curso.

## <a name="enable-automatic-updates"></a>Ativar as atualizações automáticas

Pode optar por permitir a recuperação de Site gerir as atualizações das seguintes formas:-

- [Como parte do passo ativar replicação](#as-part-of-the-enable-replication-step)
- [Ativar/desativar a extensão de atualizar as definições no interior do Cofre](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Como parte do passo ativar replicação:

Quando ativar a replicação para a máquina virtual ou iniciar [da vista de máquina virtual](azure-to-azure-quickstart.md), ou [do Cofre de serviços de recuperação](azure-to-azure-how-to-enable-replication.md), obterá uma opção para optar por permitir a recuperação de sites para: gerir as atualizações para a extensão de recuperação de sites ou para gerir a mesma manualmente.

![Enable-replicação-atualização automática](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Ativar/desativar a extensão de atualizar as definições no interior do Cofre

1. Dentro do cofre, navegue para **gerir**-> **infraestrutura de recuperação de sites**
2. Em **virtual Machines do Azure para**-> **definições de atualização da extensão**, clique no botão de alternar para escolher se pretende permitir *ASR para gerir atualizações* ou *gerir manualmente*. Clique em **Guardar**.

![cofre alternar-autuo-atualização](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Quando escolhe *permitir ASR para gerir*, a definição é aplicada a todas as máquinas virtuais no cofre correspondente.


> [!Note] 
> Ambas as opções notificá-lo da conta de automatização que é utilizada para gerir as atualizações. Se pretende ativar esta funcionalidade pela primeira vez num cofre, será criada uma nova conta de automatização. Todas as replicações ativar subsequentes no mesmo cofre irão utilizar um criado anteriormente.

## <a name="common-issues--troubleshooting"></a>Problemas comuns e resolução de problemas

Se existir um problema com as atualizações automáticas, será notificado do mesmo em 'Problemas de configuração' no dashboard do cofre. 

No caso de tentativa ativar as atualizações automáticas e ter falhado, consulte abaixo para resolução de problemas.

**Erro**: não tem permissões para criar uma conta Run As do Azure (principal de serviço) e conceder a função de contribuinte ao principal de serviço. 
- Ação recomendada: Certifique-se de que a conta com sessão iniciada é atribuída o contribuinte e repita a operação.
 
Depois das atualizações automáticas são ativadas, a maioria dos problemas podem ser healed pelo serviço de recuperação de sites e requer que a clicar no '**reparação**' botão.

![botão de reparação](./media/azure-to-azure-autoupdate/repair.png)

No caso de botão de reparação não estiver disponível, consulte a mensagem de erro apresentada no painel de definições de extensão.

 - **Erro**: A conta Run as não tem permissão para aceder ao recurso de serviços de recuperação.

    **Ação recomendada**: eliminar e, em seguida, [voltar a criar a conta Run As](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account) ou certifique-se de que a do Azure Active Directory aplicação automatização Run As da conta tem acesso ao recurso de serviços de recuperação.

- **Erro**: conta Run As não foi encontrada. Um destes foi eliminado ou não criado - aplicação do Azure Active Directory, a Principal de serviço, a função de recurso de certificado de automatização, recurso de ligação de automatização - ou o Thumbprint não é idêntico entre certificado e a ligação. 

    **Ação recomendada**: eliminar e [, em seguida, voltar a criar a conta Run As](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account).
