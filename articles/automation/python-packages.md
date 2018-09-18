---
title: Gerir pacotes de Python 2 na automatização do Azure
description: Este artigo descreve como gerir pacotes de Python 2 na automatização do Azure.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5974a8e622ca0969b2a7b5ee9500766ac95398c9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986295"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Gerir pacotes de Python 2 na automatização do Azure

A automatização do Azure permite-lhe executar runbooks de Python 2 no Azure e sobre os Runbook Workers híbridos de Linux. Para ajudar na simplificação de runbooks, pode utilizar pacotes de Python para importar os módulos que precisa. Este artigo descreve como gerir e utilizar pacotes de Python na automatização do Azure.

## <a name="import-packages"></a>Pacotes de importação

Na sua conta de automatização, selecione **pacotes de Python 2** sob **recursos partilhados**. Clique em **+ adicionar um pacote de Python 2**.

![Adicionar o pacote do Python](media/python-packages/add-python-package.png)

Sobre o **Adicionar pacote do Python 2** , selecione um pacote local para carregar. O pacote pode ser um `.whl` arquivo ou `.tar.gz` ficheiro. Quando selecionado, clique em **OK** para carregar o pacote.

![Adicionar o pacote do Python](media/python-packages/upload-package.png)

Assim que tiver sido importado um pacote, estão listadas no **pacotes de Python 2** página na conta de automatização. Se tiver de remover um pacote, selecione o pacote e escolha **eliminar** na página do pacote.

![Lista de pacotes](media/python-packages/package-list.png)

## <a name="use-a-package-in-a-runbook"></a>Utilizar um pacote num runbook

Assim que tiver importado um pacote, pode usá-lo agora num runbook. O exemplo seguinte utiliza a [ pacote de utilitário de automatização do Azure](https://github.com/azureautomation/azure_automation_utility). Este pacote torna mais fácil de utilizar o Python com a automatização do Azure. Utilizar o pacote, siga as instruções no repositório do GitHub e adicioná-lo para o runbook utilizando `from azure_automation_utility import get_automation_runas_credential` por exemplo, para importar a função para a conta RunAs a obter.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Desenvolver e testar runbooks offline

Para desenvolver e testar os runbooks de Python 2 offline, pode utilizar o [python de automatização do Azure emulada ativos](https://github.com/azureautomation/python_emulated_assets) módulo no GitHub. Este módulo permite-lhe fazer referência os recursos partilhados, como credenciais, variáveis, ligações e certificados.

## <a name="next-steps"></a>Passos Seguintes

Para começar com runbooks de Python 2, consulte [meu primeiro runbook de Python 2](automation-first-runbook-textual-python2.md)