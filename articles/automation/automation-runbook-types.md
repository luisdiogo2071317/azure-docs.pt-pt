---
title: Tipos de Runbook da automatização do Azure
description: 'Descreve os diferentes tipos de runbooks que podem ser usados na automatização do Azure e considerações que deve levar em conta ao determinar que tipo para usar. '
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 572d887a14aa25e45f0ad74660c5d3fbb1d32851
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425633"
---
# <a name="azure-automation-runbook-types"></a>Tipos de runbook da automatização do Azure

A automatização do Azure suporta vários tipos de runbooks que são descritos brevemente na seguinte tabela.  As seções abaixo fornecem informações adicionais sobre cada tipo, inclusive considerações sobre quando usar cada um.

| Tipo | Descrição |
|:--- |:--- |
| [Gráficos](#graphical-runbooks) |Com base no Windows PowerShell e criado e editado completamente no editor gráfico no portal do Azure. |
| [Fluxo de trabalho de PowerShell gráfico](#graphical-runbooks) |Com base no fluxo de trabalho do Windows PowerShell e criados e editados completamente no editor gráfico no portal do Azure. |
| [PowerShell](#powershell-runbooks) |Runbook de texto com base no script do Windows PowerShell. |
| [Fluxo de Trabalho do PowerShell](#powershell-workflow-runbooks) |Runbook de texto com base no fluxo de trabalho do Windows PowerShell. |
| [Python](#python-runbooks) |Runbook de texto com base em Python. |

## <a name="graphical-runbooks"></a>Runbooks gráficos

[Gráfico](automation-runbook-types.md#graphical-runbooks) e runbooks de fluxo de trabalho de PowerShell gráfico são criados e editados com o editor gráfico no portal do Azure.  Pode exportá-los para um ficheiro e, em seguida, importá-los para outra conta de automatização, mas não é possível criar ou editá-los com outra ferramenta.  Os runbooks gráficos gerar código do PowerShell, mas não pode ver ou modificar o código diretamente. Não é possível converter um dos runbooks gráficos da [formatos de texto](automation-runbook-types.md), nem um runbook de texto pode ser convertido para o formato gráfico. Os runbooks gráficos podem ser convertidos para runbooks de fluxo de trabalho de PowerShell gráfico durante a importação e vice-versa.

### <a name="advantages"></a>Vantagens

* Visual modelo de criação insert-link-configurar  
* Se concentrar em como os dados fluem através do processo  
* Representar visualmente processos de gestão  
* Incluir outros runbooks como runbooks subordinados para criar fluxos de trabalho de alto nível  
* Incentiva a programação modular  

### <a name="limitations"></a>Limitações

* Não é possível editar o runbook fora do portal do Azure.
* Pode exigir uma atividade de código que contém o código do PowerShell para realizar lógica complexa.
* Não é possível ver ou editar diretamente o código do PowerShell que é criado pelo fluxo de trabalho gráfico. Tenha em atenção que pode ver o código que cria no quaisquer atividades de código.

## <a name="powershell-runbooks"></a>Runbooks do PowerShell

Os runbooks do PowerShell são baseados no Windows PowerShell.  Editar diretamente o código do runbook com o editor de texto no portal do Azure.  Também pode utilizar qualquer editor de texto offline e [importe o runbook](automation-creating-importing-runbook.md) para a automatização do Azure.

### <a name="advantages"></a>Vantagens

* Implemente toda a lógica complexa com código do PowerShell sem as complexidades adicionais de fluxo de trabalho do PowerShell.
* Runbook inicia-se mais rapidamente do que os runbooks do fluxo de trabalho do PowerShell, uma vez que ele não precisa ser compilado antes de executar.

### <a name="limitations"></a>Limitações

* Deve estar familiarizado com os scripts do PowerShell.
* Não é possível utilizar [processamento paralelo](automation-powershell-workflow.md#parallel-processing) para efetuar várias ações em paralelo.
* Não é possível utilizar [pontos de verificação](automation-powershell-workflow.md#checkpoints) para retomar o runbook em caso de erro.
* Runbooks de fluxo de trabalho do PowerShell e runbooks gráficos só pode ser incluídos como runbooks subordinados com o cmdlet Start-AzureAutomationRunbook que cria uma nova tarefa.

### <a name="known-issues"></a>Problemas Conhecidos

Seguem-se atuais problemas conhecidos com runbooks do PowerShell.

* Os runbooks do PowerShell não é possível obter um não encriptado [recurso de variável](automation-variables.md) com um valor nulo.
* Os runbooks do PowerShell não é possível obter um [recurso de variável](automation-variables.md) com *~* no nome.
* Get-Process num loop no PowerShell runbook pode falhar após cerca de 80 iterações.
* Um runbook do PowerShell poderá falhar se ele tenta gravar uma grande quantidade de dados para o fluxo de saída de uma só vez.   Normalmente, pode contornar este problema por meio da geração apenas as informações que necessárias ao trabalhar com objetos grandes.  Por exemplo, em vez de saída semelhante *Get-Process*, o utilizador pode apresentar apenas os campos obrigatórios com *Get-Process | Selecione ProcessName, CPU*.

## <a name="powershell-workflow-runbooks"></a>Runbooks de fluxo de trabalho do PowerShell

Runbooks de fluxo de trabalho do PowerShell são runbooks de texto com base na [fluxo de trabalho do Windows PowerShell](automation-powershell-workflow.md).  Editar diretamente o código do runbook com o editor de texto no portal do Azure.  Também pode utilizar qualquer editor de texto offline e [importe o runbook](automation-creating-importing-runbook.md) para a automatização do Azure.

### <a name="advantages"></a>Vantagens

* Implemente toda a lógica complexa com o código de fluxo de trabalho do PowerShell.
* Uso [pontos de verificação](automation-powershell-workflow.md#checkpoints) para retomar o runbook em caso de erro.
* Uso [processamento paralelo](automation-powershell-workflow.md#parallel-processing) para efetuar várias ações em paralelo.
* Pode incluir outros runbooks gráficos e runbooks de fluxo de trabalho do PowerShell como runbooks subordinados para criar fluxos de trabalho de alto nível.

### <a name="limitations"></a>Limitações

* Autor deve estar familiarizado com o fluxo de trabalho do PowerShell.
* Runbook tem de lidar com a complexidade adicional de fluxo de trabalho do PowerShell, tal como [anular a serialização de objetos](automation-powershell-workflow.md#code-changes).
* Demora mais tempo a iniciar-se que os runbooks do PowerShell, uma vez que ele precisa ser compilado antes de executar o Runbook.
* Os runbooks do PowerShell só pode ser incluídos como runbooks subordinados com o cmdlet Start-AzureAutomationRunbook que cria uma nova tarefa.

## <a name="python-runbooks"></a>Runbooks de Python

Runbooks de Python compiladas em Python 2.  Pode editar diretamente o código do runbook com o editor de texto no portal do Azure ou pode utilizar qualquer editor de texto offline e [importe o runbook](automation-creating-importing-runbook.md) para a automatização do Azure.

### <a name="advantages"></a>Vantagens

* Utilize as bibliotecas Python robustas.

### <a name="limitations"></a>Limitações

* Deve estar familiarizado com os scripts do Python.
* Apenas de Python 2 é suportado no momento, o que significa funções específicas de Python 3 irão falhar.
* Para utilizar bibliotecas de terceiros, deve [importar o pacote](python-packages.md) para a conta de automatização para o mesmo a ser utilizado.

## <a name="considerations"></a>Considerações

Deve levar em conta as seguintes considerações adicionais ao determinar que tipo para usar para um determinado runbook.

* Não é possível converter os runbooks do gráfico para o tipo de texto ou vice-versa.
* Existem limitações através de runbooks de diferentes tipos como um runbook subordinado.  Ver [runbooks subordinados na automatização do Azure](automation-child-runbooks.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a criação de runbooks gráficos, veja [a criação de gráficos na automatização do Azure](automation-graphical-authoring-intro.md)
* Para compreender as diferenças entre o PowerShell e o PowerShell fluxos de trabalho para runbooks, consulte [o fluxo de trabalho do Learning Windows PowerShell](automation-powershell-workflow.md)
* Para obter mais informações sobre como criar ou importar um Runbook, consulte [criar ou importar um Runbook](automation-creating-importing-runbook.md)

