---
title: Alojamento de alta densidade no serviço de aplicações do Azure com o dimensionamento por aplicação | Documentos da Microsoft
description: Alojamento de alta densidade no App Service do Azure
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: byvinyal
ms.openlocfilehash: f2cf472ef3c2c9950dd9f9382009e21fbf62771b
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856790"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Alojamento de alta densidade no serviço de aplicações do Azure com o dimensionamento por aplicação
Por predefinição, dimensionar aplicações de serviço de aplicações, tem de dimensionar os [plano do App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) que são executados. Quando várias aplicações são executadas no mesmo plano de serviço de aplicações, cada instância de escalamento horizontal é executada todas as aplicações no plano.

Pode habilitar *dimensionamento por aplicação* no serviço de aplicações planear nível. Dimensiona-se uma aplicação, independentemente do plano de serviço de aplicações que o hospeda. Dessa forma, um plano do serviço de aplicações pode ser dimensionado até 10 instâncias, mas uma aplicação pode ser definida para utilizar apenas cinco.

> [!NOTE]
> Dimensionar por aplicação só está disponível para **padrão**, **Premium**, **Premium V2** e **Isolated** escalões de preço.
>

## <a name="per-app-scaling-using-powershell"></a>Por aplicação de dimensionamento com o PowerShell

Criar um plano com o dimensionamento, passando por aplicação a ```-PerSiteScaling $true``` parâmetro para o ```New-AzureRmAppServicePlan``` cmdlet.

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Ativar o dimensionamento com um plano de serviço de aplicações existentes, passando por aplicação a `-PerSiteScaling $true` parâmetro para o ```Set-AzureRmAppServicePlan``` cmdlet.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

Ao nível da aplicação, configure o número de instâncias que a aplicação pode utilizar no plano do serviço de aplicações.

No exemplo abaixo, a aplicação está limitada a duas instâncias, independentemente de quantas instâncias do plano de serviço de aplicações subjacente aumenta horizontalmente para.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` é diferente do `$newapp.MaxNumberOfWorkers`. Utilizações de dimensionamento por aplicação `$newapp.SiteConfig.NumberOfWorkers` para determinar as características de dimensionamento da aplicação.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Por aplicação de dimensionamento com o Azure Resource Manager

Cria o modelo Azure Resource Manager seguinte:

- Um plano de serviço de aplicações é dimensionado para 10 instâncias
- uma aplicação que está configurada para escalar até um máximo de cinco instâncias.

O plano do serviço de aplicações é a configuração do **PerSiteScaling** propriedade como true `"perSiteScaling": true`. A aplicação é a configuração do **número de workers** a utilizar para 5 `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Configuração recomendada para o alojamento de alta densidade
Por aplicação dimensionamento é uma funcionalidade que está ativada em ambas as regiões do Azure global e [ambientes de serviço de aplicações](environment/app-service-app-service-environment-intro.md). No entanto, a estratégia recomendada é utilizar ambientes do serviço de aplicações para tirar partido das respetivas funcionalidades avançadas e os maiores conjuntos de capacidade.  

Siga estes passos para configurar de alta densidade para as suas aplicações de alojamento:

1. Configurar o ambiente de serviço de aplicações e escolha um conjunto de trabalho dedicado para o cenário de alojamento de alta densidade.
1. Criar um único plano do serviço de aplicações e dimensioná-lo para utilizar a capacidade disponível no conjunto de trabalho.
1. Definir o `PerSiteScaling` sinalizador como true no plano de serviço de aplicações.
1. Novas aplicações são criadas e atribuídas a esse plano de serviço de aplicações com o **numberOfWorkers** definida como **1**. Utilizando esta configuração produz a mais alta densidade possível neste conjunto de trabalho.
1. O número de trabalhadores pode ser configurado de forma independente por aplicação para conceder recursos adicionais, conforme necessário. Por exemplo:
    - Pode definir uma aplicação de uso intenso **numberOfWorkers** ao **3** tenham maior capacidade de processamento para essa aplicação. 
    - Aplicações de baixa utilização definiria **numberOfWorkers** ao **1**.

## <a name="next-steps"></a>Próximos Passos

- [Descrição geral aprofundada dos planos do serviço de aplicações do Azure](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Introdução ao Ambiente do Serviço de Aplicações](environment/app-service-app-service-environment-intro.md)
