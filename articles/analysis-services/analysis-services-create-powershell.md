---
title: Início Rápido - Criar um servidor do Azure Analysis Services com o PowerShell | Microsoft Docs
description: Saiba como criar um servidor do Azure Analysis Services com o PowerShell
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8ce9606c6b59175e03da5382f76fb320080db86e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427031"
---
# <a name="quickstart-create-a-server---powershell"></a>Início Rápido: Criar um servidor - PowerShell

Este início rápido descreve a utilização do PowerShell a partir da linha de comandos para criar um servidor do Azure Analysis Services na sua subscrição do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**: aceda à [Avaliação Gratuita do Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) para criar uma conta.
- **Azure Active Directory**: a sua subscrição tem de estar associada a um inquilino do Azure Active Directory e tem de ter uma conta nesse diretório. Para saber mais,v eja [Authentication and user permissions](analysis-services-manage-users.md) (Autenticação e permissões de utilizador).
- **Versão 4.0 ou posterior do módulo do Azure PowerShell**. Para localizar a versão, execute ` Get-Module -ListAvailable AzureRM`. Para instalar ou atualizar, veja [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="import-azurermanalysisservices-module"></a>Importar o módulo AzureRm.AnalysisServices

Para criar um servidor na sua subscrição, utilize o módulo de componente [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Carregue o módulo AzureRm.AnalysisServices para a sessão do PowerShell.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure através do comando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). Siga as indicações no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. Quando criar o servidor, tem de especificar um grupo de recursos na sua subscrição. Caso ainda não tenha um grupo de recursos, pode criar um novo através do comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup` na região E.U.A. Oeste.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Criar um servidor

Crie um novo servidor através do comando [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). O exemplo seguinte cria um servidor com o nome myServer em myResourceGroup, na região E.U.A. Oeste, no escalão D1 (gratuito), e especifica philipc@adventureworks.com como administrador do servidor.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Limpar recursos

Pode remover o servidor da sua subscrição através do comando [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Se continuar com outros guias de início rápido e tutoriais nesta coleção, não remova o servidor. O exemplo seguinte remove o servidor que criou no passo anterior.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a criar um servidor na sua subscrição do Azure com o PowerShell. Agora que tem o servidor, pode ajudar a protegê-lo ao configurar uma firewall do servidor (opcional). Também pode adicionar um modelo de dados de exemplo básico ao seu servidor diretamente a partir do portal. Ter um modelo de exemplo é útil para saber como configurar as funções de base de dados do modelo e testar as ligações de cliente. Para obter mais informações, avance para o tutorial para adicionar um modelo de exemplo.

> [!div class="nextstepaction"]
> [Início Rápido: Configurar a firewall do servidor - Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Tutorial: Adicionar um modelo de exemplo ao servidor](analysis-services-create-sample-model.md)