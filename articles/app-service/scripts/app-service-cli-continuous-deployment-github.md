---
title: Script da CLI do Azure de exemplo - criar aplicação com implementação contínua a partir do GitHub | Documentos da Microsoft
description: Exemplo do Script da CLI do Azure - criar uma aplicação web e implementar continuamente a partir do GitHub
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 032a866b39e51ebc130104797d9d4276cf19062a
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342638"
---
# <a name="create-a-web-app-with-continuous-deployment-from-github-using-cli"></a>Criar uma aplicação web com implementação contínua a partir do GitHub com a CLI

Este script de exemplo cria uma aplicação Web no Serviço de Aplicações com os respetivos recursos relacionados e, em seguida, configura a implementação contínua a partir de um repositório do GitHub. Para a implementação do GitHub sem implementação contínua, veja [Criar uma aplicação Web e implementar código a partir do GitHub](app-service-cli-deploy-github.md). Neste exemplo, precisa de:

* Um repositório do GitHub com o código da aplicação, para o qual tem permissões administrativas.
* Um [Token de Acesso Pessoal (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) para a sua conta do GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, precisa da versão 2.0 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Cria um plano do Serviço de Aplicações. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Cria uma aplicação Web do Azure. |
| [`az webapp deployment source config`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config) | Associa uma aplicação Web do Azure a um repositório do Git ou do Mercurial. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI do Serviço de Aplicações adicionais na [documentação do Serviço de Aplicações do Azure](../app-service-cli-samples.md).
