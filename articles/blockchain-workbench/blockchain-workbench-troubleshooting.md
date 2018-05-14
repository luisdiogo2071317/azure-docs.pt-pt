---
title: Azure Blockchain Workbench, resolução de problemas
description: Como resolver problemas de uma aplicação do Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 8a2715666c4fff490f5184b7b8719b412952b9bf
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench, resolução de problemas

Um script do PowerShell está disponível para ajudá-lo com o Programador de depuração ou suportar. O script gera um resumo e recolhe registos detalhados para resolução de problemas. Registos recolhidos incluem:

* Rede de Blockchain, tais como Ethereum
* Micro-serviços Blockchain Workbench
* Application Insights
* Monitorização (OMS) do Azure

Pode utilizar as informações para determinar os passos seguintes e determinar a causa raiz dos problemas. 

## <a name="troubleshooting-script"></a>Script de resolução de problemas

O script de resolução de problemas do PowerShell está disponível no GitHub. [Transfira um ficheiro zip](https://github.com/Azure-Samples/blockchain/archive/master.zip) ou clone o exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Execute o script
[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

Execute o `collectBlockchainWorkbenchTroubleshooting.ps1` script para recolher os registos e criar um ficheiro ZIP que contém uma pasta de informações de resolução de problemas. Por exemplo:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
O script aceita os seguintes parâmetros:

| Parâmetro  | Descrição | Necessário |
|---------|---------|----|
| SubscriptionID | ID de subscrição para criar ou localizar todos os recursos. | Sim |
| ResourceGroupName | Nome do grupo de recursos do Azure onde tiver sido implementada Blockchain Workbench. | Sim |
| OutputDirectory | Caminho para criar a saída. Ficheiro ZIP. Se não for especificado, será assumida a diretório atual. | Não
| OmsSubscriptionId | O id de subscrição em que é implementado OMS. Passe apenas este parâmetro se não for implementada OMS para a rede blockchain fora do grupo de recursos do Blockchain Workbench.| Não |
| OmsResourceGroup |O grupo de recursos em que é implementado OMS. Passe apenas este parâmetro se não for implementada OMS para a rede blockchain fora do grupo de recursos do Blockchain Workbench.| Não |
| OmsWorkspaceName | O nome de área de trabalho do OMS. Passar apenas este parâmetro se não for implementada OMS para a rede blockchain fora do grupo de recursos do Blockchain Workbench | Não |

## <a name="what-is-collected"></a>O que é recolhido?

O ficheiro ZIP de saída contém a seguinte estrutura de pasta:

| Pasta \ ficheiro | Descrição  |
|---------|---------|
| \Summary.txt | Resumo do sistema |
| \metrics\blockchain | Métricas sobre o blockchain |
| \metrics\workbench | Métricas sobre o workbench |
| \details\blockchain | Registos detalhados sobre o blockchain |
| \details\workbench | Registos detalhados sobre o workbench |

O ficheiro de resumo dá-lhe um instantâneo do Estado de funcionamento da aplicação e o estado geral da aplicação. O resumo fornece as ações recomendadas, realça os erros superiores e os metadados sobre como executar os serviços.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Arquitetura de Blockchain Workbench do Azure](blockchain-workbench-architecture.md)