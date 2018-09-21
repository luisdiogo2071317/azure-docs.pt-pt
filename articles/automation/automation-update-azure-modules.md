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
ms.openlocfilehash: bee1c5c48242b69ee33fedd358a83e0580d19942
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498056"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Como atualizar módulos do Azure PowerShell na automatização do Azure

Os módulos Azure PowerShell mais comuns são fornecidos por predefinição em cada conta de automatização. A equipa do Azure atualiza regularmente, os módulos do Azure, para que na conta de automatização é fornecida uma forma de atualizar os módulos na conta quando existem novas versões do portal.

Uma vez que os módulos são atualizados regularmente pelo grupo de produtos, as alterações podem ocorrer com os cmdlets incluídos, que pode afetar negativamente os runbooks dependendo do tipo de alteração, tal como mudar o nome de um parâmetro ou descontinuar um cmdlet inteiramente. Para evitar ter impacto nos seus runbooks e os processos que automatizar, recomenda-se que teste e validar antes de continuar. Se não tiver uma conta de automatização dedicada a finalidade para esta finalidade, considere a criação de um para que pode testar vários cenários diferentes e permutas durante o desenvolvimento dos seus runbooks, além do mais às alterações iterativas, como atualizar o Módulos do PowerShell. Depois dos resultados são validados e tiver efetuado quaisquer alterações necessárias, continuar com a coordenar a migração de todos os runbooks que exigiam modificação e realizar a seguinte atualização, conforme descrito na produção.

> [!NOTE]
> Uma nova conta de automatização não pode conter os módulos mais recentes.

## <a name="updating-azure-modules"></a>Atualizar módulos do Azure

1. Na página de módulos de sua conta de automatização, há uma opção chamada **módulos do Azure de atualização**. Ele está sempre ativado.<br><br> ![Atualizar a opção de módulos do Azure na página de módulos](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

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

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como criar módulos personalizados para integrar mais automação com outros sistemas, serviços ou soluções e os módulos de integração, consulte [módulos de integração](automation-integration-modules.md).

* Considere a utilização de integração de controlo de origem [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) ou [do Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) centralmente, gerir e controlar as versões do seu portfólio de configuração e de runbook de automatização.  
