---
title: "Ver as latências relativas a regiões do Azure a partir de localizações específicas | Microsoft Docs"
description: "Saiba como ver latências relativas através de fornecedores de Internet para regiões do Azure a partir de localizações específicas."
services: network-watcher
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: a6c2ffa619eeff8b455df8a8b2157525af12c640
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Vista latência relativa a regiões do Azure a partir de localizações específicas

Neste tutorial, saiba como utilizar o Azure [observador de rede](network-watcher-monitoring-overview.md) de serviço para o ajudar a decidir a que região do Azure para implementar a aplicação ou serviço na, com base no seu utilizador demográficos. Além disso, pode utilizá-lo para ajudar a avaliar as ligações de fornecedores de serviços para o Azure.  
        
## <a name="create-a-network-watcher"></a>criar um observador de rede

Se já tiver um observador de rede no Azure, pelo menos, um [região](https://azure.microsoft.com/regions), pode ignorar as tarefas nesta secção. Crie um grupo de recursos para o observador de rede. Neste exemplo, o grupo de recursos é criado na região EUA leste, mas pode criar o grupo de recursos em qualquer região do Azure.

```powershell
New-AzureRmResourceGroup -Name NetworkWatcherRG -Location eastus
```

Crie um observador de rede. Tem de ter um observador de rede criado pelo menos uma região do Azure. Neste exemplo, é criado um observador de rede na região do Azure do Leste dos EUA.

```powershell
New-AzureRmNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Comparar as latências de rede relativo para uma única região do Azure a partir de uma localização específica

Avaliar fornecedores de serviços, ou um utilizador, tais como "o site foi lento," um problema de relatórios de resolução de problemas de uma localização específica para a região do azure em que um serviço é implementado. Por exemplo, o comando seguinte devolve as média relativas à Internet serviço fornecedor latências entre o estado de Washington nos Estados Unidos e a oeste nos 2 região do Azure entre Dezembro 13-15 de 2017:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Location "West US 2" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> A região que especificar no comando anterior não tem de ser a mesma região que especificou quando obtido o observador de rede. O comando anterior requer apenas que especificou um observador de rede existente. O observador de rede pode estar em qualquer região. Se especificar valores para `-Country` e `-State`, tem de ser válidos. Os valores são maiúsculas e minúsculas. Os dados estão disponíveis para um número limitado de países, Estados e cidades. Execute os comandos numa [ver países disponíveis, Estados, cidades e fornecedores](#view-available) para ver uma lista dos países/regiões disponíveis, cidades e Estados que pretende utilizar com o comando anterior. 

> [!WARNING]
> Tem de especificar uma data posterior a 14 de Novembro de 2017 para `-StartTime` e `-EndTime`. Especificar uma data antes de 14 de Novembro de 2017 devolve sem dados. 

Segue a saída do comando anterior:

```powershell
AggregationLevel   : State
ProviderLocation   : {
                       "Country": "United States",
                       "State": "washington"
                     }
ReachabilityReport : [
                       {
                         "Provider": "Qwest Communications Company, LLC - ASN 209",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 92
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 92
                           }
                         ]
                       },
                       {
                         "Provider": "Comcast Cable Communications, LLC - ASN 7922",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 96
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 96
                           }
                         ]
                       }
                     ]
```

Na saída devolvida, o valor para **pontuação** é a latência relativa em regiões e fornecedores. Uma pontuação de 1 é a latência (mais alta) pior, enquanto 100 é a latência mais baixa. As latências relativas são uma média para o dia. No exemplo anterior, enquanto que tem limpar que as latências os mesmos foram ambos os dias e que existe uma diferença entre a latência dos dois fornecedores pequena, também tem limpar que as latências para ambos os fornecedores são baixa na escala de 1-100. Enquanto isto é esperado, uma vez que o estado de Washington nos Estados Unidos fisicamente está próximo da oeste nos 2 região do Azure, por vezes, os resultados não estão como esperado. Quanto maior for o intervalo de datas que especificar, mais pode latência média ao longo do tempo.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Comparar as latências de rede relativo em regiões do Azure a partir de uma localização específica

Se, em vez de especificar as latências relativas entre uma localização específica e uma região do Azure específico utilizando `-Location`, quer determinar as latências relativas a todas as regiões do Azure a partir de uma localização física específica, pode fazê-lo que demasiado. Por exemplo, o comando seguinte ajuda-o a avaliar a que região do azure para implementar um serviço no se os utilizadores primários são utilizadores Comcast localizados no estado de Washington:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Provider "Comcast Cable Communications, LLC - ASN 7922" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

>[!NOTE]
Ao contrário quando especificar uma localização única, se não especificar uma localização ou especificar várias localizações, por exemplo, "Oeste US2", o "EUA Oeste", tem de especificar um fornecedor de serviços Internet quando executar o comando. 

## <a name="view-available"></a>Ver países disponíveis, Estados, cidades e fornecedores

Dados estão disponíveis para fornecedores de serviços Internet específicos, países, Estados e cidades. Para ver uma lista de todos os fornecedores de serviços Internet disponíveis, países, Estados e cidades, que pode visualizar dados, introduza o seguinte comando:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Dados estão disponíveis apenas para o países, Estados e cidades devolvidas pelo comando anterior. O comando anterior requer que especifique um observador de rede existente. O exemplo especificado o *NetworkWatcher_eastus* observador de rede num grupo de recursos com o nome *NetworkWatcherRG*, mas pode especificar qualquer observador de rede existente. Se não tiver um observador de rede existente, crie um, efetuando as tarefas [criar um observador de rede](#create-a-network-watcher). 

Depois de executar o comando anterior, pode filtrar o resultado devolvido ao especificar os valores válidos para **país**, **estado**, e **Cidade**, se assim o desejar.  Por exemplo, para ver a lista de fornecedores de serviços Internet disponíveis em Seattle, Washington, nos Estados Unidos, introduza o seguinte comando:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> O valor especificado para **país** tem de estar em maiúsculas e minúsculas. Os valores especificados para **estado** e **Cidade** tem de ser em minúsculas. Os valores tem de estar listados no resultado devolvido depois de executar o comando com valores de para **país**, **estado**, e **Cidade**. Se especificar as maiúsculas e minúsculas incorreta ou especifique um valor para **país**, **estado**, ou **Cidade** que não se encontra no resultado devolvido depois de executar o comando com nenhuma valores para estes propriedades, o resultado devolvido está vazio.
