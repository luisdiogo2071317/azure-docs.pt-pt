---
title: Criar um balanceador de carga interno – modelo do Azure
titlesuffix: Azure Load Balancer
description: Saiba como criar um balanceador de carga interno com um modelo no Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: d029de11c7101e961506f1fa46f5a71c6efb103e
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54044013"
---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Criar um balanceador de carga interno com um modelo

> [!div class="op_single_selector"]
> * [Portal do Azure](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [CLI do Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Modelo](../load-balancer/load-balancer-get-started-ilb-arm-template.md)


[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Implementar o modelo com a função clique para implementar

O modelo de exemplo disponível no repositório público utiliza um ficheiro de parâmetros com os valores predefinidos utilizados para gerar o cenário descrito acima. Para implementar este modelo com a função clique para implementar, siga [esta ligação](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), clique em **Implementar no Azure**, substitua os valores de parâmetro predefinidos, se necessário, e siga as instruções apresentadas no portal.

## <a name="deploy-the-template-by-using-powershell"></a>Implementar o modelo com o PowerShell

Para implementar o modelo que transferiu com o PowerShell, siga os passos abaixo.

1. Se nunca tiver utilizado o Azure PowerShell, veja [How to Install and Configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview) e siga as instruções até ao fim para iniciar sessão no Azure e selecionar a sua subscrição.
2. Transfira o ficheiro de parâmetros para o disco local.
3. Edite o ficheiro e guarde-o.
4. Execute o cmdlet **New-AzureRmResourceGroupDeployment** para criar um grupo de recursos com o modelo.

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Implementar o modelo com a CLI do Azure

Para implementar o modelo com a CLI do Azure, siga os passos abaixo.

1. Se nunca tiver utilizado a CLI do Azure, veja [Install and Configure the Azure CLI (Instalar e Configurar a CLI do Azure)](../cli-install-nodejs.md) e siga as instruções até ao ponto onde poderá selecionar a sua conta e subscrição do Azure.
2. Execute o comando **azure config mode** para mudar para o modo Resource Manager, como mostrado abaixo.

    ```azurecli
    azure config mode arm
    ```

    O resultado esperado para o comando acima é o seguinte:

        info:    New mode is arm

3. Abra o ficheiro de parâmetros, selecione o conteúdo e guarde-o num ficheiro no seu computador. Para este exemplo, guardámos o ficheiro de parâmetros em *parameters.json*.
4. Execute o comando **azure group deployment create** para implementar o novo balanceador de carga interno com o modelo e os ficheiros de parâmetros que transferiu e alterou acima. A lista apresentada depois do resultado explica os parâmetros utilizados.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Passos Seguintes

[Configurar um modo de distribuição de balanceador de carga com a afinidade do IP de origem](load-balancer-distribution-mode.md)

[Configurar definições de tempo limite TCP inativo para o balanceador de carga](load-balancer-tcp-idle-timeout.md)

Para a sintaxe JSON e propriedades de um balanceador de carga num modelo, consulte [Network/loadbalancers](/azure/templates/microsoft.network/loadbalancers).
