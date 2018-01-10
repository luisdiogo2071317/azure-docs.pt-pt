---
title: "Criar uma função no Azure que é implementado a partir do Visual Studio Team Services | Microsoft Docs"
description: "Criar uma aplicação de função e implementar o código de função do Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: bf9428f23e851bae3485ec3d724dfb9ccd2af4c1
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-visual-studio-team-services"></a>Criar uma função no Azure que é implementado a partir do Visual Studio Team Services

Este tópico mostra como utilizar as funções do Azure para criar um [sem servidor](https://azure.microsoft.com/overview/serverless-computing/) através da aplicação de função a [plano de consumo](../functions-scale.md#consumption-plan). A aplicação de função, o que é um contentor para as suas funções, continuamente for implementada a partir de um repositório de serviços de equipa do Visual Studio (VSTS). Para concluir este tópico, tem de ter:

* Um repositório VSTS que contém o projeto de aplicação de função e para o qual tem permissões administrativas.
* A [token de acesso pessoal (TERESA)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) para aceder ao seu repositório VSTS.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se em vez disso utilizar a CLI do Azure localmente, terá de instalar e utilizar a versão 2.0 ou uma versão posterior. Para determinar a versão da CLI do Azure, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Este exemplo cria uma aplicação de função do Azure e implementa o código de função do Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos para criar um grupo de recursos, conta de armazenamento, aplicação de função e todos os recursos relacionados. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Criar grupo AZ](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [criar conta de armazenamento AZ](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Cria um plano de Serviço de Aplicações. |
| [Criar AZ functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [configuração de controlo de origem do AZ serviço de aplicações web](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Associa uma aplicação de função um Git ou Mercurial repositório. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, consulte [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Exemplos de script CLI de funções do Azure adicionais podem ser encontrados no [documentação de funções do Azure](../functions-cli-samples.md).
