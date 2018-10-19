---
title: O Azure Blockchain Workbench, resolução de problemas
description: Como resolver problemas de uma aplicação do Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e205fce8b718e68200face33447e37cd3317298f
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405489"
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>O Azure Blockchain Workbench, resolução de problemas

Um script do PowerShell está disponível para ajudá-lo em desenvolvedor depuração ou de suporte. O script gera um resumo e recolhe registos detalhados para resolução de problemas. Registos recolhidos incluem:

* Rede de Blockchain, como Ethereum
* Microsserviços de Blockchain Workbench
* Application Insights
* Monitorização (Log Analytics) do Azure

Pode utilizar as informações para determinar os passos seguintes e determinar a causa raiz dos problemas. 

## <a name="troubleshooting-script"></a>Script de resolução de problemas

O resolução de problemas de script de PowerShell está disponível no GitHub. [Transfira um ficheiro zip](https://github.com/Azure-Samples/blockchain/archive/master.zip) ou clone o exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Executar o script
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Execute o `collectBlockchainWorkbenchTroubleshooting.ps1` script para recolher registos e crie um ficheiro ZIP que contém uma pasta de informações de resolução de problemas. Por exemplo:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
O script aceita os seguintes parâmetros:

| Parâmetro  | Descrição | Necessário |
|---------|---------|----|
| subscriptionID | SubscriptionID para criar ou localizar todos os recursos. | Sim |
| ResourceGroupName | Nome do grupo de recursos do Azure onde tiver sido implementada Blockchain Workbench. | Sim |
| OutputDirectory | Caminho para criar a saída. Ficheiro ZIP. Se não for especificado, por predefinição, o diretório atual. | Não |
| LookbackHours | Número de horas para utilizar quando extrair telemetria. Valor predefinido é de 24 horas. O valor máximo é de 90 horas | Não |
| OmsSubscriptionId | O ID de subscrição em que o Log Analytics é implementado. Apenas transmita este parâmetro se o Log Analytics para a rede de blockchain é implementado fora do grupo de recursos de Blockchain Workbench.| Não |
| OmsResourceGroup |O grupo de recursos em que o Log Analytics é implementado. Apenas transmita este parâmetro se o Log Analytics para a rede de blockchain é implementado fora do grupo de recursos de Blockchain Workbench.| Não |
| OmsWorkspaceName | O nome de área de trabalho do Log Analytics. Apenas transmitir este parâmetro se o Log Analytics para a rede de blockchain é implementado fora do grupo de recursos de Blockchain Workbench | Não |

## <a name="what-is-collected"></a>O que é recolhido?

O ficheiro ZIP contém a seguinte estrutura de pastas:

| Ficheiro ou pasta | Descrição  |
|---------|---------|
| \Summary.txt | Resumo do sistema |
| \Metrics\blockchain | Métricas sobre blockchain |
| \Metrics\Workbench | Métricas sobre a Bancada de trabalho |
| \Details\Blockchain | Registos detalhados sobre blockchain |
| \Details\Workbench | Registos detalhados sobre a Bancada de trabalho |

O ficheiro de resumo fornece um instantâneo do Estado de funcionamento da aplicação e o estado geral do aplicativo. O resumo fornece as ações recomendadas, realça os erros principais e metadados sobre a execução de serviços.

O **métricas** pasta contém métricas de vários componentes do sistema ao longo do tempo. Por exemplo, o ficheiro de saída `\Details\Workbench\apiMetrics.txt` contém um resumo dos códigos de resposta diferente e tempos de resposta durante o período de coleção. O **detalhes** pasta contém registos detalhados para resolução de problemas específicos com a Bancada de trabalho ou da rede subjacente do blockchain. Por exemplo, `\Details\Workbench\Exceptions.csv` contém uma lista das exceções mais recentes que ocorreram no sistema, que é útil para resolução de problemas de erros com contratos inteligentes ou as interações com o blockchain. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Arquitetura do Azure Blockchain Workbench](architecture.md)
