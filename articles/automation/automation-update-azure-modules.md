---
title: Atualizar módulos do Azure na automatização do Azure
description: Este artigo descreve como agora pode atualizar os módulos Azure PowerShell comuns fornecidos por predefinição na automatização do Azure.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f461bc1fd17ee957be5f223de731608011d021f5
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959364"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Como atualizar módulos do Azure PowerShell na automatização do Azure

Os módulos Azure PowerShell mais comuns são fornecidos por predefinição em cada conta de automatização. A equipa do Azure atualiza regularmente o os módulos do Azure. Na sua conta de automatização, é apresentada uma forma de atualizar os módulos na conta quando existem novas versões do portal.

Uma vez que os módulos são atualizados regularmente pelo grupo de produtos, as alterações podem ocorrer com os cmdlets incluídos. Esta ação pode afetar negativamente os runbooks dependendo do tipo de alteração, tal como mudar o nome de um parâmetro ou descontinuar um cmdlet inteiramente. Para evitar ter impacto nos seus runbooks e os processos que automatizar, testar e validar antes de continuar. Se não tiver uma conta de automatização dedicada a finalidade para esta finalidade, considere a criação de um para que pode testar vários cenários diferentes durante o desenvolvimento dos seus runbooks. Esse teste deve incluir alterações iterativas, como atualizar os módulos do PowerShell. Se desenvolver seus scripts localmente, recomenda-se de ter as mesmas versões do módulo localmente que tem na sua conta de automatização quando um teste para garantir que receberá os mesmos resultados. Depois dos resultados são validados e aplicou as alterações necessárias, pode mover as alterações para produção.

> [!NOTE]
> Uma nova conta de automatização não pode conter os módulos mais recentes.

## <a name="updating-azure-modules"></a>Atualizar módulos do Azure

1. Na página de módulos de sua conta de automatização, há uma opção chamada **módulos do Azure de atualização**. Ele está sempre ativado.<br><br> ![Atualizar a opção de módulos do Azure na página de módulos](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Antes de atualizar seus módulos do Azure, que é recomendado que Atualize-os num teste de conta de automatização para garantir que seus scripts existentes funcionam conforme esperado antes de atualizar seus módulos do Azure.
  >
  > O **módulos do Azure de atualização** botão só está disponível na cloud pública. não está disponível na [regiões de soberania](https://azure.microsoft.com/global-infrastructure/). Veja [maneiras alternativas de atualizar seus módulos](#alternative-ways-to-update-your-modules) secção para saber mais.

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

    Se os módulos já estão atualizados, em seguida, o processo for concluído dentro de alguns segundos. Quando tiver concluído o processo de atualização, receberá uma notificação.<br><br> ![Atualizar o estado de atualização de módulos do Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    Os módulos de AzureRm do .NET core (azurerm. *. Núcleos) não são suportados na automatização do Azure e não pode ser importado.

> [!NOTE]
> A automatização do Azure utiliza os módulos mais recentes na sua conta de automatização quando é executada uma nova tarefa agendada.  

Se utilizar cmdlets destes módulos do PowerShell do Azure nos runbooks, quer para executar este processo de atualização de todos os meses, ou então, para se certificar de que tem os módulos mais recentes. A automatização do Azure utiliza o `AzureRunAsConnection` ligação para se autenticar ao atualizar os módulos. Se o principal de serviço expirou ou já não existe no nível da subscrição, a atualização de módulo irá falhar.

## <a name="alternative-ways-to-update-your-modules"></a>Maneiras alternativas de atualizar os módulos

Conforme mencionado, o **módulos do Azure de atualização** botão não está disponível em Clouds soberanas, só está disponível na cloud do Azure global. Isso é que a versão mais recente dos módulos do PowerShell do Azure da galeria do PowerShell poderá não funcionar com os recursos de Gestor de recursos atualmente implementados nessas nuvens.

Pode importar e executar o [AzureModule.ps1 atualização](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook para tentar atualizar os módulos do Azure na sua conta de automatização. Em geral, é uma boa idéia para atualizar todos os módulos do Azure ao mesmo tempo. No entanto, este processo pode falhar se as versões que está a tentar importar a partir da Galeria não ser compatível com os serviços do Azure atualmente implementados para o ambiente do Azure de destino. Isso pode exigir que verifique se que as versões compatíveis do módulos são especificadas nos parâmetros do runbook.

Utilize o `AzureRmEnvironment` parâmetro para passar o ambiente correto para o runbook.  Os valores aceitáveis são **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, e **AzureUSGovernment**. Estes valores podem ser recuperados do usando `Get-AzureRmEnvironment | select Name`. Se não passar um valor para este parâmetro, o runbook será predefinido para a cloud pública do Azure **AzureCloud**

Se pretender utilizar uma versão do módulo Azure PowerShell específica em vez de mais recente disponível na galeria do PowerShell, passar essas versões para o opcional `ModuleVersionOverrides` parâmetro do **atualização AzureModule** runbook. Para obter exemplos, consulte a [AzureModule.ps1 atualização](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. Os módulos do Azure PowerShell que não são mencionados no `ModuleVersionOverrides` parâmetro são atualizados com as versões mais recentes do módulo na galeria do PowerShell. Se passar nada para o `ModuleVersionOverrides` parâmetro, todos os módulos são atualizados com as versões mais recentes do módulo na galeria do PowerShell. Este comportamento é igual a **módulos do Azure de atualização** botão.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como criar módulos personalizados para integrar mais automação com outros sistemas, serviços ou soluções e os módulos de integração, consulte [módulos de integração](automation-integration-modules.md).

