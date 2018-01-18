---
title: "Clonagem de aplicação de Web com o PowerShell"
description: "Saiba como clonar das suas aplicações Web novas aplicações da Web através do PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.openlocfilehash: 30817a1a6a8079e7a896305ab0b59e48fad4d644
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplicação do App Service do Azure através do PowerShell de clonagem
Com o lançamento do Microsoft Azure PowerShell versão 1.1.0, foi adicionada uma nova opção para `New-AzureRMWebApp` que lhe permite clonar uma aplicação Web existente para uma aplicação recentemente criada numa região diferente ou na mesma região. Esta opção permite aos clientes implementar com um número de aplicações em diferentes regiões forma rápida e fácil.

Aplicação da clonagem está atualmente só é suportada para planos de serviço de aplicações de escalão premium. A nova funcionalidade utiliza as mesmas limitações, como a funcionalidade de cópia de segurança do Web Apps, consulte [cópia de segurança numa aplicação web no App Service do Azure](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>A clonagem de uma aplicação existente
Cenário: Uma aplicação web existente no Sul Central-na região e pretender clonar o conteúdo para uma nova aplicação web na região Norte Central-nos. Pode ser conseguido utilizando a versão do Gestor de recursos do Azure do cmdlet do PowerShell para criar uma nova aplicação web com o `-SourceWebApp` opção.

Saber o nome do grupo de recursos que contém a aplicação web de origem, pode utilizar o seguinte comando do PowerShell para obter informações da aplicação de web de origem (neste caso, denominado `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Para criar um novo plano do App Service, pode utilizar `New-AzureRmAppServicePlan` comando como no exemplo seguinte

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Utilizar o `New-AzureRmWebApp` comando, pode criar a nova aplicação web na região Norte Central-nos e associá-lo para um escalão premium existente plano do App Service. Além disso, pode utilizar o mesmo grupo de recursos que a aplicação web de origem ou definir um novo grupo de recursos, conforme mostrado no comando seguinte:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

A clonagem de uma aplicação web existente, incluindo todas as ranhuras de implementação associada, tem de utilizar o `IncludeSourceWebAppSlots` parâmetro. O comando do PowerShell seguinte demonstra a utilização desse parâmetro com o `New-AzureRmWebApp` comando:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

A clonagem de uma aplicação web existente na mesma região, terá de criar um novo grupo de recursos e um novo serviço de aplicações planear na mesma região e, em seguida, utilize o seguinte comando do PowerShell para clonar a aplicação web

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>A clonagem de uma aplicação existente para um ambiente de serviço de aplicações
Cenário: Uma aplicação web existente no Sul Central-na região e pretender clonar o conteúdo para uma nova aplicação web para existente aplicação serviço de ambiente (ASE).

Saber o nome do grupo de recursos que contém a aplicação web de origem, pode utilizar o seguinte comando do PowerShell para obter informações da aplicação de web de origem (neste caso, denominado `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Saber o nome do ASE e o nome do grupo de recursos que o ASE pertence, pode criar a nova aplicação web num ASE existente, conforme mostrado no comando seguinte:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

O `Location` parâmetro é necessário devido à razão legado, mas é ignorada quando criar a aplicação está num ASE. 

## <a name="cloning-an-existing-app-slot"></a>A clonagem de uma ranhura de aplicação existente
Cenário: Que pretende clonar uma ranhura de aplicação Web existente ou uma nova aplicação Web ou uma nova ranhura de aplicação Web. A nova aplicação Web pode ser na mesma região que a ranhura de aplicação Web original ou numa região diferente.

Saber o nome do grupo de recursos que contém a aplicação web de origem, pode utilizar o seguinte comando do PowerShell para obter informações da origem ranhura de aplicação web (neste caso, denominado `source-webappslot`) associado à aplicação Web `source-webapp`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

O comando seguinte mostra como criar um clone da aplicação de web de origem para uma nova aplicação web:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Configurar o Gestor de tráfego durante a clonagem de uma aplicação
Criar aplicações web de multirregião e configurar o Gestor de tráfego do Azure para encaminhar o tráfego para todas estas aplicações web, é um cenário importante para garantir que as aplicações de clientes são de elevada disponibilidade. Ao clonar uma aplicação web existente, tem a opção para ligar a ambas as aplicações web para um novo perfil de Gestor de tráfego ou uma existente. Apenas Azure Resource Manager a versão do Gestor de tráfego é suportada.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Criar um novo perfil de Gestor de tráfego durante a clonagem de uma aplicação
Cenário: Que pretende clonar uma aplicação web noutra região, ao configurar um perfil do Gestor de tráfego do Azure Resource Manager que inclui ambas as aplicações web. O comando seguinte mostra como criar um clone da aplicação de web de origem para uma nova aplicação web ao configurar um novo perfil de Gestor de tráfego:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Adicionar novos clonar a aplicação Web para um perfil do Traffic Manager existente
Cenário: Já tiver um perfil de Gestor de tráfego do Azure Resource Manager e pretender adicionar ambas as aplicações web, como pontos finais. Para fazê, terá primeiro de Monte o tráfego existente perfil do Gestor de ID. Terá do ID de subscrição, o nome do grupo de recursos e o nome de perfil de Gestor de tráfego existente.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Depois de ter o ID do tráfego do Configuration Manager, o comando seguinte mostra como criar um clone da aplicação de web de origem para uma nova aplicação web ao adicioná-las para um perfil do Traffic Manager existente:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Restrições atuais
Esta funcionalidade está atualmente em pré-visualização e novas capacidades são adicionadas ao longo do tempo. Seguem-se as restrições conhecidas na versão atual da clonagem de aplicação:

* As definições de dimensionamento automático não são Clonadas
* Definições da agenda de cópia de segurança não são Clonadas
* Definições da VNET não são Clonadas
* Informações de aplicação não são automaticamente configuradas na aplicação de web de destino
* As definições de autenticação fácil não são Clonadas
* Extensão do kudu não são Clonadas
* Regras de sugestão não são Clonadas
* Conteúdo da base de dados não está a ser clonado
* Alterações de saída endereços IP, se a clonagem, para uma unidade de escala diferentes

### <a name="references"></a>Referências
* [A clonagem de aplicação Web](app-service-web-app-cloning.md)
* [Cópia de segurança de uma aplicação web no App Service do Azure](web-sites-backup.md)
* [Suporte de Gestor de recursos do Azure para a pré-visualização do Traffic Manager do Azure](../traffic-manager/traffic-manager-powershell-arm.md)
* [Introdução ao Ambiente do Serviço de Aplicações](environment/intro.md)
* [Utilizar o Azure PowerShell com o Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

