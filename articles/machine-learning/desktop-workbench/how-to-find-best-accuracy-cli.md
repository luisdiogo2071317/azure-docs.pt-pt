---
title: Localizar execuções com o maior precisão e a duração mais baixa no Azure Machine Learning Workbench | Documentos da Microsoft
description: Um caso de utilização de ponto-a-ponto para localizar o maior precisão através da CLI com o Azure Machine Learning Workbench
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs, jmartens, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 094fd6d8c6c6d647533cf5409d1a85283c71c80e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953247"
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>Find é executado com o maior precisão e a duração mais baixa
Tendo em conta várias execuções, é um caso de uso encontrar execuções com a maior precisão. Uma abordagem é usar a interface de linha de comandos (CLI), com um [JMESPath](http://jmespath.org/) consulta. Para obter mais informações sobre como utilizar o JMESPath na CLI do Azure, consulte [consultas JMESPath utilizar com a CLI do Azure](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). No exemplo seguinte, quatro execuções são criadas com valores de precisão de 0, 0.98, 1 e 1. Execuções são filtradas se eles estão no intervalo `[MaxAccuracy-Threshold, MaxAccuracy]` onde `Threshold = .03`.

## <a name="sample-data"></a>Dados de exemplo
Se não tiver execuções existentes com um `Accuracy` valor, os seguintes passos geram execuções para consultar.

Em primeiro lugar, crie um ficheiro de Python no Azure Machine Learning Workbench, nomeie- `log_accuracy.py`e cole o seguinte código:
```python
from azureml.logging import get_azureml_logger

logger = get_azureml_logger()

accuracy_value = 0.5

if len(sys.argv) > 1:
     accuracy_value = float(sys.argv[1])

logger.log("Accuracy", accuracy_value)
```

Em seguida, crie um ficheiro `run.py`e cole o seguinte código:
```python
import os

accuracy_values = [0, 0.98, 1.0, 1.0]
for value in accuracy_values:
    os.system('az ml experiment submit -c local ./log_accuracy.py {}'.format(value))
```

Por último, abra a CLI por meio da bancada de trabalho e execute o comando `python run.py` para submeter quatro experimentações. Quando o script estiver concluído, deverá ver quatro mais execuções no `Run History` separador.

## <a name="query-the-run-history"></a>Consultar o histórico de execuções
O primeiro comando localiza o valor de precisão máxima.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Com este valor de precisão máx. de `1` e um valor de limiar de `0.03`, os segundo filtros de comando é executado usando `Accuracy` e, em seguida, ordena é executado por `duration` ascendente.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Se pretender que uma verificação rigorosa do limite superior, o formato de consulta é ``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``

Se utilizar o PowerShell, o código a seguir usa variáveis locais para armazenar o limiar e a precisão máxima:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o registo, consulte [como utilizar o histórico de execuções e métricas de modelo no Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md).    
