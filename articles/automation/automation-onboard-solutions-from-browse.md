---
title: Saiba como integrar soluções de gestão de atualizações, controlo de alterações e inventário de várias VMs na automatização do Azure
description: Saiba como para carregar uma Azure Virtual máquina com soluções de gestão de atualizações, controlo de alterações e inventário que fazem parte da automatização do Azure
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 0a624d850b8c3260acb24cb17566090e8ad0043e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233942"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Ativar a gestão de atualizações, controlo de alterações e soluções de inventário em várias VMs

A automatização do Azure fornece soluções para gerir atualizações de segurança do sistema operativo, controlar as alterações e de inventário que está instalado nos seus computadores. Existem várias formas de carregar máquinas, pode carregar a solução [de uma máquina virtual](automation-onboard-solutions-from-vm.md), da sua [conta de automatização](automation-onboard-solutions-from-automation-account.md), ao procurar máquinas virtuais ou pelo [runbook](automation-onboard-solutions.md). Este artigo abrange integração estas soluções ao procurar máquinas virtuais no Azure.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie a sessão no Azure em https://portal.azure.com

## <a name="enable-solutions"></a>Ativar soluções

No portal do Azure, navegue para **máquinas virtuais**.

Utilizando as caixas de verificação, selecione as máquinas virtuais que pretende integrar com o controlo de alterações e de inventário ou de gestão de atualizações. Integração está disponível até três diferentes para grupos de recursos de cada vez.

![Lista de VMs](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Utilize os controlos de filtro para modificar a lista de máquinas virtuais e, em seguida, clique na caixa de verificação para selecionar todas as máquinas virtuais na lista superior.

Na barra de comandos, clique em **serviços** e selecione **alterações**, **inventário**, ou **gestão de atualizações**.

> [!NOTE]
> **Registo de alterações** e **inventário** utilizar a mesma solução, quando um está ativado o outro está ativado, bem como.

A imagem seguinte destina-se a gestão de atualizações. Registo de alterações e inventário têm o mesmo esquema e o mesmo comportamento.

A lista de máquinas virtuais é filtrada para mostrar apenas as máquinas virtuais que estão na mesma subscrição e localização. Se as máquinas virtuais estiverem em mais de três grupos de recursos, são selecionados os grupos de recursos de três primeiro.

Utilize os controlos de filtro para selecionar máquinas virtuais de diferentes subscrições, localizações e grupos de recursos.

![Carregar solução de gestão de atualizações](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Reveja as opções para a área de trabalho de análise de registo e a conta de automatização. Uma nova área de trabalho e a conta de automatização estão selecionadas por predefinição. Se tiver uma conta de automatização e área de trabalho de análise de registos existente que pretende utilizar, clique em **alterar** para selecioná-los a partir de **configuração** página. Quando terminar, clique em **Guardar**.

![Selecione a área de trabalho e de conta](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Desmarque a caixa de verificação junto de qualquer máquina virtual que não pretende ativar. Máquinas virtuais que não é possível ativar já estão a ser desmarcadas.

Clique em **ativar** para ativar a solução. A solução demora até 15 minutos a ativar.

## <a name="troubleshooting"></a>Resolução de problemas

Quando a integração várias máquinas, poderão existir máquinas que mostram como **não é possível ativar**. Existem diferentes motivos por que motivo algumas máquinas poderão não estar ativadas. As secções seguintes mostram algumas das razões possíveis para o **não é possível ativar** estado numa VM ao tentar carregar.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>Relatórios de VM para uma área de trabalho diferentes: '\<workspaceName\>'.  Alterar a configuração para utilizá-la para ativar

**Causa**: Este erro mostra que a VM que está a tentar carregar relatórios para outra área de trabalho.

**Solução**: clique em **utilização como configuração** para alterar a área de trabalho alvo de conta de automatização e análise de registos.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Relatórios VM para uma área de trabalho que não está disponível nesta subscrição

**Causa**: A área de trabalho que a máquina virtual relatórios para:

* Está numa subscrição diferente, ou
* Já não existe ou
* Está num grupo de recursos que não tem permissões de acesso a

**Solução**: localizar a conta de automatização carregar e associado à área de trabalho que a VM relatórios para a máquina virtual ao alterar a configuração do âmbito.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>Versão do sistema operativo VM ou distribuição não é suportada

**Causa:** a solução não é suportada para todas as distribuições de Linux ou todas as versões do Windows.

**Solução:** referir-se ao [lista de clientes suportados](automation-update-management.md#clients) para a solução.

### <a name="classic-vms-cannot-be-enabled"></a>Não não possível ativar a VMs clássicas

**Causa**: não são suportadas máquinas virtuais que utilizam o modelo de implementação clássica.

**Solução**: migrar a máquina virtual para o modelo de implementação do resource manager. Para saber como fazê-lo, consulte o artigo [migrar os recursos do modelo de implementação clássica](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>VM se encontra parada. (desalocado)

**Causa**: À máquina virtual não se encontra num **executar** estado.

**Solução**: para carregar uma VM para uma solução a VM deve estar em execução. Clique em de **iniciar VM** ligação inline para iniciar a VM sem navegar sair da página.

## <a name="next-steps"></a>Passos Seguintes

Agora que a solução estiver ativada para as máquinas virtuais, visite o artigo de descrição geral da gestão de atualização para saber como ver a avaliação de atualização para as suas máquinas.

> [!div class="nextstepaction"]
> [Gestão de atualizações - avaliação de atualização de vista](./automation-update-management.md#viewing-update-assessments)

Tutoriais de adição sobre as soluções e como utilizá-los:

* [Tutorial - Gerir atualizações para a VM](automation-tutorial-update-management.md)

* [Tutorial - identificar o software numa VM](automation-tutorial-installed-software.md)

* [Tutorial - alterações numa VM de resolução de problemas](automation-tutorial-troubleshoot-changes.md)