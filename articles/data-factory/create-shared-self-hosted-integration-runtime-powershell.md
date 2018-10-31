---
title: Criar um runtime de integração autoalojado partilhado na fábrica de dados do Azure com o PowerShell | Documentos da Microsoft
description: Saiba como criar um runtime de integração autoalojado partilhado no Azure Data Factory, que permite acesso de fábricas de dados múltiplos, o runtime de integração.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: abnarain
ms.openlocfilehash: d7f3fbcb3235c8c620876e68a62f3e491770779d
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252139"
---
# <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory-with-powershell"></a>Criar um runtime de integração autoalojado partilhado na fábrica de dados do Azure com o PowerShell

Este guia passo a passo mostra-lhe como criar um runtime de integração autoalojado partilhada (IR) no Azure Data Factory com o Azure PowerShell. Em seguida, pode utilizar o runtime de integração autoalojado partilhado na fábrica de dados de outro. Neste tutorial, vai executar os seguintes passos: 

1. Criar uma fábrica de dados. 
1. Criar um integration runtime autoalojado.
1. Partilhe o runtime de integração autoalojado com outras fábricas de dados.
1. Crie um runtime de integração ligado.
1. Revogar o compartilhamento.

## <a name="prerequisites"></a>Pré-requisitos 

- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar. 

- **Azure PowerShell**. Siga as instruções em [instalar o Azure PowerShell no Windows](/powershell/azure/install-azurerm-ps). Utilizar o PowerShell para executar um script para criar um runtime de integração autoalojado que pode ser partilhado com outras fábricas de dados. 

> [!NOTE]
> Para obter uma lista de regiões do Azure em que a fábrica de dados está atualmente disponível, selecione as regiões que lhe interessam, na página seguinte: [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory).

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie o ISE do Windows PowerShell.

1. Crie variáveis.

    Copie e cole o seguinte script e substitua as variáveis (SubscriptionName, ResourceGroupName, etc.) com valores reais. 

    ```powershell
    # If input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
    $SubscriptionName = "[Azure subscription name]" 
    $ResourceGroupName = "[Azure resource group name]" 
    $DataFactoryLocation = "EastUS" 

    # Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
    # Data factory name. Must be globally unique 
    $SharedDataFactoryName = "[Shared Data factory name]" 
    $SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
    $SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

    # Linked integration runtime information. This is a Data Factory compute resource for running any activities
    # Data factory name. Must be globally unique
    $LinkedDataFactoryName = "[Linked Data factory name]"
    $LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
    $LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"
    ```

1. Inicie sessão e selecione uma subscrição.

    Adicione o seguinte código ao script para iniciar sessão e selecione a sua subscrição do Azure:

    ```powershell
    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

1. Crie um grupo de recursos e uma fábrica de dados.

    *(Este passo é opcional. Se já tiver uma fábrica de dados, ignore este passo.)* 

    Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup` na localização WestEurope. 

    ```powershell
    New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
    ```

    Execute o comando seguinte para criar uma fábrica de dados: 

    ```powershell
    Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                             -Location $DataFactoryLocation `
                             -Name $SharedDataFactoryName
    ```

## <a name="create-a-self-hosted-integration-runtime"></a>Criar um integration runtime autoalojado

*(Este passo é opcional. Se já tiver o runtime de integração autoalojado que pretende partilhar com outras fábricas de dados, ignore este passo.)*

Execute o seguinte comando para criar um runtime de integração autoalojado:

```powershell
$SharedIR = Set-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription
```

### <a name="get-the-integration-runtime-authentication-key-and-register-a-node"></a>Obtenha a chave de autenticação do runtime de integração e registe-se um nó

Execute o seguinte comando para obter a chave de autenticação para o runtime de integração autoalojado:

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName
```

A resposta contém a chave de autenticação para este integration runtime autoalojado. Utilize esta chave quando registar o nó do integration runtime.

### <a name="install-and-register-the-self-hosted-integration-runtime"></a>Instalar e registar o runtime de integração autoalojado

1. Transferir o instalador do runtime de integração autoalojado partir [do Azure Data Factory Integration Runtime](https://aka.ms/dmg).

2. Execute o instalador para instalar a integração de autoalojado num computador local.

3. Registre-se a nova integração de autoalojado com a chave de autenticação que obteve no passo anterior.

## <a name="share-the-self-hosted-integration-runtime-with-another-data-factory"></a>Partilhar o runtime de integração autoalojado com outro fábrica de dados

### <a name="create-another-data-factory"></a>Criar fábrica de dados de outro

*(Este passo é opcional. Se já tiver a fábrica de dados com a qual pretende partilhar, ignore este passo.)*

```powershell
$factory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $LinkedDataFactoryName
```
### <a name="grant-permission"></a>Conceder permissão

Conceder permissão à fábrica de dados que precisa acessar o runtime de integração autoalojado que criou e registado.

> [!IMPORTANT]
> Não ignore este passo!

```powershell
New-AzureRMRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId ` #MSI of the Data Factory with which it needs to be shared
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' ` #This is the Contributor role
    -Scope $SharedIR.Id
```

## <a name="create-a-linked-self-hosted-integration-runtime"></a>Criar um runtime de integração autoalojado ligado

Execute o seguinte comando para criar um runtime de integração autoalojado ligado:

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $LinkedDataFactoryName `
    -Name $LinkedIntegrationRuntimeName `
    -Type SelfHosted `
    -SharedIntegrationRuntimeResourceId $SharedIR.Id `
    -Description $LinkedIntegrationRuntimeDescription
```

Agora, pode utilizar este runtime de integração ligado em qualquer serviço ligado. O runtime de integração ligado está a utilizar o runtime de integração partilhado para executar atividades.

## <a name="revoke-integration-runtime-sharing-from-a-data-factory"></a>Revogar o runtime de integração de partilha de uma fábrica de dados

Para revogar o acesso de uma fábrica de dados de acessar o runtime de integração partilhado, pode executar o seguinte comando:

```powershell
Remove-AzureRMRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId `
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' `
    -Scope $SharedIR.Id
```

Para remover o runtime de integração ligado existente, pode executar o seguinte comando contra o runtime de integração partilhado:

```powershell
Remove-AzureRmDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Links `
    -LinkedDataFactoryName $LinkedDataFactoryName
```

## <a name="next-steps"></a>Passos Seguintes

- Reveja conceitos de runtime de integração no [Integration runtime no Azure Data Factory](concepts-integration-runtime.md).

- Saiba como criar um runtime de integração autoalojado no portal do Azure no [criar e configurar um runtime de integração autoalojado](create-self-hosted-integration-runtime.md).
