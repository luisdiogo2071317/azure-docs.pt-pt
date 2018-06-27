---
title: Alojamento de alta densidade em utilização por aplicações de dimensionamento de App Service do Azure | Microsoft Docs
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
ms.openlocfilehash: 97e1efe34417c3bf2f23801b2112b718f55d3416
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961949"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Alojamento de alta densidade em utilização por aplicações de dimensionamento de App Service do Azure
Por predefinição, pode dimensionar a aplicações do App Service ao aumentar a [plano do App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) são executados nos. Quando várias aplicações são executadas no mesmo plano de serviço de aplicações, cada instância de escalamento horizontal é executada todas as aplicações no plano.

Pode ativar *por aplicação dimensionamento* no App Service planear nível. Dimensiona de uma aplicação independentemente do plano de serviço de aplicações que aloja-lo. Desta forma, um plano do App Service pode ser escalado para 10 instâncias, mas uma aplicação pode ser definida para utilizar apenas cinco.

> [!NOTE]
> Dimensionamento por aplicação só está disponível para **padrão**, **Premium**, **Premium V2** e **Isolated** escalões de preço.
>

## <a name="per-app-scaling-using-powershell"></a>Por aplicação dimensionamento com o PowerShell

Criar um plano com dimensionamento mediante a transmissão por aplicação o ```-perSiteScaling $true``` atributo para o ```New-AzureRmAppServicePlan``` commandlet

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Para atualizar um plano de serviço de aplicações existente com dimensionamento por aplicação: 

- obter o plano de destino ```Get-AzureRmAppServicePlan```
- modificar a propriedade localmente ```$newASP.PerSiteScaling = $true```
- publicar as suas alterações para o azure ```Set-AzureRmAppServicePlan``` 

```powershell
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Ao nível da aplicação, configure o número de instâncias, que pode utilizar a aplicação no plano de serviço de aplicações.

No exemplo abaixo, a aplicação está limitada a duas instâncias, independentemente de quantas instâncias do plano do app service subjacente aumenta horizontalmente de forma a.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` é diferente do `$newapp.MaxNumberOfWorkers`. Utiliza dimensionamento por aplicação `$newapp.SiteConfig.NumberOfWorkers` para determinar as características de dimensionamento da aplicação.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Por aplicação dimensionamento com o Azure Resource Manager

Cria o modelo Azure Resource Manager seguinte:

- Um plano de serviço aplicacional ampliar a 10 instâncias
- uma aplicação que está configurada para dimensionar para um máximo de cinco instâncias.

O plano de serviço de aplicações é a definição de **PerSiteScaling** propriedade para verdadeiro `"perSiteScaling": true`. A aplicação é a definição de **número de funcionários** a utilizar para 5 `"properties": { "numberOfWorkers": "5" }`.

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
Por aplicação da receção é uma funcionalidade que está ativada em ambas as regiões do Azure global e [ambientes do App Service](environment/app-service-app-service-environment-intro.md). No entanto, a estratégia de recomendada é utilizar ambientes do App Service para tirar partido das respetivas funcionalidades avançadas e os agrupamentos maiores da capacidade.  

Siga estes passos para configurar alta densidade de alojamento para as suas aplicações:

1. Configurar o ambiente de serviço de aplicações e escolha um conjunto de trabalho que se encontra dedicado para o cenário de alojamento de alta densidade.
1. Criar um único plano de serviço de aplicações e dimensione-a para utilizar a capacidade disponível no conjunto de trabalho.
1. Definir o `PerSiteScaling` sinalizador como true no plano de serviço de aplicações.
1. Novas aplicações são criadas e atribuídas a essa plano de serviço de aplicações com o **numberOfWorkers** propriedade definida como **1**. Utilizando esta configuração gera a densidade mais elevada possível neste conjunto de trabalho.
1. O número de funcionários pode ser configurado de forma independente por aplicação para conceder recursos adicionais, conforme necessário. Por exemplo:
    - Pode definir uma aplicação de utilização é elevado **numberOfWorkers** para **3** com mais capacidade de processamento para essa aplicação. 
    - Baixa utilização aplicações iriam definir **numberOfWorkers** para **1**.

## <a name="next-steps"></a>Próximos Passos

- [Descrição geral dos planos do App Service do Azure](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Introdução ao Ambiente do Serviço de Aplicações](environment/app-service-app-service-environment-intro.md)
