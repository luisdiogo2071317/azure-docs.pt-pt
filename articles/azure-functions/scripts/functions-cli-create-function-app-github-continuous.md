---
title: "Criar uma função no Azure que é implementado a partir do GitHub | Microsoft Docs"
description: "Criar uma aplicação de função e implementar o código de função de um repositório do GitHub com as funções do Azure."
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: c4224bc7973cd1e3ca36799db9f23a124fcba807
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-github"></a>Criar uma função no Azure que é implementado a partir do GitHub

Este script de exemplo cria uma aplicação de função utilizando o [plano de consumo](../functions-scale.md#consumption-plan) com os respetivos recursos relacionados e implementa continuamente o seu código de função de um repositório do GitHub. Neste exemplo, é necessário:

* Um repositório do GitHub com o código de funções, que tem permissões administrativas.
* A [pessoais acesso Token (TERESA)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) para a sua conta do GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se em vez disso utilizar a CLI do Azure localmente, terá de instalar e utilizar a versão 2.0 ou uma versão posterior. Para determinar a versão da CLI do Azure, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Este exemplo cria uma aplicação de função do Azure e implementa o código de função a partir do GitHub.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação de script

Cada comando nas ligações de tabela para a documentação específica do comando. Este script utiliza os seguintes comandos:

| Comando | Notas |
|---|---|
| [Criar grupo AZ](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [criar conta de armazenamento AZ](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Cria um plano de Serviço de Aplicações. |
| [Criar AZ functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [configuração de controlo de origem do AZ serviço de aplicações web](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Associa uma aplicação de função um Git ou Mercurial repositório. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, consulte [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Exemplos de script CLI de funções do Azure adicionais podem ser encontrados no [documentação de funções do Azure](../functions-cli-samples.md).
