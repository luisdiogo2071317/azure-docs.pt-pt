---
title: "Reconfigurar um tempo de execução de integração do Azure-SSIS | Microsoft Docs"
description: "Saiba como reconfigurar um tempo de execução de integração do Azure-SSIS no Azure Data Factory, após já ter aprovisionado."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: spelluru
ms.openlocfilehash: c1743a0d06f911122ed0aba586aec837f81c578c
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="manage-an-azure-ssis-integration-runtime"></a>Gerir um tempo de execução de integração do Azure-SSIS
O [criar um tempo de execução de integração do Azure-SSIS](create-azure-ssis-integration-runtime.md) artigo mostra-lhe como criar um tempo de execução de integração do Azure-SSIS (IR) utilizando o Azure Data Factory. Este artigo fornece informações sobre a necessidade de reconfigurar um tempo de execução de integração de SSIS do Azure existente.  

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja [Data Factory version 1 documentation](v1/data-factory-introduction.md) (Documentação da versão 1 do Data Factory).

Depois de aprovisionar e iniciar uma instância do tempo de execução de integração de SSIS do Azure, pode reconfigurá-la ao executar uma sequência de `Stop`  -  `Set`  -  `Start` cmdlets do PowerShell consecutivamente. Por exemplo, o script do PowerShell seguinte altera o número de nós atribuídos para a instância de tempo de execução de integração do Azure-SSIS a cinco.

## <a name="reconfigure-an-azure-ssis-ir"></a>Reconfigurar uma resposta a incidentes SSIS do Azure

1. Em primeiro lugar, pare o tempo de execução de integração de SSIS do Azure utilizando o [Stop-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. Este comando disponibiliza todos os respetivos nós e deixa de faturação.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Em seguida, reconfigurar a resposta a incidentes SSIS do Azure utilizando o [conjunto AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. O comando de exemplo seguintes dimensiona terminar um integração do Azure-SSIS runtime cinco nós.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Em seguida, inicie o tempo de execução de integração de SSIS do Azure utilizando o [início AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. Este comando aloca todos os respetivos nós para a execução de pacotes SSIS.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

## <a name="delete-an-azure-ssis-ir"></a>Eliminar uma resposta a incidentes SSIS do Azure
1. Em primeiro lugar, liste todos os existente IRs de SSIS Azure sob a fábrica de dados.

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Em seguida, pare todos os IRs de SSIS do Azure existente na sua fábrica de dados.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Em seguida, remova todos os IRs de SSIS do Azure existente na sua fábrica de dados um por um.

    ```powershell
    Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Por fim, remova a fábrica de dados.

    ```powershell
    Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Se criou um novo grupo de recursos, remova o grupo de recursos.

    ```powershell
    Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Passos Seguintes
Para mais informações sobre o tempo de execução de SSIS do Azure, consulte os tópicos seguintes: 

- [Tempo de execução do Azure-SSIS integração](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações concetuais sobre tempos de execução de integração em geral, incluindo o IR. SSIS do Azure 
- [Tutorial: implementar pacotes do SSIS no Azure](tutorial-deploy-ssis-packages-azure.md). Este artigo disponibiliza instruções passo a passo para criar um IR Azure-SSIS e utiliza uma base de dados da Base de Dados SQL do Azure para alojar o catálogo do SSIS. 
- [How to: Create an Azure-SSIS integration runtime](create-azure-ssis-integration-runtime.md) (Como criar um runtime de integração do Azure-SSIS). Este artigo é uma continuação do tutorial e mostra instruções sobre como utilizar a Instância Gerida do SQL do Azure (pré-visualização privada) e associar o IR a uma VNet. 
- [Join an Azure-SSIS IR to a VNet](join-azure-ssis-integration-runtime-virtual-network.md) (Associar um IR Azure-SSIS a uma VNet). Este artigo disponibiliza informações concetuais sobre como associar um IR Azure-SSIS a uma rede virtual do Azure (VNet). Também descreve os passos para utilizar o portal do Azure para configurar a VNet, de modo a que o IR Azure-SSIS se possa associar à mesma. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Monitorizar um IR Azure-SSIS) Este artigo mostra como obter informações sobre um IR Azure-SSIS e descrições dos estados nas informações devolvidas. 
 
