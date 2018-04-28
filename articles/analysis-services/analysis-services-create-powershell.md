---
title: Criar um servidor do Azure Analysis Services com o PowerShell | Microsoft Docs
description: Saiba como criar um servidor do Azure Analysis Services com o PowerShell
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3f0d3ae6786e9f63f0e4eb025118d0d217eced64
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Criar um servidor do Azure Analysis Services com o PowerShell

Este guia de início rápido descreve a utilização do PowerShell a partir da linha de comandos para criar um servidor do Azure Analysis Services num [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) na sua subscrição do Azure.

Esta tarefa requer a versão 4.0 ou posterior do módulo do Azure PowerShell. Para localizar a versão, execute ` Get-Module -ListAvailable AzureRM`. Para instalar ou atualizar, veja [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

> [!NOTE]
> A criação de um servidor poderá resultar num novo serviço sujeito a faturação. Para obter mais informações, veja [Preços do Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="before-you-begin"></a>Antes de começar
Para concluir este guia de início rápido, necessita de:

* **Subscrição do Azure**: aceda à [Avaliação Gratuita do Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) para criar uma conta.
* **Azure Active Directory**: a sua subscrição tem de estar associada a um inquilino do Azure Active Directory e tem de ter uma conta nesse diretório. Para saber mais,v eja [Authentication and user permissions](analysis-services-manage-users.md) (Autenticação e permissões de utilizador).

## <a name="import-azurermanalysisservices-module"></a>Importar o módulo AzureRm.AnalysisServices
Para criar um servidor na sua subscrição, utilize o módulo de componente [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Carregue o módulo AzureRm.AnalysisServices para a sessão do PowerShell.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão sua subscrição do Azure utilizando o [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) comando. Siga as indicações no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. Quando criar o servidor, tem de especificar um grupo de recursos na sua subscrição. Caso ainda não tenha um grupo de recursos, pode criar um novo através do comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup` na região E.U.A. Oeste.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Criar um servidor

Crie um novo servidor através do comando [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). O exemplo seguinte cria um servidor com o nome myServer em myResourceGroup, na região E.U.A. Oeste, no escalão D1, e especifica philipc@adventureworks.com como administrador do servidor.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Limpar recursos

Pode remover o servidor da sua subscrição através do comando [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Se continuar com outros guias de início rápido e tutoriais nesta coleção, não remova o servidor. O exemplo seguinte remove o servidor que criou no passo anterior.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Passos Seguintes
[Gerir o Azure Analysis Services com o PowerShell](analysis-services-powershell.md)
[implementar um modelo do SSDT](analysis-services-deploy.md)
[criar um modelo no portal do Azure](analysis-services-create-model-portal.md)
