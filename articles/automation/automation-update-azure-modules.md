---
title: Atualizar módulos do Azure na automatização do Azure
description: Este artigo descreve como agora pode atualizar os módulos Azure PowerShell comuns fornecidos por predefinição na automatização do Azure.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fbb57753117f3c60010fe910616b8d0af5178360
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434828"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Como atualizar módulos do Azure PowerShell na automatização do Azure

Os módulos Azure PowerShell mais comuns são fornecidos por predefinição em cada conta de automatização. A equipa do Azure atualiza regularmente, os módulos do Azure, para que na conta de automatização é fornecida uma forma de atualizar os módulos na conta quando existem novas versões do portal.

Uma vez que os módulos são atualizados regularmente pelo grupo de produtos, as alterações podem ocorrer com os cmdlets incluídos, que pode afetar negativamente os runbooks dependendo do tipo de alteração, tal como mudar o nome de um parâmetro ou descontinuar um cmdlet inteiramente. Para evitar ter impacto nos seus runbooks e os processos que automatizar, recomenda-se que teste e validar antes de continuar. Se não tiver uma conta de automatização dedicada a finalidade para esta finalidade, considere a criação de um para que pode testar vários cenários diferentes e permutas durante o desenvolvimento dos seus runbooks, além do mais às alterações iterativas, como atualizar o Módulos do PowerShell. Depois dos resultados são validados e tiver efetuado quaisquer alterações necessárias, continuar com a coordenar a migração de todos os runbooks que exigiam modificação e realizar a seguinte atualização, conforme descrito na produção.

> [!NOTE]
> Uma nova conta de automatização não pode conter os módulos mais recentes.

## <a name="updating-azure-modules"></a>Atualizar módulos do Azure

1. Na página de módulos de sua conta de automatização, há uma opção chamada **módulos do Azure de atualização**. Ele está sempre ativado.<br><br> ![Atualizar a opção de módulos do Azure na página de módulos](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Antes de atualizar seus módulos do Azure, que é recomendado que Atualize-os num teste de conta de automatização para garantir que seus scripts existentes funcionam conforme esperado antes de atualizar seus módulos do Azure.
  >
  > O **módulos do Azure de atualização** botão só está disponível na cloud pública. Não está disponível na [regiões de soberania](https://azure.microsoft.com/global-infrastructure/). Veja [maneiras alternativas de atualizar seus módulos](#alternative-ways-to-update-your-modules) secção para saber mais.


2. Clique em **módulos do Azure de atualização**, é mostrada uma notificação de confirmação que pede-lhe se pretende continuar.<br><br> ![Atualizar notificação de módulos do Azure](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Clique em **Sim** e iniciar o processo de atualização do módulo. O processo de atualização demora cerca de 15 a 20 minutos para atualizar os seguintes módulos:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Se os módulos já estão atualizados, em seguida, o processo for concluído dentro de alguns segundos. Quando o processo de atualização estiver concluída, será notificado.<br><br> ![Atualizar o estado de atualização de módulos do Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    Os módulos de AzureRm do .NET core (azurerm. *. Núcleos) não são suportados na automatização do Azure e não pode ser importado.

> [!NOTE]
> A automatização do Azure utiliza os módulos mais recentes na sua conta de automatização quando é executada uma nova tarefa agendada.  

Se utilizar cmdlets destes módulos do PowerShell do Azure nos runbooks, quer para executar este processo de atualização de todos os meses, ou então, para se certificar de que tem os módulos mais recentes. Utilizações de automatização do Azure em que a ligação de AzureRunAsConnection para autenticar ao atualizar os módulos, se o principal de serviço expirou ou já não existe no nível da subscrição, a atualização de módulo irá falhar.

## <a name="alternative-ways-to-update-your-modules"></a>Maneiras alternativas de atualizar os módulos

Conforme mencionado, o **módulos do Azure de atualização** botão não está disponível nas Clouds soberanas, só está disponível na cloud do Azure global. Isso é que a versão mais recente dos módulos do PowerShell do Azure da galeria do PowerShell poderá não funcionar com os serviços do Gestor de recursos atualmente implementados nessas nuvens.

A atualizar os módulos ainda pode ser feita através da importação a [AzureModule.ps1 atualização](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook em sua conta de automatização e executá-lo.

Utilize o `AzureRmEnvironment` parâmetro para passar o ambiente correto para o runbook.  Os valores aceitáveis são **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, e **AzureUSGovernmentCloud**. Se não introduzir um valor para este parâmetro, o runbook será predefinido para a cloud pública do Azure **AzureCloud**.

Se pretender utilizar uma versão do módulo Azure PowerShell específica em vez de mais recente disponível na galeria do PowerShell, passar essas versões para o opcional `ModuleVersionOverrides` parâmetro do **atualização AzureModule** runbook. Para obter exemplos, consulte a [AzureModule.ps1 atualização](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. Os módulos do Azure PowerShell que tenham sido mencionados no `ModuleVersionOverrides` parâmetro são atualizados com as versões mais recentes do módulo na galeria do PowerShell. Se nada for passado para o `ModuleVersionOverrides` parâmetro, todos os módulos são atualizados com as versões mais recentes do módulo na galeria do PowerShell, que é o comportamento do **módulos do Azure de atualização** botão.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como criar módulos personalizados para integrar mais automação com outros sistemas, serviços ou soluções e os módulos de integração, consulte [módulos de integração](automation-integration-modules.md).

* Considere a utilização de integração de controlo de origem [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) ou [do Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) centralmente, gerir e controlar as versões do seu portfólio de configuração e de runbook de automatização.  
